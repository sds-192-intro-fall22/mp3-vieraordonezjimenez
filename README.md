# Mini-Project 3

# Overview

In this mini-project, you will analyze the Environmental Protection Agency's Toxic Release Inventory to profile a "fenceline community" in the United States. Fenceline communities are neighborhoods in close proximity to high-risk chemical facilities, and are often sites of environmental racism and other environmental justice concerns. Your analysis will include a series of maps that help us understand the risks the community faces and you will be expected to iterate a self-defined function in your analysis. The final output of the project can be in whatever format you wish (a blog post, a report, a poster, a video, etc) as long as it renders in quarto and you produce 400-500 words of text summarizing and interpreting your findings. **Be sure to format your rendered files using techniques you learned in SDS 100!**

# Learning Goals

* Navigate different forms of data documentation
* Prepare data for analysis, using pivots and other cleaning functions
* Write functions for data analysis and iterate over them
* Produce both point-based and polygon maps of geospatial data
* Understand the complexities of layering data recorded at different geographic scales

### A Note on the Git Flow

While not a formal part of the assessment for this lab, I do expect that you will establish a Git workflow with your team for this assignment. You may use the workflow from MP2 as a model, and I would encourage you to determine within your group which line numbers different group members will work on. I do expect to see commits from all group members. 

# Detailed Instructions

## Get to know the TRI data

1. Watch:

