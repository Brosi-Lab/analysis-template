---
title: "Your Title [must edit]"
author: "Your Name [must edit]"
date: "started [date file was begun, in 12 October 2018 format]; most recent edits 24 March 2022"
output:
  html_document:
    toc: true
    toc_depth: 4
    toc_float: true
    number_sections: true
    code_folding: hide
    theme: cosmo
    keep_md: true
  md_document:
    variant: markdown_github

---



# Preamble [*delete this when altering for your project!*]

## notes on the .Rmd header

* the header of this Rmarkdown template file has been altered in several ways...
    + note that you will need to edit the header with the title, your name, and the start date of the project
    + the header includes code to update the date when the analysis was last edited, which I really like (the default just puts the date at which you started the project... not that helpful)
* a table of contents (`toc`) is automatically generated and the sections of it are automatically numbered
    + because of the auto-numbering, you should use a first level header (a single `#`) to denote the highest-level sections, and go down from there (e.g., `##` to denote a second-level header, etc.)
    + PLEASE be thoughtful, consistent, and detail-oriented when setting up the header hierarchy in your document; it really helps with viewing and editing the code and especially the report
* the TOC is "floating", i.e. follows the reader as she or he scrolls down in the document, and can be clicked on to change sections
    + this is a HUGE advantage for moving around in a complex analysis quickly and easily
* it includes "code folding": the code is present in the `html` file but is hidden by default, one clicks on a button that says `code` in order to show it. This helps keep the report document streamlined and easy to scroll through.
* there is an `html` theme called `cosmo` that this template uses; while typographically more aesthetically pleasing than the default, a more important reason to use it is that the headers are not unreasonably huge. This is particularly important given TOC auto-numbering, wherein you must give top-level headers top-level status and in the default, that leads to unreasonably huge text.
    + I have not played around with the themes much, feel free to use one that generates reasonable header sizes (as long as the evil and dreaded Comic Sans font is never, ever, ever used)

## how to use this file

* this template is set up specifically for `html` Rmarkdown reports (not `pdf`)
* remember to replace (nearly) all of the text with text specific to your project
* you can refer to the .Rmd file in https://github.com/Brosi-Lab/FINAL-snowmelt-seeds as a "model" of how to do this
* you MUST name all code chunks with a unique name in order for your Rmarkdown file to knit to a report

## workspace / environment philosophy

while you should *eventually* delete the text in this section from your report, I suggest that you keep it around while you are coding, as a reminder to keep things streamlined and clean. Once an analysis gets to the very final stages (and assuming you have a clean workspace at that point!) you can delete.

* one bit of analysis philosophy is keeping a clean workspace / environment
    + if you save every temporary variable, model object, subsetted dataset, etc., your workspace will get really cluttered and personally I find it difficult to keep track of where things came from (e.g., what dataset is `community-data-4` and how does it differ from `community-data-5` or `community-data-2`?)
    + thus almost every code chunk has a line for cleanup at the end
    + think about cleanup at the end of every chunk, and keep updating the cleaning!
    + keep an eye on the "Environment" tab in RStudio---if it isn't pretty streamlined, you need to pay more attention to this
* typically I strive to having one overall "master" dataset and sometimes one dataset per major group of analyses
    + I almost always remove other components: model ojects, plot objects, etc. etc. unless they are needed downstream in the analysis
    + even then, once they are no longer needed downstream, I will typically remove after they have served their purpose
* one exception to this is that it can be (very) annoying to have to re-run everything above, say, a particular code chunk to get that code chunk to work again, if you delete some of the previous objects or objects you create in that chunk
    + thus, I often comment out the cleanup section while I am working on a particular chunk
    + but once it is working to my satisfaction, I will un-comment it to execute the cleanup

**Eventually, you will want to delete all of the text above this in your actual .Rmd file.**

---

# Overview

give context to your project here. while you don't have to go overboard, it is important to lay out what the questions are that you are answering, where the data come from, and the basic approach you will be taking to the analysis. Don't skimp.

