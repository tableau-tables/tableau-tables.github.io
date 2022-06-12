# Using the Tableau Table Library to write a JavaScript Extension

Most every Markdown processor has some form of extension of plugin
mechanism. Although it is redundant to say it here, be sure to spend a
little while reading how your particular processor implements plugins.

In general, Markdown processors operate as a pipeline of modules, where
each module parses, manipulates, of generates output. 

Some processors take this further and have separate pipelines for
different stages of processing: tokenizing or parsing, handling blocks,
handling inline segments, creating indices, and generating HTML, for
example.

Tableau deals with tables. Tables are a Markdown block element, and so
in a pipeline processor you'll be adding code at the block level.

The lax syntax of Markdown makes it difficult for processors to be
certain what kind of block they are looking at: is it a table, or just a
paragraph with a pipe character? They commonly deal with this by having
extensions perform a quick sanity check (typically a regular expression
match on a block of text). If this passes, the processor can then call
the main code of the extension. Even it this point, an extension can
discover that the quick test was wrong, and that the block is not one
it handles, in which case it uses a specific return value to tell the
processor to try an alternative.

Processors also differ when in comes to generating HTML. Sometimes the
extension generates HTML when it first processes the block, and other
times it instead saves information, and the processor will call a
separate function to convert that saved information into HTML it can
use.

## The Tableau Table Library

The tableau API has three functions: 

`test(src: string): boolean`
: pass this function a string, at it returns `true` if that string
_might_ represent a tableau table, false otherwise. This is the
quick-n-nasty regular expression hack.

`to_ast(str): Ast | false`
: takes a string containing a possible tableau table, validates that it
is, and then returns a data structure describing the table contents.
This structure is described below.

`ast_to_html(ast: Ast): string`
: The `ast_to_html` function takes an AST and generates an HTML
representation. The format of this HTML is described below. This
function might not be usable if your Markdown processor requires you to
use its functions to generate HTML tags. In that case you can always
copy the overall structure of the `ast_to_html` function's code in your
code.

## The Input String

The input to both `test` and `to_ast` is a single string containing
newline-separated lines. Before being processed, the library strips
leading and trailing spaces from the string, and then combines any line
that ends with a backslash character with the next line.

## The Ast

