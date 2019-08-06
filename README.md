# Using the `renv` package with a Bookdown

This is what worked for me after many iterations of trial and error. It differs slightly from what is recommended in the `renv` package's [Collaborating with renv](https://rstudio.github.io/renv/articles/collaborating.html).


### Part 1 - The original author(s):

1. Two starting points:
    + If they already have a Bookdown (linked to GitHub) ready, skip to step 2. 
    + Otherwise, create a new GitHub repo, link the repo to a new RStudio Project, and create a skeleton bookdown via `bookdown:::bookdown_skeleton(getwd())`.
  
1. In the Bookdown project, initialize `renv` via [`renv::init()`](https://rstudio.github.io/renv/reference/init.html). This adds three new files: `renv.lock`, `renv`, and `.Rprofile`. 
    + `renv.lock` - contains information about the packages used in the Bookdown project (found via `renv::dependencies()`
    + `.Rprofile` - used by `renv` behind the scenes (adds `source("renv/activate.R")`)
    + `renv/` - things that `renv` uses behind the scenes

1. Continue with typical workflow...edit bookdown content...install a new package...etc.... (for the example: added `library(dplyr)` to `01-intro.Rmd`)

1. Take a "snapshot" of the packages used in the Bookdown via `renv::snapshot()`. This will update `renv.lock` with any changes.

1. Upload `renv.lock` to GitHub, add `.Rprofile` and `renv/` to `.gitignore`
    

### Part 2 - The person who wants to render the bookdown locally but doesn't have all of the required packages installed:

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

1. Prepare project using `renv::activate()`, NOT `renv::init()`. The latter causes `renv.lock` to fill up with other packages I had installed (that were not used in the Bookdown). `renv::activate()` doesn't change `renv.lock` so it can be used in the next step.

1. Restore the last snapshot (i.e. `renv.lock`) and install any missing packages: `renv::restore(actions = "install")`

...does this cause `dplyr` to be installed? Can they preview the bookdown now?

Yes!!!



## Notes

* Tried uploading `.Rprofile` and `renv/` to GitHub in Part 1 but this caused a few different unwanted things to happen in Part 2.
    + See this [issue](https://github.com/rstudio/renv/issues/74)
    + `renv` tried to add other packages I had installed (that were not used in the Bookdown) into the `renv.lock` file when I called `renv::restore()`.
    + If I didn't have a package installed that was required by the Bookdown project I would immediately get an warning message, but `renv` would not prompt me to install the missing package when I called `renv::restore()`. Not sure why this happened?