# Package Loading and Management

## package loading

* it is good practice to have a single code chunk at the top of your file that loads ALL packages used anywhere in the analysis. That way, if someone else is trying to replicate your code, they will not get part of the way through and realize they are missing a needed package.
* it is also good practice to refer to uncommon packages needed for code to run, in the place *where* that code is implemented
    + this is not particularly important for commonly-used packages like `ggplot2` and (much) more important for less-used packages
* a minor pet peeve of mine is the package startup messages that can massively clutter up Rmarkdown reports
    + I include `message = F, results = F, warning = F` in the code chunk header to get rid of those...
    + but pay attention to the messages in case there are important warnings or errors; they will appear in this window when the code chunk is run, though not in the .html report
    + a perhaps better alternative to just paying attention is included here, using the `evaluate` package which allows for saving of messages arising from whatever process you specify (including package loading) and then printing them at the end of the report, along with `sessionInfo`; this keeps your report clean and streamlined, while preserving that information which can be useful for troubleshooting (though typically that is rare)
* the code in the chunk below allows you to:
    + make a list of all the packages you want / need for your Rmarkdown file
    + load the packages in a single line of code
* I suggest always including the `tidyverse` and `knitr` packages at a minimum
    + if you load `tidyverse` you don't have to load its subcomponents (`ggplot2`, `dplyr`, `stringr`, etc.)
    + I have also included a few other packages that we often use in this list
    + however, you should remove any packages from this list that you don't actually use!!
* usually, I don't have any text above the code chunk for package loading as it is self explanatory
    + however, if you are loading non-standard packages directly from GitHub or another non-CRAN source, you may want to include a note about it
    
## Package management using `renv`

* package conflicts are one source of non-reproducibility, as well as future issues and collaborator conflicts with your code
    + if you are using one version of a package, say, and a collaborator is using another, you may get different results or code may not function
    + if anyone (including yourself) goes back to an analysis file you had set up previously, and some of the packages have been updated in the meanwhile, it may not work or may generate different results
* one straightforward way to manage packages is with the `renv` package
    + more at https://rstudio.github.io/renv/articles/renv.html 
    + when you initialize `renv` with your project, it creates a "snapshot" of the *exact versions of packages* that you are using
    + because CRAN is an archive, it includes *pointers* to those exact package versions on CRAN, rather than storing the entire packages along with the project
    + a version of each exact package is stored on your local machine (but they have to be anyway for your code to function)
* **workflow is easy** in `renv` (I promise!)
    + initialize your project once and only once using `renv::init()`
    + anytime any packages are added or removed, call `renv::snapshot()` to update the "lockfile" of which packages `renv` is managing
    + most of the time, that is all you will need to do!
    + if you do run into package conflicts / issues, you can call `renv::restore()` to go back to the set of packages and versions that you previously were using
* to change your mind about `renv`... (usually not an issue, just in case)
    + if you were playing around with `renv` and run into errors / problems related to `renv` itself (e.g., you initialize a project but then accidentally delete the lockfile, etc.), you can stop `renv` from managing your packages in a project by calling `renv::deactivate()`
    + if you deactivate `renv` from a project, but you've still kept the lockfile etc., you can reactivate it as well via `renv::activate()` 