[![Toxic Release Inventory](http://img.youtube.com/vi/Fqjh6t6Hx6s/0.jpg)](http://www.youtube.com/watch?v=Fqjh6t6Hx6s)

[![TRI for Communities](http://img.youtube.com/vi/Hj3yGpe_s-8/0.jpg)](http://www.youtube.com/watch?v=Hj3yGpe_s-8)

2. Read about the Toxic Release Inventory Program [here](https://www.epa.gov/toxics-release-inventory-tri-program/what-toxics-release-inventory).

3. **Very Important**: Read Factors to Consider [memo] (https://www.epa.gov/system/files/documents/2022-02/factorstoconsider_approved-by-opa_1.25.22-copy.pdf)

4. Review the [data documentation](https://www.epa.gov/system/files/documents/2021-10/tri-basic-data-file-documentation-ry2020_100721.pdf). Specifically, the data dictionary for this dataset spans pages 5-18. 

> **It will be important for you to know that the CRS for TRI data is 4269.**

## Select and Research a US Fenceline Community

5. Ultimately, you should select one US **county** for your analysis. Some of you may know of examples of current or historic fenceline communities in the US, and others may wish to learn more about those communities. 

> Note that later in the assignment you will be asked to visualize census data with at least two other data sources on a map. You may wish to visualize the data with historic redlining maps, and if so, you should select a county that contains a city for which we have [HOLC Maps](https://dsl.richmond.edu/panorama/redlining/#loc=5/39.1/-94.58&text=downloads) available. Read up about that county online. If you'd like to run your selected county by me, please feel free to do so. 

Here are some resources for researching fenceline communities:

* [Life at the Fenceline](https://ej4all.org/life-at-the-fenceline)
* [WHO’S IN DANGER? Race, Poverty, and Chemical Disasters](https://comingcleaninc.org/assets/media/images/Reports/Who's%20in%20Danger%20Report%20FINAL.pdf)

## Set up your environment

6. In RStudio, `File` > `New Project` > `Version Control` > `Git` and then copy the URL to this repo. Open `tri_analysis.qmd` and add your group member's names to the header (lines 5, 7, and 9).

7. Navigate to the [TRI Basic Data Files](https://www.epa.gov/toxics-release-inventory-tri-program/tri-basic-data-files-calendar-years-1987-present), and download the 2020 file for the state your county is located in. 

8. Move the CSV file into the `dataset` folder on your local machine. **Note that only one student in your group needs to do this.** The dataset can be pushed to other members of your group.

9. In `fenceline_analysis.qmd`, read the CSV file you just downloaded into a data frame. Be sure to use a descriptive variable name for your data frame. 
  - Filter the data to your county. 
  - As you read in the documentation, quantitative values are sometimes reported in pounds and and sometimes reported in grams in this dataset. Convert all quantitative values to a common unit of measure with a single line of code. 

### A Note on the Unit of Observation in TRI

This is a multi-dimensional dataset. Each row indicates a quantity of releases, but this is not the total quantity of releases for a facility. It is the total quantity *of a particular chemical* for a facility. This means that the same facility will be repeated multiple times in the dataset - once for each toxic chemical it released in 2020. It also means that you need two variables to uniquely identify each row - the facility and the chemical. It's really important to keep this in mind when performing any data wrangling or creating any visualizations with this dataset. Check out the example below to see why. 

| facility      | industry         | chemical | releases |
|---------------|------------------|----------|----------|
| blue facility | electronic waste | benzene  | 2        |
| blue facility | electronic waste | ammonia  | 3        |
| red facility  | mining           | benzene  | 4        |
| red facility  | mining           | ammonia  | 5        |

Let's say you wanted to know the number of facilities per industry type in this dataset. If I just counted the number of times each industry type appeared in the `industry` column, `R` would indicate that there are two electronic waste facilities and two mining facilities. But this is not the case, right? There is only one electronic waste facility (the blue facility), and only one mining facilty (the red facility). They are separated across multiple rows because they released different types of toxic chemicals and reported releases for each. You'll need to perform some kind of data aggregation in this case to get an accurate count. 

## Clean Data and Perform Analysis

10. There are a number of ways you can analyze the data in this project, but there are three criteria that must be met:

- Your analysis must include at least one pivot.
- Your analysis must iterate a self-defined function over multiple rows or variables.
- Your analysis must include at least two chloropleth maps overlaying TRI data on other data sources. 

As long as these three criteria are met, there is not a minimum number of analyses or code chunks in this assignment. For example, you might decide to iterate a function that includes a pivot or that creates a map in order to meet two criteria at once. Or you might decide that you want to meet each of these three criteria in separate analyses. Either approach is fine. Below, I will provide a few hints to help you think about how you might going about meeting these criteria.

### Pivot Hints

This dataset documents information about releases of toxic chemicals. What kind of critical information about different kinds of releases is stored in column names in this dataset? When might you want to compare different kinds of releases on a plot or in a table?

### Function/Iteration Hints

Because this is a multi-dimensional dataset, one approach to this assignment would be to iterate a function to perform some data analysis for each facility in the dataset or for each chemical in the dataset. 

### Mapping Hints

It's up to you to find other data to map on this plot, but I will provide three options that might be interesting:

- [Race or ethnicity data via `tidycensus`](https://walker-data.com/tidycensus/articles/basic-usage.html)
  - Note that getting this set-up can be non-trivial, so reach out to me if you need help
- [Historic Redlining Maps](https://dsl.richmond.edu/panorama/redlining/#loc=5/39.1/-94.58&text=downloads)]
- [CDC's Places Dataset](https://chronicdata.cdc.gov/500-Cities-Places/PLACES-Local-Data-for-Better-Health-Census-Tract-D/cwsq-ngmh)

...and be absolutely sure that all data is being mapped with a common CRS!

## Present your analysis

10. You can choose how you want to present your analysis in this final mini-project as long as it renders in quarto! Feel free to get creative. You could produce a blog post as we have in the last assignments. ...or you could record a video summarizing your findings, post it to YouTube, and embed it in quarto. ...or you could experiment with presenting your findings as a quarto presentation instead of a webpage. 

...but you must produce 400-500 words to help summarize and interpret the findings in your analysis, and you must at some point touch upon ethical issues related to data collection or presentation. 
    
## Record standards and submit assignment

11. Open `standards.qmd`, and under each heading, indicate how the work you completed for this project demonstrated fluency in that standard. You may wish to reference the Evaluation section below for help with writing this up. 
12. Open `contributions.qmd` and briefly describe each team member's contributions to the project.
13. When you are done, you should save all .qmd files, render the documents, commit changes, and then push changes back to GitHub. That's it for submission. You don't need to submit anything on Moodle. 

# Evaluation 

You will be evaluated on the extent to which your mini-project demonstrates fluency in the following course learning dimensions:

* Tidying Data:
  * 1 point - Demonstrates an ability to import local data
  * 1 point - Demonstrates an ability to `mutate` across mulitple columns
  * 1 point - Demonstrates an ability to perform a pivot
* Programming in R
  * 1 point - Demonstrates an ability to write a custom function
  * 1 point - Demonstrates an ability to iterate a function over multiple values in a vector
  * 1 point - Demonstrates an ability to discern different data object types in `R`
* Mapping
  * 1 point - Demonstrates an ability to produce point and/or polygon-based maps with descriptive title and labels
  * 1 point - Demonstrates an ability to transform data to an appropriate CRS
  * 1 point - Demonstrates an ability to create informative color palettes for points or polygons on a map
  

