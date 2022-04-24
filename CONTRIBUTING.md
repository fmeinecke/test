# Contribution Process

In general, everyone can and should be contributing content to the book! We have a philosophy of contributing even incomplete, but correct information (and a hard prohibition on complete but incorrect information).

In order to get quick feedback and help, please consider joining the [Google Chat room `Science@Zalando`](https://chat.google.com/room/AAAAo03Xe4M) and follow these steps on a local fork of the repository:

1. Fork this repository and create a branch on your local machine.

2. Publish the branch to *your* remote repository.

3. Add or edit the R-Markdown files until you are happy with the changes (a powerful editor could either be [VS Code](https://code.visualstudio.com/) or [RStudio](https://rstudio.com/)). Please follow the [Style Guide](#style-guide). This has information on how to correctly insert [images](#images). To compile the book for preview on your local machine see the [guide](#preview-the-book-on-your-machine) below.

4. Commit the changes locally and push the to the remote repository. 

5. Create a pull request to *this* repository. That will create a staged view of the entire book at https://science.docs.zalando.net/pr/#pull_request_number/index.html (where you replace *pull_request_number* with the actual number of your pull request).

6. Make further edits and keep committing locally and pushing to your remote repository. This will create a new build process of the entire book for you to review. 

7. When you are happy with your changes, post in the chat room requesting a review.

8. Any member of the GitHub team [Science Reviewers](https://github.bus.zalan.do/orgs/bpai/teams/science-reviewers) (page is readable for [members of the GitHub BPAI org](https://github.bus.zalan.do/orgs/bpai/people)) will review the changes as well and, if they are happy, will merge the changes. Within 1-2 minutes, the entire book is up-to-date at https://science.docs.zalando.net/. They will also remove the remote-branch from the repository.

If you have any questions or suggestions, please join the [chat room](https://chat.google.com/room/AAAAo03Xe4M) and ask them there or send an email directly to justin.rao@zalando.de.

## Style Guide

Unless otherwise specified below, we use the [APA Style Guide](https://apastyle.apa.org/). 

Also, use American English. 

### Content self-sufficiency (when to link/when not to link)

All content in the book should be self-explanatory, i.e. it should not require the reader to link out to external material to understand the included material. This is especially true for descriptions of what is running in production. 

Links should be used to direct readers to additional information that may be of interest.

### Diagrams

This project supports two tools for creating diagrams in code: [tikz](https://en.wikibooks.org/wiki/LaTeX/PGF/TikZ) and [graphviz (dot)](https://graphviz.gitlab.io/).

Create a code chunk with `tikz` or `graphviz` as the code chunk engine:

````
```{tikz}
```
````

Other [knitr code chunk options](https://yihui.org/knitr/options/) can be used as normal. Enter native tikz or graphviz code into the code chunk.

#### Tikz example

````
```{tikz, basic-problem-tikz, fig.cap="Simple e-commerce attribution problem. What is the incremental contribution of each touch point on the outcome $y$?", cache=TRUE, fig.width=4}
\usetikzlibrary{arrows}
\begin{tikzpicture}[node distance=2cm, auto,>=latex', thick, scale = 0.5]
\node (x1) {$x_1$};
\node (x2) [right of=x1] {$x_2$};
\node (xn) [right of=x2] {$x_n$};
\node (y) [right of=xn] {$y$};

\draw[->, dashed] (x1) to node {} (x2);
\draw[->, dashed] (x2) -- (xn);
\draw[->, dashed] (xn) -- (y);

\draw[->, dashed] (x1) to[out=30,in=120] (xn);

\draw[->] (x1) to[out=30,in=120] (y);
\draw[->] (x2) to[out=30,in=120] (y);
\draw[->] (xn) to[out=30,in=120] (y);

\end{tikzpicture}
```
````

![tikz example](images/CONTRIBUTING/tikz-example.png)

#### Graphviz (dot) example

````
```{dot spillover-problem, fig.cap="A negative spillover problem", fig.width=1, cache=TRUE, dpi=250}
digraph spilloverproblem {
    graph [layout = dot, rankdir = TB, overlap = true, fontsize = 10];
    edge [arrowsize = 0.5];
    node [shape = plaintext];

    ads     [label = "Off-site Social Advertising"];
    reco    [label = "On-site Recommendation"];
    sale1   [label = "First Sale"];
    sale2   [label = "Second Sale"];
    salen   [label = "n-th Sale"];

    ads -> reco;
    ads -> sale1;
    reco -> sale1;
    reco -> sale2;
    reco -> salen
}
```
````

![graphviz example](images/CONTRIBUTING/graphviz.png)

### Emphasis

- **bold**: denotes the *definition* of a core concept that is then used throughout the text (not in bold but linking to the definition) 

     + **average treatment effect**, **confounder**

- *italics*: simple emphasis 

    + "Do *not* turn off the computer!"

- quotes (`""`): 

    + denote a quote from a text 

        - "The x-pox scenario is pedagogically useful but intentionally simplistic" (Manski 2013, 125). 

    + scare quotes drawing attention to an unusual or arguably inaccurate use

### Headings 

Headings at levels 1 and 2 (e.g. `#` and `##`) use [title case](https://blog.apastyle.org/apastyle/2012/03/title-case-and-sentence-case-capitalization-in-apa-style.html): 

  - Training vs. Test Data
  
Headings from level 3+ should follow sentence case:

  - Training vs. test data

Do not use headings of level 5+.

### Images

To include images, first place a [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics) version of the image into the chapter's image directory. For example, to include an image called `fraud-image-example.png` in the *Fraud* chapter:

```
images
├── Fraud
│   └── fraud-image-example.png
```

Then, in the text where you want to include the image, use a [knitr code chunk](https://bookdown.org/yihui/rmarkdown/r-code.html) to link to the image file. For example:

````markdown
```{r CHUNK-LABEL, echo=FALSE, fig.cap="Caption for the Figure"}
knitr::include_graphics("images/Fraud/fraud-image-example.png")
```
````

`CHUNK-LABEL` is a label you define that identifies the chunk. Specify the figure caption with the `fig.cap` chunk option. See the [knitr reference manual](https://yihui.org/knitr/options/) for the full set of chunk options and their values, including options to change the figure width (`fig.width`) and height (`fig.height`).

### Tables

Tables can be written in [normal Markdown style](https://www.markdownguide.org/extended-syntax/#tables).
For example:

```md
| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |
```

Bookdown offers support for more formats, details can be found in the [official docs](https://bookdown.org/yihui/bookdown/tables.html).
In the `.Rmd` files, a table should be embedded within a `<center>` tag and complemented by a label and a caption.

```
<center>
Table: (\#tab:tablelabel) Table caption

| Heading 1 | Values |
| --- | ---:|
| ABC | 1.32 |
</center>
```

Note that the table label `tab:tablelabel` has to start with `tab:` for the automatic numbering to work.
The table can be referenced from all places in the document as shown below:

```
In Table \@ref(tab:tablelabel) we show ...
```

In the rendered book the example from above will appear as shown in the following Figure.

![Table screenshot](images/CONTRIBUTING/table-screenshot.png)
  
### Mathematical Notation 

Please follow this style guide for writing mathematical notation.

- Metric spaces should be written in calligraphic Roman letters, e.g. `$\mathcal{X}$`.

- Sets should be written in Roman capital letters, e.g. `$X$`.

- Indices are preferably written in lower-case Roman letters, e.g. `$i$` in `$X_i$`.

- Matrices and vectors should be indicated with bold font, in particular when the matrix has at least two rows or columns or the vector has at least two elements, e.g., vector `$\mathbf{x}$` or matrix `$\mathbf{X}$`.

- Vector are column vector by default; we use the `$^\top$` symbol for transposition of vectors and matrices.

- Function application should be indicated with parentheses, e.g. `$f(x)$` is the value of the function `$f$` at the point `$x$`. Functions are ideally written in lower-case Roman letters. If there is a chance of confusion with an index, we recommend to add text to reduce uncertainty for the reader.

- The symbol `$P$` is used for the probability of a discrete event and `$p$` is reserved for probability density. Also, conditional probabilities are denoted by a `$|$`, i.e., `$P(x|y)$`.

- Greek letters should be used for parameters and hyper-parameters rather than random variables, e.g. `$f(x;\theta)$` denotes that `$f$` is a function that maps `$x$` and the set of functions is parameterized by `$\theta$`.

Most importantly, remember that the text is written for the *reader* not for the *writer*. Thus, if you think there could be ambiguity in the meaning of the mathematical notation, just add an extra sentence in English language to disambiguate. See also [Halmos (1970)](http://math.uga.edu/~azoff/courses/halmos.pdf) for guidance on mathematical writing generally. 

## Preview the book on your local machine

Once you have cloned the book onto your local machine and created a new branch for editing, you probably want to be able to preview the output as you edit. Here are instructions for building and previewing the book on your local machine.

### Text editor Markdown preview

Since the book is mostly just Markdown, a Markdown preview in your editor (with math support, e.g. in VS Code: *Markdown All In One* and *Markdown + Math* packages) will preview the output close to how it will look in the final version. For example:

![Markdown Preview in VS Code](images/CONTRIBUTING/markdown-preview-vs-code.png)

The following instructions build the entire book.

### From the terminal 

#### Install


1. In your terminal install [graphviz](https://graphviz.gitlab.io/download/). For example, on macOS using [Homebrew](https://brew.sh/):

```shell
brew install graphviz
```

If you are on Linux you will also need to install the software listed [here](https://github.bus.zalan.do/bpai/research-docs-overlay/blob/e7e7f425d5daefb5cf570fa6d37c7b1ccd9976c6/delivery.yaml#L25).

2. Make sure you have [R](https://cloud.r-project.org/) and [pandoc including citeproc](https://pandoc.org/installing.html) installed.

3. Start an R session in the terminal and install *bookdown*:

```R
install.packages(c("bookdown", "magick", "pdftools"))
```

#### Build

1. Make sure that *research-docs* is your working directory and compile the book:

```R
# Website
bookdown::render_book('index.Rmd', 'bookdown::gitbook')
```

If you want to compile the PDF you will need a LaTeX installation:

```R
# If no LaTeX installed
install.packages("tinytex")

# PDF
bookdown::render_book('index.Rmd', 'bookdown::pdf_book')
```

#### From RStudio

1. From RStudio open the *research-docs* project:

**File** → **Open Project...** 

2. Then click **Build Book**:

![Build Book in RStudio](images/CONTRIBUTING/rstudio-build.png)

If you don't have *bookdown* or LaTeX installed, RStudio will prompt you to install them.

You may also need to install graphviz and the other programs (on Linux) as described [above](from-the-terminal).


## Spell Checking

In the `R` console, run the following:

```R
# install R package for spell checking
install.packages("spelling")

# load the package
library(spelling)

# run spell checker in the file(s) of your choice
exceptions <- Filter(function(l) {!startsWith(l, "#")}, readLines(file("spell_check_exceptions.txt", "r")))
spell_check_files('Recommendations.Rmd', lang='en_US', ignore=exceptions)

# it works for both .Rmd and .md files:
spell_check_files('CONTRIBUTING.md', lang='en_US', ignore=exceptions)
```

If you see false positives, please add them to the file `[spell_check_exceptions.txt](spell_check_exceptions.txt)`.
Re-run the line starting with `exceptions <-` to reload the file.