```r
# IF USING RENV TO MANAGE PACKAGES (recommended)
# make sure `renv` package is installed: `install.packages("renv")`
# once and only once, call:
# renv::init()
# once that is done, delete the code on this line and above
# (delete the above code if not using `renv`)

# CREATE PACKAGE LIST:
# (ADD / delete the packages you need / don't below!)
Packages <- c("tidyverse", "knitr", "kableExtra", "glmmTMB", "broom.mixed",
              "car", "evaluate") 

# LOAD PACKAGES:
lapply(Packages, library, character.only = TRUE)

# SAVE PACKAGE WARNING MESSAGES
# this saves messages associated with package loading; display at the end
# `evaluate` package
package.loading.messages = evaluate(lapply(Packages, library, character.only = TRUE))

# # SNAPHOT packages for `renv`
# # if using renv, then run the line of code below whenever packages change
# # ...but ONLY after making sure that the code is running and packages are
# # playing nicely with one another
# renv::snapshot() # leave this commented out, run as needed

# # RESTORE packages using `renv`
# # only use this if you have added new packages or changed package versions
# # and your code is now not working
# renv::restore() # leave this commented out, only use if needed (rare)

# # DEACTIVATE / ACTIVATE `renv`
# if you need to deactivate `renv` from your project:
# renv::deactivate() # leave this commented out, only use if needed (rare)
# if you want to re-activate `renv`:
# renv::activate() # leave this commented out, only use if needed (rare)

# (delete code relating to snapshot / restore / deactivate if not using `renv`)
```

<!-- keep the code below; it prints the packages you used in the Rmarkdown report -->
<!-- but you probably want to delete these comments :) -->

**Packages used:**

tidyverse, knitr, kableExtra, glmmTMB, broom.mixed, car, evaluate

# Data

## data import

* describe details of each dataset and where it comes from here (before the code chunk)
* to import your data:
    + copy each dataset to be input into the `data-in` subfolder in your repository
    + your input data should never be anywhere else in the project
    + **NEVER EVER** use an absolute file path (e.g., `C:/Windows/Bobsdirectory/dataset1.csv`) anywhere, but they are most common in data import code
    + instead, **ALWAYS** use a relative file path, typically `read.csv("data-in/mydata")` if the name of the data file you added was "mydata"---but always leaving the `data-in/` prefix in the path
    + repeat in the code chunk below for every dataset you will be importing for your analysis
* in general, try to avoid ever setting the working directory (`setwd()`)---if you are using an R project, then it will set the working directory for you, and you can specify subfolders in the line of code where you are reading from or writing to them

Once you have imported your data:

* it is absolutely worth checking out your dataset in various ways, and doing it here (i.e., upon importing)
* But that *doesn't* mean that those checks should be printed out in your report---they probably shouldn't, they will make the report feel cluttered
    + one thing I have done is to put in the code for my checks, but to comment it out (e.g., `# head(dat)` or `# str(iris)` or `# View(iris)`) so that it doesn't clutter up the report, but someone examining the code can see that I did go through and check it out
    + still, I usually delete these lines of code just to keep things clean and streamlined



```r
# FIRST THING: CHANGE CODE CHUNK HEADER TO REMOVE `eval = F`!

# start by removing the `Packages` variable from your environment
# (you can't do this in the code chunk above because you display the list
# of package names after that chunk)
rm(Packages)

# this is where you import your data
# change as needed with your data names
mydat <- read.csv("data-in/mydat.csv")
myotherdat <- read.csv("data-in/myotherdat.csv")

# repeat the above for each dataset to be imported

# # do some simple exploration here to make sure your data are as expected
# # keep it commented out, so as to keep your report uncluttered
# head(mydat)
# str(mydat)
# View(mydat)
# you are welcome / encouraged to delete that code after having
# done some basic checks on your data

# cleanup
# typically you won't have anything else to clean up here, but just in case...
```

* if there are any important things to note about the dataset after importing, formatting, please note that here
    + that said, it is not necessary to include any text here if there isn't anything that is noteworthy
* do **NOT** print out a whole dataset into your report unless it is really tiny, AND there is a good reason to show it!
* if there is something important for the readers / users of the report to note about the structure of the data, perhaps you can show the first few lines of the data frame, but don't do more than that unless there is a really good reason
* if you must show part of the data, use the `kable` function (from the `knitr` package), along with formatting from `kableExtras` to make it prettier / easier to read than base R

## data formatting

