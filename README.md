 Introduction
Lesson Material used during the [Data Science for the Public Good Program][2]
at the [Social and Decision Analytics Laboratory][1].

> The Data Science for the Public Good program teaches student fellows
> how to sift through vast amounts of information related to public
> safety, employment, and the provision of services to discover how
> communities can become more efficient and sustainable. Through the
> lenses of statistics, social science, and data science research,
> DSPG students will learn to integrate all available data resources

Many of the lessons will be adapted versions of the corresponding
[Software-Carpentry][3] or [Data Carpentry][4] Lesson materials

<table>
<tr><th>5/22</th><th align="left" colspan="2">Program Introduction</th></tr>
<tr><td></td><td>Overview of SDAL</td><td>Sallie Keller</td></tr>
<tr><td></td><td>Overview of DSPG Program</td><td>Gizem Korkmaz</td></tr>
<tr><td></td><td>Overview of Technology & Training Schedule</td><td>Aaron Schroeder</td></tr>
<tr><th></th><th align="left" colspan="2">Training</th></tr>
<tr><td></td><td>Institutional Review Board (IRB) - Online</td><td></td></tr>
<tr><th>5/23</th><th align="left" colspan="2">Training</th></tr>
<tr><td></td><td style="data-type='html'"><div><a href="./training/system_setup">System Setup</a></div></td><td>Server Access, RStudio Connection, Database Connection</td></tr>
<tr><td></td><td><a href="./training/unix_tools">Unix Tools</a></td><td>Navigating Files and Directories (cd, ls)</td></tr>
<tr><td></td><td></td><td>Working with Files and Directories (mkdir, touch, nano)</td></tr>
<tr><td></td><td></td><td>Shell Scripts (running shell scripts and understanding the working directory)</td></tr>
<tr><td></td><td></td><td>SSH (connecting to a remote server/computer with secure shell)</td></tr>
<tr><td></td><td>Git</td><td>Setting up Git; Creating a repository; Tracking changes; Exploring history; Ignoring Things; Remotes in Github; Collaboration; Conflicts</td></tr>
<tr><th></th><th align="left" colspan="2">Project Overview Presentations</th></tr>
</table>


# Syllabus

The topics that will be covered

## 1. System Setup, Unix Tools & Git
#### Navigating Files and Directories
  - `cd`, `ls`
  
#### Working with Files and Directories
  - `mkdir`, `touch`, `nano`
  
#### Shell Scripts
  - running shell scripts and understanding the working directory
  
#### SSH
  - `ssh` connecting to a remote server/computer

#### Git
  - Setting up Git
  - Creating a repository
  - Tracking changes
  - Exploring history
  - Ignoring Things
  - Remotes in Github
  - Collaboration
  - Conflicts
  
## 2. The Data Science Process & Data Discovery
  - 
  - 

## 3. DataDiscovery, Ingestion, Use & Storage
#### RCUrl
  - `ftp`, `ftps`, `sftp`
  
#### APIs
  - `Google`, `Arlington`

#### Database
  - `SQL`, `DBI/PostgreSQL`

#### Files
  - `csv`, `Excel`, `RData`, `zip`

## 4. Data Objects, Functions & Looping in R
  - `Data.Frame`, `Data.Table`, `Spatial Data.Frames` [point, line, polygon], `Raster`
  - functions
  - for loops vs apply family

## 5. Data Information Management
#### Metadata
#### Provenance
#### Value Mapping
#### Lexicon

## 6. Data Profiling
#### Structure
  - Missing Variables, Combined Variables, Multiple Observation Directions, Combined Observational Unit Types, Divided Observation Unit Type

#### Quality
  - Completeness, Value Validity, Consistency, Uniqueness, Duplication

## 6. Data Preparation & Linkage
#### Cleaning
#### Transformation
#### Restructuring
#### Linking
  - Joins / Merges (Deterministic, Probabilistic)
  - String Matching (Insecure, Secure)

## 7. Data Exploration

## 8. Data Analysis / Modeling

## 9. Data Presentation & Reporting
  - `Shiny`
    - Presentations, Dashboards
  - Dynamic reports with knitr
    - `Markdown`, `LaTeX`








1. Unix Shell
   - Navigating Files and Directories
       - `cd`, `ls`
   - Working with Files and Directories
       - `mkdir`, `touch`, `nano`
   - Shell Scripts
       - running shell scripts and understanding the working directory
   - SSH
       - `ssh` connecting to a remote server/computer
2. R ([cheatsheets][6])
   - Project templates
       - Project structure as described [here][5]
   - Dynamic reports with knitr
       - Markdown
       - LaTeX
   - Working with data
       - Reading csv files
       - Working with databases
   - Manipulating dataframes
   - Visualizing data
   - Creating functions
   - Making choices
   - [GeoSpatial][4] analysis
       - [vector data][7]
       - [raster data][8]
       - [geospatial][9]
   - Model Fitting
   - Machine Learning Basics
3. SQL
   - What is SQL and why?
   - Selecting data
   - Insert data
   - Combining
   - Sorting and removing duplicates
   - Filtering
   - Aggregation
   - R and SQL
4. Git
   - Setting up Git
   - Creating a repository
   - Tracking changes
   - Exploring history
   - Ignoring Things
   - Remotes in Github
   - Collaboration
   - Conflicts
5. Make
   - Makefiles
   - Automatic Variables
   - Dependencies on Data and Code
   - Pattern Rules
   - Variables
   - Functions
6. Extras
   - Shiny

[1]: https://www.bi.vt.edu/sdal
[2]: https://www.bi.vt.edu/sdal/projects/data-science-for-the-public-good-program
[3]: https://software-carpentry.org/lessons/
[4]: http://www.datacarpentry.org/lessons/
[5]: https://github.com/chendaniely/computational-project-cookie-cutter
[6]: https://www.rstudio.com/resources/cheatsheets/
[7]: http://neondataskills.org/tutorial-series/vector-data-series/
[8]: http://neondataskills.org/tutorial-series/raster-data-series/
[9]: https://github.com/datacarpentry/r-spatial-data-management-intro
