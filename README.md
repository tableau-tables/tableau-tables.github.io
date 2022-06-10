# Tableau Tables

Tableau tables is an enhanced markup for Markdown tables, written as a library to
make it easy to add as a plugin to existing Markdown processors.

I created tableau because I use Markdown to write standalone documents,
and I needed better support for tabular material:

#### Headers

* Headerless tables
* Multiline headers
* Multiple separate headers
* Headers in columns and rows
* Captions

#### Layout

* Layout uses CSS styles and not inline attributes (making it easier to
  change the style of a whole document)
* Per cell alignment and CSS classes
* Default attributes, both down columns and across rows
* Table-wide classes
* Row and column span
* Continuation lines

Here are some tables created using Tableau, rendered using either
Python Markdown or JavaScript Marked.

And here's a description of the markup.

## If You Want to Use Tableau Tables

You'll need to find an extension/plugin for the particular Markdown
processor you're using. 

* [Marked](https://github.com/markedjs/marked) (JavaScript)
  Extension available on NPM. See ....

* [Python Markdown](https://python-markdown.github.io/)
  The extension is available through GitHub. For information see
  https://....

## If you use a different Markdown Processor

I use multiple Markdown processors, and I didn't want to have to
implement a whole new tableau plugin for each.

Instead, I wrote two library implementations of tableau, one in Python
and the other in JavaScript.

These libraries are intended to make it
easy to write a plugin so that you can use tableau-formatted tables in
a Markdown processor that doesn't currently support it.

I wrote the Marked extension in a couple of hours (most of which was
spent fighting with Typescript types).

If you're interested in writing an extension, see the documentation for
the JavaScript library or the Python library. If your particular
environment requires features not currently in the library, let me know
and I'll add them.

If you'd like to create a version of the libraries for another
programming language, please let me know so we can coordinate effort.