Data formatting, cleaning, and munging is typically the most time-consuming part of any analysis. Much of the time it makes sense to divide this into multiple code chunks, don't hesitate to do so if it makes it logically easier to follow, or if it makes it easier to code.

Include some text here giving an overview of what needs to be done in the chunk or chunks below. If you do use multiple code chunks, include at least a little bit of text before each chunk alerting the reader what that chunk is doing.

Often, we will merge multiple disparate datasets together. If this happens, and one or more of the input datasets is not used again, my preference is to remove those datasets to keep the analysis environment streamlined (following the "workspace / environment philosopy" text above).

If there are columns in a dataset that are never used, my preference is to delete them, again to keep things clear and simple; this is where you want to do that. Using `dplyr::select()` is a good way to do that.


```r
# this is where you format your data

# again, consider using multiple code chunks

# cleanup
# (always clean up after yourself)
```

# Analysis 

Give an overview / roadmap here of what the various big-picture analyses are, e.g.:

* analysis 1 covers this aspect of the project
* analysis 2 covers this other thing
* analysis 3 addresses something different... 

sometimes it can be helpful to give a little bit of information on the philosophy behind your analysis choice. E.g., "an alternative way to analyze these data would be to instead do Y; we chose to do X because of reason 1, reason 2, and reason 3... "

Though if that philosophy is specific to particular analysis components, include only in the components where it is relevant.

## Data overview

In most of the empirically focused papers we write, the Results section starts off with a short subsection usually entitled something like "overview". Thus, it's good to have some code here that does basic calculations of things like how many samples you collected, how many species of plants and pollinators, how many interactions, how many seeds you counted, etc.

In this context, I think it is helpful to put **in-line code** into the **text** section of the report (i.e., not in a chunk) so that the results can be, well, contextualized. For example, we could report on the mean sepal width in the `iris` dataframe that is included with R:

* mean sepal width in the `iris` dataframe: 3.0573333

Sometimes it makes sense to include a code chunk (i.e., not just inline code) here, if needed to make more complex calculations; anything more than a couple of operations is typically best left to a code chunk, and can then be reported on in the text below the code chunk. The chunk below includes code from Loy et al.'s accelerated snowmelt and seed production analysis; the lines of codes included outside of the chunk (in the default Rmarkdown text) are repeated within the chunk (but commented out and thus not evaluated in the chunk). While this is by definition repetitive, it makes it easy for a reader of the Rmarkdown report to see how the calculations were conducted, without having to separately open the .Rmd file.


```r
# FIRST THING if altering: CHANGE CODE CHUNK HEADER TO REMOVE `eval = F`!

# this code is just an example of one way this could be done; change to best suit the purposes of your project.

# most of this is displayed in text below the code chunk

# Number of plants
# * `r nrow(masterdat)` plants

# Number of plants per treatment
plants.per.treat = masterdat %>% group_by(plot.treat) %>%
  summarise(no_rows = length(plot.treat))
    # + `r as.numeric(plants.per.treat[2,2])` plants in control plots
    # + `r as.numeric(plants.per.treat[1,2])` plants in snowmelt-accelerated plots

# Total number of seeds
# * `r as.integer(sum(masterdat$totalseeds))` total seeds
# "as.integer" makes it so that it does not display in scientific notation
```

## Analysis component 1

Now we move on to the actual data analyses...

* it is **really important** to give detailed context to each analysis chunk; for example:
    + what does this chunk do?
    + why this particular analysis?
    + what question is it answering?
    + what are the specifics involved, e.g. specification of random effects, other components like zero-inflation, etc.
* if you found information online that helped you with the analysis, I strongly suggest including links to it here; likewise, if you were following the analysis in a paper, I would include that here as well
* It is also **critically important** to edit this information as your analysis is changed / updated
* a minor point is to consider the use of `suppressWarnings()` if one or more operations in your code is generating warning messages that are not particularly helpful and which are cluttering up your report
    + BUT: if the warnings are helpful or potentially important, don't do that!
    + as noted above, you can also do this with `warnings = F` in the code chunk header

