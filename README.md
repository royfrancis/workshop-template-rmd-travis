# workshop-template-rmd-travis [![travis-build](https://travis-ci.org/royfrancis/workshop-template-rmd-travis.svg?branch=master)](https://travis-ci.org/royfrancis/workshop-template-rmd-travis)

This repo is a template for Rmarkdown-driven workshop website that uses Travis-CI for automated rendering. The rendered view of this repo is available [here](https://royfrancis.github.io/workshop-template-rmd-travis/).

## File descriptions
### Repo related files

|Filename|Type|Description|
|---|---|---|
|`_site.yml`|yml|Website config|
|.travis.yml|yml|Travis-CI config|
|deploy.sh|sh|Travis-CI script|
|README.md|md|This document|
|DESCRIPTION|-|Travis-CI R build config|
|LICENSE|-|Repo usage license|

### Workshop content files

|Filename|Type|Description|
|---|---|---|
|index.Rmd|Rmd|Home page|
|schedule.csv|csv|Schedule data|
|home_schedule.Rmd|Rmd|Schedule page|
|home_info.Rmd|Rmd|Practical info|
|home_precourse.Rmd|Rmd|Precourse instructions|
|home_lab.Rmd|Rmd|All labs|
|slide_topic.Rmd|Rmd|Slide files for topics|
|lab_topic.Rmd|Rmd/md|Lab files for topics|
|assets|Folder|Shared assets|
|data|Folder|Shared data|
|other|-|Other files can include pdf, pptx etc|

## Updating

Fork/clone the repository. Only work in the master branch.

`git clone github-link`

### Rerun a workshop

If this repo is updated for a different date and location, this is minimum changes required.

1. Update **`_site.yml`**
    - **Set argument `output_dir:` to a year-month (YYMM) combination like 1908**
    - Set `uppmax_project` if needed. This is used in **home_precourse.Rmd**
    - Check arguments `name:` and `title:`
    - Check `location:`. This affects details displayed in **home_info.Rmd**.
2. Update **index.Rmd**
    - Check `title:` and `subtitle:`
    - Check instructions, descriptions and links
3. Update **schedule.csv**
    - This table holds the schedule information
    - Open/edit in a spreadsheet or text editor
    - Columns are delimited by `;`
    - Do not change the number of columns, position of columns, column names or date format
    - Rows can be freely added or removed
    - Set date, room, start_time, end_time, topic and person as needed
    - *date*: Full date for each day in format dd/mm/yyyy. Missing/empty cells are filled down automatically
    - *room*: (Optional) Room number for the workshop. Missing/empty cells are filled down automatically
    - *start_time/end_time*: Start and end time for session (Eg: 09:00) 
    - *topic*: Topic name (Keep it short)
    - *teacher*: Name of the person covering the topic
    - *assistant*, *`link_slide`*, *`link_lab`* and *`link_room`* are optional. If included, it will show up on the schedule
    - *assistant* is optional for listing TAs
    - *link_slide*: (Optional) Link to the presentation. Local links can be like `slide_topic.html`. Use this labelling convention.  
    - *link_lab*: (Optional) Link to the lab material. Local links can be like `lab_topic.html`. This is the labelling convention used.  
    - *link_room*: (Optional) Link to the room location. Can be a google map link, mazemap link etc. External links must start with `http://`
4. Optionally update **home_schedule.Rmd**
5. Optionally update **home_precourse.Rmd** with instructions are needed
    - R packages for students to install are shown here
    - Uppmax ID is retrieved from **`_site.yml`**
    - R packages are retrieved from **`_site.yml`**
6. Optionally check info in **home_info.Rmd**

### Modify contents

If the contents are also updated, further changes are required.

7. Update or create new **slide_** files
    - Presentation material
    - This can be Rmd, PDF, pptx etc.
    - If Rmd, it must use custom YAML header (See an example slide)
    - External data can be added to folders data and images
    - Do not create .md and .Rmd files with same name as they both get converted to .html files
8. Update or create new **lab_** Rmd or md files
    - Lab material
    - Can be Rmd or md
    - Simple YAML header with `title`, and/or `subtitle` `author` is sufficient
    - If table-of-contents is to be hidden, the YAML must be modified. See formatting tips below.
    - External data can be added to the folders **data**
    - Do not create .md and .Rmd files with same name as they both get converted to .html files

> The `assets` directory contains css styles, headers, footers, logos etc. If you are using images in your .Rmd file, place them in the directory `data/topic` and refer to them using relative path like `![](./data/topic/image.jpg)`. Images generated in R during rendering of the .Rmd file is automatically handled. If you have data (tsv, csv, txt text files, .Rds files), place them inside the directory `data/topic` and read them using relative path `x <- read.delim("./data/topic/table.txt")`. Do not use paths that link outside of the project environment.

9. Update **home_content.Rmd**
    - Lists all materials organised by related topics
    - Optionally includes extra materials not under schedule
10. Update R packages in **`_site.yml`**.
11. Update the repo's **README.md** if needed
    - Check year in the bottom

### Create a new workshop

If a new workshop repo is created using this template, the following changes also apply.

12. Enable that GitHub repo in Travis-CI and create an environment variable `GITHUB_TOKEN`
13. Change repo and badge links in **README.md**
14. Change `href` in **`_site.yml`**

These files most likely need not be changed: **.travis.yml**, **deploy.sh**, **LICENSE** and the directory **assets**.

### Push changes

After all changes have been finalised. Commit the changes and push the repo back to GitHub.

```
git add .
git commit -m "Updated contents for Mar 2019 Uppsala"
git push origin
```

Once the source files are pushed to GitHub, it is automatically rendered to the branch gh-pages and website is visible at `org.github.io/repo/`. Details are further described below. For local rendering see below.

### Formatting tips

All .Rmd and .md files by default take the render arguments from `_site.yml` specified under `output: bookdown::html_document2`. The CSS style used is that from the default bootstrap as well as **lab.css**. YAML instructions within each .Rmd or .md file can be used to override the defaults. It is not always necessary to used .Rmd files. .md files are fine to use as long as R related functionality is not needed. The slides for example do not use the default template at all. In fact it completely uses a different output format `moon_reader` and styles from **style.css**.

- For `home_` or `lab_` Rmd files, the table of contents can be turned off by adding the below to YAML.

```
output:
  bookdown::html_document2:
    toc: false
```

- For `home_` or `lab_` Rmd files, numbers before headings/titles can be turned off.

```
output:
  bookdown::html_document2:
    number_sections: false
```

- To enable accordion tabs (for hidden answers), block titles and fontawesome, you need to add this bit AFTER the YAML.

````
```{r,child="assets/header-lab.Rmd"}
```
````

## Local rendering

For local rendering, you need to have R installed on your system. R dependencies listed in `site.yml` under **packages_cran_repo** and **packages_bioc_repo** need to be installed. And then any R packages pertaining to your particular Rmd file(s) (if needed) must be installed.

Run `rmarkdown::render_site()` in the project directory. This renders all Rmd and md files to generate the HTML files and all other necessary files (including the assets, images and data directories) and moves them into a directory specified under `output_dir` in **`_site.yml`**. Open `output_dir/index.html` to start. Remove this directory after use. **DO NOT** commit and push this output directory to GitHub.

For testing purposes, you can run `rmarkdown::render("bla.Rmd")` on individual Rmd/md files. This is a time-saver as the whole website need not be rendered just to preview this one file.

**DO NOT** push any rendered material such as `slide_topic.html`, `lab_topic.html` or supporting directories `slide_topic_files`, `lab_topic_files` etc to GitHub.

## How it all works

![](data/common/versioning.png)

The source content is maintained in the master branch. The source gets a new commit id anytime new content is pushed. The rendered material is maintained on the gh-pages branch under separate folders. These folders have the format YYMM. The contents of this folder is overwritten with a new push unless the directory name is changed (*output_dir* in `_site.yml`).  For convenience, last commit in the master branch for each workshop can be tagged as such **v1911** denoting YYMM. This can be used to easily connect a folder on gh-pages to the commit ID of the source code that produced it.

### Travis-CI

When the committed changes are pushed to GitHub, Travis-CI automatically runs to render the output. The `.travis.yml` config file is used to build a linux container where R and necessary linux dependencies are installed. When completed, the `deploy.sh` script is executed. The first Travis build can take up to 30 mins or more depending on the number of R packages. Subsequent builds take a few minutes depending on the changes because caching is enabled for R packages.

### deploy.sh script

The `deploy.sh` first installs R packages described in `_site.yml`. It then creates a new directory specified under `output_dir` in `_site.yml`. And then render the whole website into it by `rmarkdown::render_site()`. The details of this described below. When the rendering is completed, the contents of gh-pages branch is pulled down to a folder named `tmprepo`. The existence of `output_dir` in `tmprepo` is checked. If already present, it is deleted. The `output_dir` folder is copied into `tmprepo`. Lastly, a list of all folders inside `tmprepo` is added to an index file called `index.md`. This will serve as the root of gh-pages. Finally, all files are added and committed to git and pushed to the gh-pages branch. Git has permission to push to gh-pages due to Travis environment variable `GITHUB_TOKEN`.

### render_site() function

This function uses the information inside the config file `_site.yml`. The top navigation menu is described here. The default output style for all Rmd/md documents are specified under `output:`. Note that this described custom CSS style from `assets/labs.css` and custom footer from `assets/footer-lab.html`. If `output:` is specified within individual Rmd files, it overrides the default in `_site.yml`. The rendered output will automatically be moved to location specified under `output_dir`.

---

**2020** NBIS • SciLifeLab
