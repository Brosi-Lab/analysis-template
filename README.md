# analysis-template

template repository for data analyses in the Brosi Lab

When using this as a template, be sure to update this README file, replacing all of the text except for some of the text related to the folders in the "file structure"" section. 

In particular, you will want to include text on:

* authors / contributors
* when the analysis is published, a citation and link to the paper
* a brief description of where the data are from
* a brief description of what the analysis is getting at 

**File structure:**&mdash;YOU WILL NEED TO CHANGE SOME THINGS! read the below carefully:

* the **main project folder** (not a subfolder) should ONLY contain:
    + R project file ("___.Rproj")&mdash;this will be renamed automatically following your project setup (you shouldn't have to do anything)
    + Rmarkdown file ("___.Rmd")&mdash;you should **rename the file** called `Rmarkdown-template-Brosi-lab.Rmd` once you start your new project; this will become your primary analysis file
    + Rmarkdown html report ("___.html")&mdash;**delete this file** (`Rmarkdown-template-Brosi-lab.html`) once you start your new project; when you knit your .Rmd file to create a report, it will generate a new .html file reflecting your updated name for the .Rmd file
    + .gitignore (automatic; don't mess)
    + .DS_Store (automatic; don't mess)
    + in addition, you may include one "sandbox" coding file, that you can play around in without worrying about making a mess (and also e.g. store deleted code temporarily, etc.). This can be a .R file or .Rmd; you'll have to manually add this if you want it
* **data-in subfolder** should contain the data files you will import into your project:
    + DELETE the "DELETE ME.txt" file included here
    + copy into this folder the data you will input
* **data-out subfolder** contains data that were formatted / subset / cleaned / etc. in this analysis, that are saved for other analyses in the future:
    + DELETE the "DELETE ME.txt" file included here
    + NOTE: this folder is optional and may need to be deleted from some projects; WAIT to delete it until you are pretty sure you won't be saving any data for export
* **results subfolder** contains tables with model results
    + DELETE the "DELETE ME.txt" file included here
    + this one might also be optional if you don't want to save any, say, output tables from analyses, but again WAIT to delete until you are sure you won't use
* **plots subfolder** has saved plots from the analysis
    + DELETE the "DELETE ME.txt" file included here
    + save plots here; it is rare to have an analysis where you don't save any plots but if you find yourself in that situation, you may delete this subfolder
    