In addition, for many if not most analyses, we typically have to do a little bit of additional formatting that isn't in the "data formatting" section above. That is perfectly reasonable and often makes the report easier to follow logically. If you are doing that, however, please be sure to include a little bit of text outlining the formatting changes to be made here.


```r
# FIRST THING: CHANGE CODE CHUNK HEADER TO REMOVE `eval = F`!

# put any code related to data formatting for an analysis here

# this is where you would put your analysis code

# STREAMLINE MODEL RESULTS
# (for models including categorical factors with many levels...
# ...especially if there are interaction terms,
# we typically simplify model results using the
# `Anova` function from the `car` package)
aov.my.model = Anova(my.model)

# DISPLAY MODEL RESULTS
aov.my.model

# SAVE MODEL RESULTS (often but not always want to do this)
# this is in addition to displaying them in the Rmarkdown report
# for mixed-effects models, the `broom.mixed` package is very helpful for that:
out.aov.my.model = tidy(out.aov.my.model)
write.csv(out.aov.my.model, file = "results/my-model-results.csv")
# **IMPORTANT:** note the "results/" at the start of the file path, 
# which saves the output into the "results" folder in our project (key!)

# CLEANUP
# if there are temporary variables, etc. that were generated in this chunk,
# remove them here, e.g. the objects created by `broom.mixed`:
# (uncomment out the one line of code below, alter as needed)
rm(aov.my.model, out.aov.my.model)
# ...but don't remove your model objects yet, because you need to do model
# assessment on them (next code chunk)
```

* after each analysis, **it is really (really) important** to include some text about what the results mean
* discuss *directionality* of results, not just whether or not differences are significant; and also ideally effect sizes
* ideally for each analysis, plots and analyses are done near to one another
    + in contrast to doing all the analyses, then all the plots separately

### you can potentially break analyses down into subcomponents with headers

breaking an analysis down into smaller bits makes sense for many analyses... 


```r
# this is where you would put your analysis code

# see code chunk above for more specifics!!

# cleanup
# (always clean up after yourself)
```

...but remember for each of those bits to **put some text here** to give some context about the results!

### model validation, analysis 1

ALWAYS include data validation: do your data meet the assumptions of the models you are running? It's good to include just a bit of text here about your validation, but typically it is very brief.

If you ran an analysis two or more alternative ways, you may wish to split the validation component into multiple chunks. Remember to include some explanatory text, usually before and *always* after each chunk, even if very brief.


```r
# FIRST THING: CHANGE CODE CHUNK HEADER TO REMOVE `eval = F`!

# this is where you would put your code for model validation / assessment

# (for GLMMs, we typically use the `DHARMa` package for this, e.g.
# replace "my.model" with the name of the model you are assessing
# and probably change the name "sim.out.my.model" to something more descriptive)
# simulate residuals using DHARMa:
sim.out.my.model <- simulateResiduals(fittedModel = my.model, plot = F)
plot(sim.out.my.model) # plot residual plots
testDispersion(sim.out.my.model, plot = F) # print dispersion results
testZeroInflation(sim.out.my.model, plot = F) # print zero-inflation results

# CLEANUP
# again, remove anything that isn't subsequently used in the analysis
rm(sim.out.my.model)
```

as always, remember for each of those bits to **put some text here** to give some context about the validation results! Just showing a validation plot without any context is unhelpful. Tell the reader / your future self *why* your plot (or other result) is consistent (or not!) with model assumptions.

### plots analysis 1

* I strongly prefer to have plots associated with each analysis, near each analysis in the Rmarkdown document (as opposed, e.g., to putting all of the analyses together at the end)
    + I think it makes it easier to read / interpret the analysis report, but also makes it easier to code.
* give just a bit of brief context for each plot, before the code chunk in which you execute the plot
* when saving plots, **be sure to save them in the `plots` subfolder** (see code in the chunk below)

