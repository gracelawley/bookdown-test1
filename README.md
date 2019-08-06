# bookdown-test1

### The original author/creator of the bookdown:

1. `bookdown:::bookdown_skeleton(getwd())`
1. `renv::init()`

    This adds three new files.
  
```{r echo = FALSE}
knitr::include_graphics("img/renv-init.png")  
```
  
1. ...edit bookdown content... (added `library(dplyr)` to `01-intro.Rmd`)
1. `renv::snapshot()`

    Resulting output:
    
```{r echo = FALSE}
knitr::include_graphics("img/renv-snapshot.png")
```
    
1. Upload `renv.lock` to GitHub, add `.Rprofile` and `renv/` to `.gitignore`


### The person who wants to render the bookdown locally but doesn't have all of the required packages installed:

*Assuming `dplyr` is not installed*

1. Fork and clone the original repo
1. Create a new RStudio Project linked to the forked repo
1. Open up the project in RStudio

...what happens when they try to preview the bookdown as-is? __Throws an error:__


    ```r
    Quitting from lines 63-64 (bookdown-test1.Rmd) 
    Error in library(dplyr) : there is no package called 'dplyr'
    ```

1. Install `renv` package if needed: `if (!require("renv")) install.packages("renv")`
1. Prepare project: `renv::activate()`
1. Restore the last snapshot (i.e. `renv.lock`) and install any missing packages: `renv::restore(actions = "install")`

...does this cause `dplyr` to be installed? Can they preview the bookdown now?

Yes!!!




