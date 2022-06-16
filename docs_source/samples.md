## Table from the APA Style Guidelines

<style>
.md-typeset h2 {
    margin-top: 4.0em;
}
.md-typeset h1 {
    margin-bottom: -2em;
}
</style>


=== "Result"
    |!.apa Results From a Factor Analysis of the Parental Care and Tenderness (PCAT) Questionnaire |
    |#... PCAT item |{ | Loading |{ |{ |
    |^          |^ |#... 1 | 2 | 3 |
    |< Factor 1: Tenderness—Positive |{...|
    |:>   |<     |>...|
    |  20.| You make a baby laugh over and over again by making silly faces.	| .86 |	.04 |	.01 |
    |  22.| A child blows you kisses to say goodbye.	| .85 |	−.02 |	−.01 |
    |  16.| A newborn baby curls its hand around your finger.	| .84 |	−.06 |	.00 |
    |  19.| You watch as a toddler takes their first step and tumbles gently back down.	| .77 |	.05 |	−.07 |
    |  25.| You see a father tossing his giggling baby up into the air as a game.	| .70 |	.10 |	−.03 |
=== "Markdown source"
    ~~~ md
    |!.apa Results From a Factor Analysis of the Parental Care and Tenderness (PCAT) Questionnaire |
    |#... PCAT item |{ | Loading |{ |{ |
    |^          |^ |#... 1 | 2 | 3 |
    |< Factor 1: Tenderness—Positive |{...|
    |:>   |<     |>...|
    |  20.| You make a baby laugh over and over again by making silly faces.	| .86 |	.04 |	.01 |
    |  22.| A child blows you kisses to say goodbye.	| .85 |	−.02 |	−.01 |
    |  16.| A newborn baby curls its hand around your finger.	| .84 |	−.06 |	.00 |
    |  19.| You watch as a toddler takes their first step and tumbles gently back down.	| .77 |	.05 |	−.07 |
    |  25.| You see a father tossing his giggling baby up into the air as a game.	| .70 |	.10 |	−.03 |
    ~~~

## A Word in Memory

<style>
.tableau-table.mem th {
  background-color: inherit;
  color: var(--md-primary-fg-color);
  font-size: 120%;
}

.tableau-table.mem .t {
  border: 0.5px solid var(--grid-line-color-normal);
  border-bottom: none;
  padding-top: 0;
  padding-bottom: 0;
  font-weight: 600;
}
.tableau-table.mem .c {
  height: 0.75em;
  padding: 0;
  width: 1em;
  border: 0.5px solid var(--grid-line-color-normal);
  border-top: none;
}
</style>

=== "Result"
    |!.mem |
    |# address 2000: |.t… 12 |{|{|{|{|{|{|{|   34 |{|{|{|{|{|{|{|   56 |{|{|{|{|{|{|{|   78 |{|{|{|{|{|{|{|
    |^               |.c…    | | | | | | | |      | | | | | | | |      | | | | | | | |      | | | | | | | | 
=== "Markdown Source"
    ~~~ css
    <style>
    .tableau-table.mem th {
      background-color: inherit;
      color: var(--md-primary-fg-color);
      font-size: 120%;
    }
    .tableau-table.mem .t {
      border: 0.5px solid var(--grid-line-color-normal);
      border-bottom: none;
      padding-top: 0;
      padding-bottom: 0;
      font-weight: 600;
    }
    .tableau-table.mem .c {
      height: 0.75em;
      padding: 0;
      width: 1em;
      border: 0.5px solid var(--grid-line-color-normal);
      border-top: none;
    }
    </style>

    |!.mem |
    |# address 2000: |.t… 12 |{|{|{|{|{|{|{|   34 |{|{|{|{|{|{|{|   56 |{|{|{|{|{|{|{|   78 |{|{|{|{|{|{|{|
    |^               |.c…    | | | | | | | |      | | | | | | | |      | | | | | | | |      | | | | | | | | 
    ~~~

## Colored Cell Backgrounds


<style>
.tableau-table.stock td.neg { background: #fdd; }
.tableau-table.stock td.pos { background: #dfd; }
[data-md-color-scheme=slate] .tableau-table.stock td.neg { background: #633; }
[data-md-color-scheme=slate] .tableau-table.stock td.pos { background: #252; }
</style>

=== "Result"
    |!.stock.hlines Fictitious Stock Closing Prices |
    |#... Symbol and Name |{    | Volume | P/E | Close | % Change  |
    |:<   |<                    |>...    |
    | MGC | Megacorp            | 12,312 | 19 | 243.75 |.neg -1.20 |
    | RUR | Rostrum's Universal | 4,955  | 21 |   8.26 |.pos  0.65 |
    | 007 | Universal Exports   | 9,582  | 14 |  43.85 |.pos  1.39 |
    | SSB | South Sea Bubble    | 32,485 |  3 |   0.42 |.neg -5.75 |
=== "Markdown source"
    ~~~ css
    |!.stock.hlines Fictitious Stock Closing Prices |
    |#... Symbol and Name |{    | Volume | P/E | Close | % Change  |
    |:<   |<                    |>...    |
    | MGC | Megacorp            | 12,312 | 19 | 243.75 |.neg -1.20 |
    | RUR | Rostrum's Universal | 4,955  | 21 |   8.26 |.pos  0.65 |
    | 007 | Universal Exports   | 9,582  | 14 |  43.85 |.pos  1.39 |
    | SSB | South Sea Bubble    | 32,485 |  3 |   0.42 |.neg -5.75 |

    <style>
      .tableau-table.stock td.neg { background: #fdd; }
      .tableau-table.stock td.pos { background: #dfd; }
      [data-md-color-scheme=slate] .tableau-table.stock td.neg { background: #633; }
      [data-md-color-scheme=slate] .tableau-table.stock td.pos { background: #252; }
    </style>
    ~~~

    1. The `data-md-color-scheme...` rules handle the light/dark color scheme
       switching in the [processor](https://squidfunk.github.io/mkdocs-material/)
       I'm using for this document.

## Nutrition Table

<style>
table.nutrition {
  border: solid 6px var(--grid-line-color-normal);
}
table.nutrition .bold {
    font-weight: bold;
}

table.nutrition .ull {
    border-bottom: 1px solid var(--grid-line-color-normal);
}

table.nutrition .ulm {
    border-bottom: 2px solid var(--grid-line-color-normal);
}

table.nutrition .ulb {
    border-bottom: 8px solid var(--grid-line-color-normal);
}
</style>

=== "Result"
    |:<                                                                   |< |> |
    |.ulb…  Serving Size 1/2 cup (about 82g)<br/>Servings Per Container 8 |{ |{ |
    |.ull…  Amount Per Serving                                            |{ |{ |
    |:.bold                                                               |  |.bold|
    |.ulm… Calories 200 |{ | Calories from Fat 130 |
    |.ull… | | % Daily Value |
    | Total Fat  |{                           | 22% |
    |            |.ull… Saturated Fat 9g      | 22% |
    |.ull…       | Trans Fat 0g               |  0% |
    |.ull… Cholesterol 55mg  |{               | 18% |
    |.ull… Sodium 40mg |{                     |  2% |
    | Total Carbohydrate 17g |{               |  6% |
    |            |.ull… Dietary Fiber 1g      |  4% |
    |.ull…       | Sugars 14g                 |  0% |
    |.ulm… Protein 3g |{ |
    |!.nutrition (from \
      [examples of data tables](https://wpdatatables.com/examples-of-data-tables/)) |
=== "Markdown source"
    ~~~ md
    |:<                                                                   |< |> |
    |.ulb…  Serving Size 1/2 cup (about 82g)<br/>Servings Per Container 8 |{ |{ |
    |.ull…  Amount Per Serving                                            |{ |{ |
    |:.bold                                                               |  |.bold|
    |.ulm… Calories 200 |{ | Calories from Fat 130 |
    |.ull… | | % Daily Value |
    | Total Fat  |{                           | 22% |
    |            |.ull… Saturated Fat 9g      | 22% |
    |.ull…       | Trans Fat 0g               |  0% |
    |.ull… Cholesterol 55mg  |{               | 18% |
    |.ull… Sodium 40mg |{                     |  2% |
    | Total Carbohydrate 17g |{               |  6% |
    |            |.ull… Dietary Fiber 1g      |  4% |
    |.ull…       | Sugars 14g                 |  0% |
    |.ulm… Protein 3g |{ |
    |!.nutrition (from \
      [examples of data tables](https://wpdatatables.com/examples-of-data-tables/)) |

    <style>
        table.nutrition {
          border: solid 6px var(--grid-line-color-normal);
        }
        table.nutrition .bold {
            font-weight: bold;
        }

        table.nutrition .ull {
            border-bottom: 1px solid var(--grid-line-color-normal);
        }

        table.nutrition .ulm {
            border-bottom: 2px solid var(--grid-line-color-normal);
        }

        table.nutrition .ulb {
            border-bottom: 8px solid var(--grid-line-color-normal);
        }
    </style>
    ~~~