**saving plots and** `.gitignore`

* this project has been set up with the `plots/` subfolder included in `.gitignore`
* thus, any plots you save as part of the project (assuming you save them in the `plots/` subfolder, as you always should!) will be saved on your local computer, but not on GitHub
    + the rationale behind this is that Git does not do well with versioning binary files like image files / .pdfs / Word & Excel docs; thus it is re-writing the *entire* file every time it gets updated, even with a tiny change
* you should display at least the key plots in your .Rmarkdown report, so they will be viewable by anyone checking out the project
    + if anyone wants to generate the exact saved plots (i.e. the graphics files) that are in the `plots/` subfolder, the code should be set up to be reproducible so that should be straightforward



```r
# FIRST THING: CHANGE CODE CHUNK HEADER TO REMOVE `eval = F`!

# any data preparation / summarization needed for plotting
# code below is just a placeholder!
myplotdata = mydata %>% group_by(mygroupingvar1, mygroupingvar1) %>% 
  summarize(mean = mean(), stdev = sd()...) 

# create the actual plot (almost always using `ggplot2`)
myplot = ggplot(myplotdata, ...)

# DISPLAY the plot in your Rmarkdown report 
# (usually just a single line of code, 
# the name of the ggplot object you created above)
myplot

# SAVE PLOT
# example, but key thing is the `plots/` prefix in the file path
ggsave(myplot, file = "plots/myplot.pdf", width = 5, height = 3)

# # CLEANUP
# # remember to also clean up any objects such as data summarizations
# # set up for your plotting, e.g.:
rm(myplotdata, myplot)
```

you may wish to include some brief text here, e.g., "this plot shows the negative overall effect of potassium cyanide exposure on bee survival, underscoring our statistical results"

## Analysis 2

Repeat as needed for each analysis component (analysis 2, 3...). remember to include:

* analysis chunk(s)---that's the easy part
* model validation
* plots associated with each analysis

These should all occur together, to keep the overall analysis report easy to read and understand.

**IMPORTANT**---always remember to check that every code chunk has a unique name, otherwise your Rmarkdown report will not knit. This is particularly important when copying and pasting code chunks.

# Session Info and Package Loading Messages

`sessionInfo` is key (though minimally) for reproducibility---replicate the code chunk below exactly

before `sessionInfo` we print the messages associated with package loading, using the `replay` function from the `evaluate` package. We do this at the end to make the report more readable, such that readers do not have to wade through long lists of (typically) not-very-helpful messages. But for troubleshooting those messages can be important, so they are listed here.


```r
replay(package.loading.messages)
```

