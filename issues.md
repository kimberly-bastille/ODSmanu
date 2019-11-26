## pandoc error, install new version

```
Error running filter author-info-blocks.lua:
Error while running filter function:
[string "author-info-blocks.lua"]:168: attempt to call a nil value (method 'map')
Error: pandoc document conversion failed with error 83
```

Error seems related to old version of pandoc:

[Error when generating PDF with academic author syntax (scholarly-metadata.lua & author-info-blocks.lua) · Issue #64 · pandoc/lua-filters](https://github.com/pandoc/lua-filters/issues/64#issuecomment-519447021)


```bash
/Applications/RStudio.app/Contents/MacOS/pandoc/pandoc --version
pandoc 2.3.1
```

[Install Pandoc • rmarkdown](https://rmarkdown.rstudio.com/docs/articles/pandoc.html)

[Pandoc - Installing pandoc](https://pandoc.org/installing.html)

```bash
brew install pandoc

/usr/local/bin/pandoc --version
pandoc 2.8
```

## cslreferences error, update pandoc-citeproc and rmarkdown, pdf template NULL

```
LaTeX Error: Environment cslreferences undefined
```

```
The extension ascii_identifiers is not supported for markdown
Error: pandoc document conversion failed with error 23
```

```bash
brew install pandoc-citeproc librsvg
```

rmarkdown 1.15 -> 1.17

[Correct Pandoc citeproc binary on windows by cderv · Pull Request #1653 · rstudio/rmarkdown · GitHub](https://github.dyf62976.workers.dev/rstudio/rmarkdown/pull/1653)

```
! LaTeX Error: Environment cslreferences undefined.

Error: Failed to compile ODM.tex. See https://yihui.name/tinytex/r/#debugging for debugging tips. See ODM.log for more info.
In addition: Warning message:
In styling_latex_position_right(x, table_info, hold_position, table.envir) :
  Position = right is only supported for longtable in LaTeX. Setting back to center...
Execution halted
```

```bash
/usr/local/bin/pandoc-citeproc --version
pandoc-citeproc 0.16.4
```

[cslreferences environment missing in latex template · Issue #1649 · rstudio/rmarkdown](https://github.com/rstudio/rmarkdown/issues/1649#issuecomment-533153249)

```md
output:
  pdf_document:
    template: null
```

## paragraph ended, usepackage{graphicx}

```
! Paragraph ended before \Gin@iii was complete.
<to be read again>
                   \par
l.180

Error: Failed to compile ODM.tex. See https://yihui.name/tinytex/r/#debugging for debugging tips. See ODM.log for more info.
```

[graphics - "Paragraph ended before \Gin@iii was complete" while inserting image with \includegraphics - TeX - LaTeX Stack Exchange](https://tex.stackexchange.com/questions/37650/paragraph-ended-before-giniii-was-complete-while-inserting-image-with-inclu)


[knitr - How to include LaTeX package in R Markdown? - TeX - LaTeX Stack Exchange](https://tex.stackexchange.com/questions/171711/how-to-include-latex-package-in-r-markdown)

```yaml
header-includes:
   - \usepackage{graphicx}
```

## side by side table and figure

Earlier attempts:

* [R Markdown - Positioning table and plot side by side - Stack Overflow](https://stackoverflow.com/questions/53659160/r-markdown-positioning-table-and-plot-side-by-side)
* [Arranging multiple grobs on a page](https://cran.r-project.org/web/packages/gridExtra/vignettes/arrangeGrob.html)
* [Displaying tables as grid graphics](https://cran.r-project.org/web/packages/gridExtra/vignettes/tableGrob.html)
* [graphics - How to include SVG diagrams in LaTeX? - TeX - LaTeX Stack Exchange](https://tex.stackexchange.com/questions/2099/how-to-include-svg-diagrams-in-latex)
* https://github.com/yihui/rmarkdown-cookbook/blob/master/05-latex.Rmd#L285
* [horizontal alignment - Aligning figures using subfigure Error: Not in outer par mode - TeX - LaTeX Stack Exchange](https://tex.stackexchange.com/questions/85014/aligning-figures-using-subfigure-error-not-in-outer-par-mode)
* [Table and Figure Side by Side](https://lgong30.github.io/skill/2017/01/12/table-figure-side-by-side.html)
* [Subfloats - LaTeX/Floats, Figures and Captions - Wikibooks, open books for an open world](https://en.wikibooks.org/wiki/LaTeX/Floats,_Figures_and_Captions#Subfloats)
* [subfloats - Combining tables and figures with common subtitle - TeX - LaTeX Stack Exchange](https://tex.stackexchange.com/questions/302464/combining-tables-and-figures-with-common-subtitle)


### convert google slide -> svg -> pdf

```bash
brew cask install inkscape
```

```
cd ~/github/ODSmanu/ODM/images
inkscape -D -z --file="$PWD/sciloop.svg" --export-pdf="$PWD/sciloop.pdf" --export-latex
````

### `subfig+adjustbox`

[subfloats - Align figure and tables in subfigure environment - TeX - LaTeX Stack Exchange](https://tex.stackexchange.com/questions/455305/align-figure-and-tables-in-subfigure-environment)

```yaml
output:
  rmarkdown::pdf_document:
    extra_dependencies:
      - subfig
      - adjustbox
header-includes:
  - \usepackage[export]{adjustbox}
```

standalone working example:

```
\begin{figure}
  \centering
  \subfloat[Confusion matrix.]{           
    \includegraphics[width=0.4\linewidth,valign=t]{./images/example-image.png}
    \label{fig:ma}
    }
  \subfloat[Accuracy and F1-scores.]{
    \adjustbox{valign=t}{
      \begin{tabular}{lr}
      \toprule
      \textbf{Metric} & \textbf{Score} \\ \midrule
      Accuracy        & 56.64\%        \\
      F1 for ON       & 72.32\%        \\
      F1 for OFF      & 0\%            \\ \bottomrule
      \end{tabular}
      \label{res-ma}}}
  \caption{Majority classifier results.}
\end{figure}
```
