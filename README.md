# analysis-template

template repository for data analyses in the Brosi Lab

When using this as a template, be sure to update this README file, replacing all of the text except for the folders in the "file structure"" section. In particular, include:

* authors / contributors
* when the analysis is published, a citation and link to the paper
* a brief description of where the data are from
* a brief description of what the analysis is getting at 

**File structure:**

* **main folder** contains:
    + R project file ("___.Rproj")
    + Rmarkdown file ("___.Rmd")
    + Rmarkdown html report ("___.html")
* **data-in subfolder** contains the input data files:
    + "___.csv" (data related to...)
    + "___.csv" (data related to...)
    + (etc)
* **data-out subfolder** contains data that were formatted / subset / cleaned / etc. in this analysis, that are saved for other analyses in the future:
    + NOTE: this folder is optional and may need to be deleted from some projects
    + "___.csv" (data related to...)
    + (etc)
* **results subfolder** contains tables with model results
    + "___.csv" (model results from...)
    + "___.csv" (model results from...)
    + (etc)
* **plots subfolder** has saved plots from the analysis