```
## > list(c("evaluate", "car", "carData", "broom.mixed", "glmmTMB", 
## + "kableExtra", "knitr", "forcats", "stringr", "dplyr", "purrr", 
## + "readr", "tidyr", "tibble", "ggplot2", "tidyverse", "stats", 
## + "graphics", "grDevices", "utils", "datasets", "methods", "base"
## + ), c("evaluate", "car", "carData", "broom.mixed", "glmmTMB", 
## + "kableExtra", "knitr", "forcats", "stringr", "dplyr", "purrr", 
## + "readr", "tidyr", "tibble", "ggplot2", "tidyverse", "stats", 
## + "graphics", "grDevices", "utils", "datasets", "methods", "base"
## + ), c("evaluate", "car", "carData", "broom.mixed", "glmmTMB", 
## + "kableExtra", "knitr", "forcats", "stringr", "dplyr", "purrr", 
## + "readr", "tidyr", "tibble", "ggplot2", "tidyverse", "stats", 
## + "graphics", "grDevices", "utils", "datasets", "methods", "base"
## + ), c("evaluate", "car", "carData", "broom.mixed", "glmmTMB", 
## + "kableExtra", "knitr", "forcats", "stringr", "dplyr", "purrr", 
## + "readr", "tidyr", "tibble", "ggplot2", "tidyverse", "stats", 
## + "graphics", "grDevices", "utils", "datasets", "methods", "base"
## + ), c("evaluate", "car", "carData", "broom.mixed", "glmmTMB", 
## + "kableExtra", "knitr", "forcats", "stringr", "dplyr", "purrr", 
## + "readr", "tidyr", "tibble", "ggplot2", "tidyverse", "stats", 
## + "graphics", "grDevices", "utils", "datasets", "methods", "base"
## + ), c("evaluate", "car", "carData", "broom.mixed", "glmmTMB", 
## + "kableExtra", "knitr", "forcats", "stringr", "dplyr", "purrr", 
## + "readr", "tidyr", "tibble", "ggplot2", "tidyverse", "stats", 
## + "graphics", "grDevices", "utils", "datasets", "methods", "base"
## + ), c("evaluate", "car", "carData", "broom.mixed", "glmmTMB", 
## + "kableExtra", "knitr", "forcats", "stringr", "dplyr", "purrr", 
## + "readr", "tidyr", "tibble", "ggplot2", "tidyverse", "stats", 
## + "graphics", "grDevices", "utils", "datasets", "methods", "base"
## + ))
## [[1]]
##  [1] "evaluate"    "car"         "carData"     "broom.mixed" "glmmTMB"    
##  [6] "kableExtra"  "knitr"       "forcats"     "stringr"     "dplyr"      
## [11] "purrr"       "readr"       "tidyr"       "tibble"      "ggplot2"    
## [16] "tidyverse"   "stats"       "graphics"    "grDevices"   "utils"      
## [21] "datasets"    "methods"     "base"       
## 
## [[2]]
##  [1] "evaluate"    "car"         "carData"     "broom.mixed" "glmmTMB"    
##  [6] "kableExtra"  "knitr"       "forcats"     "stringr"     "dplyr"      
## [11] "purrr"       "readr"       "tidyr"       "tibble"      "ggplot2"    
## [16] "tidyverse"   "stats"       "graphics"    "grDevices"   "utils"      
## [21] "datasets"    "methods"     "base"       
## 
## [[3]]
##  [1] "evaluate"    "car"         "carData"     "broom.mixed" "glmmTMB"    
##  [6] "kableExtra"  "knitr"       "forcats"     "stringr"     "dplyr"      
## [11] "purrr"       "readr"       "tidyr"       "tibble"      "ggplot2"    
## [16] "tidyverse"   "stats"       "graphics"    "grDevices"   "utils"      
## [21] "datasets"    "methods"     "base"       
## 
## [[4]]
##  [1] "evaluate"    "car"         "carData"     "broom.mixed" "glmmTMB"    
##  [6] "kableExtra"  "knitr"       "forcats"     "stringr"     "dplyr"      
## [11] "purrr"       "readr"       "tidyr"       "tibble"      "ggplot2"    
## [16] "tidyverse"   "stats"       "graphics"    "grDevices"   "utils"      
## [21] "datasets"    "methods"     "base"       
## 
## [[5]]
##  [1] "evaluate"    "car"         "carData"     "broom.mixed" "glmmTMB"    
##  [6] "kableExtra"  "knitr"       "forcats"     "stringr"     "dplyr"      
## [11] "purrr"       "readr"       "tidyr"       "tibble"      "ggplot2"    
## [16] "tidyverse"   "stats"       "graphics"    "grDevices"   "utils"      
## [21] "datasets"    "methods"     "base"       
## 
## [[6]]
##  [1] "evaluate"    "car"         "carData"     "broom.mixed" "glmmTMB"    
##  [6] "kableExtra"  "knitr"       "forcats"     "stringr"     "dplyr"      
## [11] "purrr"       "readr"       "tidyr"       "tibble"      "ggplot2"    
## [16] "tidyverse"   "stats"       "graphics"    "grDevices"   "utils"      
## [21] "datasets"    "methods"     "base"       
## 
## [[7]]
##  [1] "evaluate"    "car"         "carData"     "broom.mixed" "glmmTMB"    
##  [6] "kableExtra"  "knitr"       "forcats"     "stringr"     "dplyr"      
## [11] "purrr"       "readr"       "tidyr"       "tibble"      "ggplot2"    
## [16] "tidyverse"   "stats"       "graphics"    "grDevices"   "utils"      
## [21] "datasets"    "methods"     "base"
```