The Ast represents the whole tab split_out_head`le. The Ast has some
table-level attributes (`row_count`, `col_count`, `caption`, and
`table_classes`), and exports two functions:

~~~ ts
ast.split_out_head(): { head: Row[], body: Row[] }

ast.each_row((row) => ...)
~~~

One of Ast's jobs is to work out which rows are table headers. Use
the `split_out_head` function to return two lists of rows. 
The `head` entry contains zero or more rows that constitute the table's
header (and will likely wind up inside a `<thead>` tag). The `body`
entry is the rest of the rows.

Sometimes you need to be able to iterate over all rows, header or not
(we'll see an example of this when we implement the marked extension
below). That's what `each_row` is for.

### Rows

Rows are simply wrappers for a list of cells. They export a single
function:

``` ts
row.each_cell((cell) =>  ... )
```

Rows have no useful attributes.

### Cells and Formats

A `Cell` represents a (possibly hidden) rectangular cell in the table.

Each cell has six attributes:

`rowspan_count`<br/>`colspan_count`
: the number of rows, columns, or both this cell should merge with. If
the cell is not supposed to merge, the value will be zero. To
create a `rowspan` or `colspan` attribute value, you'll need to add one
to these values.

`hidden`
: if a cell is being spanned over, then it is marked as hidden, and you
shouldn't generate any HTML for it.

`content`
: a string containing the cell's text content. This string will 
not have been processed yet, so at some point you'll need to ensure it
goes througth a markdown processor.

`format`
: any object containing the formatting information for this cell (see
below)

`data`
: an attribute that is not used by the tableau library. You can use it
to pass information between various phases of the processing pipeline.

The `Format` object s four attributes you may use: 

`alignment`
: a single character string: "<", "=", or ">" for left, center, or
right.

`span`
: If this cell is merged with the preceding row or column, this field
will contain "^" or "{". You probably won't use this, as the Ast will
have already merged cells and set `rowspan_count` and `colspan_count` in
the cell being merged from.

`css_classes`
: A list of css_classes specified for this cell.

`heading`
: if `true`, this cell is a heading (rendered using `<th>`)


# Extension Example

This section looks at the source for a tableau table plugin for the
Marked processor.

Before we start, we need to create a new project and 
install the tableau library and Marked:

~~~ shell-session
$ mkdir tableau-marked
$ cd tableau-marked
$ yarn init -y
$ git init
$ yarn add -S js-tableau-parser
$ yarn add -S marked
$ yarn add -D @types/marked
$ mkdir src
~~~

Then do whatever magic you normally do to set up a build environment. 

### Shape of the Extension

Marked is a pipeline processor. We need to insert parts of our extension
into three parts of that pipeline:

* The `start` code, which is used to test whether a block might be
  suitable for processing with this extension.

* The `tokenizer`, where we'll parser our table and, if successful,
  insert a new token containing our AST into the Marked pipeline.

* The `renderer`, where we'll be called to handled the special `tableau`
  token we previously injected.

These three phases correspond roughly to the `test`, `to_ast`, and
`ast_to_html` APIs in the tableau library.

### Source Code

We'll create our extension in `src/tableau-marked.ts`.

~~~ ts title="src/tableau-marked.ts"
import * as Tableau from "js-tableau-parser" 
import Marked from "marked"

type Token = Marked.marked.Token  // (1)
~~~

1. It took me a while to find the correct type definition, so
   I aliased it as a reminder to my future, forgetful, self.


A Marked extension module exports a descriptor, contain the name and
type of extension, and a list of functions to inject into the pipeline.

~~~ ts
const TABLEAU_EXTENSION = {
  name:  'tableau',
  level: 'block',
  start: Tableau.test,  //(1)
  tokenizer,            //(2)
  renderer,             //(3)
}

export default {
    extensions: [ TABLEAU_EXTENSION ]
}
~~~

1. The `start` callback is simply forwarded to the `test` function in
   the tableau library.

2. The `tokenizer` just calls our function of the same name.

3. And the same for the `renderer` function.


The tokenizer needs to verify that the block it is passed contains two
or more table rows. That match is performed by the following regular
expression. 

~~~ ts
const TABLE_RE = new RegExp(
  `(`          +
    `\\s{0,3}` +     //permitted indentation
    `\\|`      +     //starting pipe
    `[^\\n]+`  +     // body of the line (see [1] below)
    `\\|`      +     // trailing pipe
    `\\s*\\n`  +     // spaces nd EOL
  `){2,}`)           // Two or more rows is a table
~~~

!!! note
    I like to use the `x` option so that I can space out and document
    complex regexps, but JavaScript support is spotty, so instead I just
    split it into lots of strings and concatenate them.

Now let's start the `tokenizer` function. Marked requires a tokenizer to
return a token object. This must have `type` and `raw` attributes. To
this we add an attribute to hold the ast.

~~~ ts
interface TableauToken {
  type: 'tableau',
  raw: string,
  ast: Tableau.Ast
}
~~~

Marked calls the tokenizer with `this` set to an object containing
a lexer. In typescript, you indicate this by adding a typed `this` parameter
to the function definition. Again, here's an alias so I don't have to go grovelling through
the `index.d.ts` file again.

~~~ ts
type TokenizerThis = Marked.marked.TokenizerThis
~~~

And now the tokenizer:

~~~ ts
function tokenizer(this: TokenizerThis, 
                   src: string, 
                   _tokens: any): TableauToken | undefined {

  const match = TABLE_RE.exec(src);       // (1)

  if (!match)                             // (2)
    return

  const ast = Tableau.to_ast(match[0])    // (3)

  if (!ast)                               // (4)
    return

  // marked makes you tokenize children here, then
  // render them later
  ast.each_row(row => {
    row.each_cell(cell => {
      const tokens: Marked.marked.Token[] = []
      this.lexer.inlineTokens(cell.content, tokens);    // (5)
      cell.data = tokens 
    })
  })

  return {
    type: 'tableau',
    raw: match[0],
    ast: ast,
  }
}
~~~

1. See if the block we receive matches the pattern we defined.

2. If the block doesn't match the pattern, return `undefined`, which
   tells Marked to try another tokenizer.

3. If it matches, we call the tableau `to_ast` function, passing it the
   matched text. This returns an ast object or false.

4. If the `to_ast` function failed, then we return `undefined` and
   Marked moves on to the next tokenizer.

5. Each cell in the AST contains its original markdown. We need to
   convert this into HTML by the time we generate output, but, as we've
   discovered, Marked makes this a two-step process. Here, we call
   Marked's inline-text tokenizer. The result of parsing the cell's
   content will be stored in the `tokens` array, which we then assign to
   the `data` attribute of the cell. We'll use it when we render.


Now the `renderer` function. Before using the library's `ast_to_html`
function, it needs to retrieve the tokens representing each cell's
content, render it, and stick the resulting HTML back into the cell.

~~~ ts
type RendererThis = Marked.marked.RendererThis

// I _think_ there's a bug in the Marked index.d.ts: the parser passed
// to the renderer does not require a renderer as its second parameter
type ActualParseInline = (tokens: Token[]) => string

function renderer(this: RendererThis, table: TableauToken) {
    table.ast.each_row(row => {
      row.each_cell(cell => {
        cell.content = (this.parser.parseInline as ActualParseInline)(cell.data)
      })
    })

  return Tableau.ast_to_html(table.ast)
}
~~~

??? note "The full source file is here"

    ~~~ ts title="src/tableau-marked.ts"
    import * as Tableau from "js-tableau-parser" 
    import Marked from "marked"

    type Token = Marked.marked.Token


    ////////////////////////////////////////////////////////////////////////

    const TABLEAU_EXTENSION = {
      name:  'tableau',
      level: 'block',
      start: Tableau.test,
      tokenizer, //: (src: string, tokens: Token[]) => tokenizer(src,  tokens),
      renderer,
    }

    ////////////////////////////////////////////////////////////////////////

    export default {
        extensions: [ TABLEAU_EXTENSION ]
    }

    ////////////////////////////////////////////////////////////////////////


    // the following defines a row (text between pipe characters)
    const TABLE_RE = new RegExp(
      `(`          +
        `\\s{0,3}` +     //permitted indentation
        `\\|`      +     //starting pipe
        `[^\\n]+`  +     // body of the line (see [1] below)
        `\\|`      +     // trailing pipe
        `\\s*\\n`  +     // spaces nd EOL
      `){2,}`)           // Two or more rows is a table

    // TODO 1: this is a fairly inefficient regexp, as it will backtrack on each
    // intermediate pipe in a table row. It can be fixed with some negative
    // lookahead, but I want to keep it simple for now.

    ////////////////////////////////////////////////////////////////////////

    interface TableauToken {
      type: 'tableau',
      raw: string,
      ast: Tableau.Ast
    }

    type TokenizerThis = Marked.marked.TokenizerThis

    function tokenizer(this: TokenizerThis, src: string, _tokens: any): TableauToken | undefined {
        const match = TABLE_RE.exec(src);

        if (!match)
          return

        const ast = Tableau.to_ast(match[0])

        if (!ast)
          return

        // marked make you tokenize children heere, then
        // render them later
        ast.each_row(row => {
          row.each_cell(cell => {
            const tokens: Marked.marked.Token[] = []
            this.lexer.inlineTokens(cell.content, tokens);
            cell.data = tokens 
          })
        })

        return {
          type: 'tableau',
          raw: match[0],
          ast: ast,
        }
      }


    ////////////////////////////////////////////////////////////////////////

    type RendererThis = Marked.marked.RendererThis

    // I _think_ there's a bug in the Markmed index.d.ts: the parser passed
    // to the renderer does not require a renderer as its second parameter
    type ActualParseInline = (tokens: Token[]) => string

    function renderer(this: RendererThis, table: TableauToken) {
        table.ast.each_row(row => {
          row.each_cell(cell => {
            cell.content = (this.parser.parseInline as ActualParseInline)(cell.data)
          })
        })

      return Tableau.ast_to_html(table.ast)
    }
    ~~~