```r
sessionInfo()
```

```
## R version 4.1.1 (2021-08-10)
## Platform: aarch64-apple-darwin20 (64-bit)
## Running under: macOS Big Sur 11.6
## 
## Matrix products: default
## BLAS:   /Library/Frameworks/R.framework/Versions/4.1-arm64/Resources/lib/libRblas.0.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/4.1-arm64/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
##  [1] evaluate_0.14     car_3.0-11        carData_3.0-4     broom.mixed_0.2.7
##  [5] glmmTMB_1.1.2.3   kableExtra_1.3.4  knitr_1.34        forcats_0.5.1    
##  [9] stringr_1.4.0     dplyr_1.0.7       purrr_0.3.4       readr_2.0.1      
## [13] tidyr_1.1.3       tibble_3.1.4      ggplot2_3.3.5     tidyverse_1.3.1  
## 
## loaded via a namespace (and not attached):
##  [1] nlme_3.1-153        fs_1.5.0            lubridate_1.7.10   
##  [4] webshot_0.5.2       httr_1.4.2          numDeriv_2016.8-1.1
##  [7] tools_4.1.1         TMB_1.7.21          backports_1.2.1    
## [10] bslib_0.3.0         utf8_1.2.2          R6_2.5.1           
## [13] DBI_1.1.1           colorspace_2.0-2    withr_2.4.2        
## [16] tidyselect_1.1.1    emmeans_1.6.3       curl_4.3.2         
## [19] compiler_4.1.1      cli_3.0.1           rvest_1.0.1        
## [22] xml2_1.3.2          sandwich_3.0-1      sass_0.4.0         
## [25] scales_1.1.1        mvtnorm_1.1-2       systemfonts_1.0.2  
## [28] digest_0.6.27       foreign_0.8-81      minqa_1.2.4        
## [31] rmarkdown_2.11      svglite_2.0.0       rio_0.5.27         
## [34] pkgconfig_2.0.3     htmltools_0.5.2     lme4_1.1-27.1      
## [37] dbplyr_2.1.1        fastmap_1.1.0       rlang_0.4.11       
## [40] readxl_1.3.1        rstudioapi_0.13     jquerylib_0.1.4    
## [43] generics_0.1.0      zoo_1.8-9           jsonlite_1.7.2     
## [46] zip_2.2.0           magrittr_2.0.1      Matrix_1.3-4       
## [49] Rcpp_1.0.7          munsell_0.5.0       fansi_0.5.0        
## [52] abind_1.4-5         lifecycle_1.0.0     stringi_1.7.4      
## [55] multcomp_1.4-17     yaml_2.2.1          MASS_7.3-54        
## [58] grid_4.1.1          crayon_1.4.1        lattice_0.20-44    
## [61] haven_2.4.3         splines_4.1.1       hms_1.1.0          
## [64] pillar_1.6.2        boot_1.3-28         estimability_1.3   
## [67] codetools_0.2-18    reprex_2.0.1        glue_1.4.2         
## [70] data.table_1.14.0   modelr_0.1.8        vctrs_0.3.8        
## [73] nloptr_1.2.2.2      tzdb_0.1.2          cellranger_1.1.0   
## [76] gtable_0.3.0        assertthat_0.2.1    openxlsx_4.2.4     
## [79] xfun_0.26           xtable_1.8-4        broom_0.7.9        
## [82] coda_0.19-4         survival_3.2-13     viridisLite_0.4.0  
## [85] TH.data_1.0-10      ellipsis_0.3.2
```

