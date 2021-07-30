# report-generator

Generates a dashboard and arbitrary number of html and xlsx reports from one or multiple data sources.

## Summary

This project is the framework for one way that Rainier Scholars generates and reviews Key Performance Indicators (KPIs). This method, developed over several iterations, includes several features we have found critical to the process of generating accurate, agreed upon metrics:

**Stakeholder Involvement**
A key to strong metrics is making sure stakeholders are involved in the process. We like to think that the point of metrics is insights, not just numbers. As our staff are the ones making the mission happen and collecting our data, they will get the most out of whatever insights result from these reports.

**Clear Definitions**
Calculating KPIs with code allows metric definitions to be embedded within both code and the reports. This helps make sure everyone is on the same page when assessing the validity of these metrics.

**Data Visibility**
This approach involves creating interactive, HTML reports that allows stakeholders to see each metric, its definition, and the data used in each calculation. We've found that having stakeholders look at the underlying data for multiple metrics all in one place streamlines the process of resolving inconsistencies and errors.

**Repeatable Calculations**
Calculating KPIs with code helps ensure consistency from year to year and metric to metric allowing metric definitions to be embedded within both code and the output. Though many metrics seem straightforward at first, at an organization where individual students are priorities, being off by even one can mean a big difference in our outcomes.

These four features have led to a more reliable, repeatable, and understandable process for everyone involved, and will hopefully continue to iterate in the future.

This repository is meant to help other nonprofits standardize and formalize their data reporting which might otherwise be spread across multiple departments, spreadsheets, and tools.

## A Note About The Data

The metrics and data in this repository have been invented and/or randomly generated for the purposes of demonstrating the process and do not necessarily reflect our actual data or KPI's.

## Installation

This repository is intended to be an example of one kind of report generation with instructions on how to but was not built with extension or plug-and-play as the main goal.

The instructions below were written with folks less familiar with Python in mind, and will cover some good general practices (e.g. command line tools, environment and package managers like `conda`), but not others (e.g. `git`).

More experienced users should clone or fork the repository.

To fully recreate this process, you will need the following software. Installation instructions can be found below.

- (Recommended) A python environment manager. This project uses Anaconda (https://www.anaconda.com/products/individual)[https://www.anaconda.com/products/individual]
- Python version 3.7.7 or greater
- The following major python packages (which will include a bunch of others)
  - jupyterlab
  - jinja2
  - pandas

### Install Anaconda

Anaconda is an open source tool used for managing Python programming environments and managing Python packages. It allows you to create separate virtual environments (including versions of Python and packages/tools installed) for your separate projects. Anaconda is often used and suggested for data science projects, but I am recommending it because it has a user interface called Anaconda Navigator as an alternative to using the command line for everything. Though this may be your first project, it is a good idea to install an environment manager like Anaconda even before starting your first Python project.

You can learn more about environment and package managers by searching the internet for phrases like "Manage Python environments and packages" and "Python Environment, Package, and Dependency Management" Just for reference, another commonly used environment manager is called `venv` and another common python package manager is called `pip`.

The best instructions for installing Anaconda are probably those on the Official Anaconda Documentation. At the time of writing, installation instructions could be found at (https://docs.anaconda.com/anaconda/install/)[https://docs.anaconda.com/anaconda/install/]

To confirm Anaconda is installed correctly, open a command prompt and enter

```
$ conda --version
```

This should return something along the lines of

```
conda 4.8.3
```

### Create a New Python Environment

With Anaconda installed, you can create a directory (a.k.a. a folder) and an Python environment for your project.

To create a folder using the command line, open a new command prompt.

Use the commands `ls` to list directories and files and the commend `cd` to change directories. For example, lets say your directories look something like this:

```
.
├──Applications
├──Desktop
├──Downloads
├──Documents
├── Documents
│   ├── Books
│   ├── Notes
│   └── Projects
│       ├── Database-Development
│       ├── QR-Code-Generation
│       └── Tutorial-Writing
└── Pictures
```

Though it isn't strictly necessary, it can be a good idea to make your project directory first so you can create an environment with the same name so you never really need to remember the name of the environment.

To make a new directory in Projects, you might enter the following commands:

```
$ cd Documents
$ ls
Books
Notes
Projects
$ cd Projects
$ ls
Database Development
QR Code Generation
Tutorial Writing
$ mkdir Report-Generator
$ ls
Database-Development
QR-Code-Generation
Report-Generator
Tutorial-Writing
```

You should now have a directory called "Report-Generator" in your Projects folder. Note that this tutorial uses dashes or underscores between words when making fils and directories. If you use spaces, you will need to use quotes around the names. For example, to make a directory called `Report Generator` you would need to use a command like `mkdir "Report Generator"` with quotes.

```
.
├──Applications
├──Desktop
├──Downloads
├──Documents
├── Documents
│   ├── Books
│   ├── Notes
│   └── Projects
│       ├── Database-Development
│       ├── QR-Code-Generation
│       ├── Report-Generator
│       └── Tutorial-Writing
└── Pictures
```

Enter the new directory:

```
$ cd Report-Generator
```

Now that you have a directory for your project, you know what you should name your Python environment. The command below will use Anaconda to create a new Python environment called Report-Generator that will have Python 3.7.7.

```
conda create -n Report-Generator python=3.7.7
```

With most environment managers, creating the environment is one step, and actually using it is another. To start using (to activate) the environment, enter:

```
conda activate Report-Generator
```

More information on using Anaconda can be found in Anaconda's documentation. At the time of writing, the page on managing environments could be found at [https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)

### Install the Required Packages

After activating your environment for this project, you are ready to start installing the packages this project uses.

You can install the packages one by one by repeatedly calling `conda install` followed by the name of each package, or you can install them all at the same time like this:

```
conda install jupyterlab jinja2 pandas
```

Follow the prompts to install all of the required packages.

### Download These Sample Files From Github

If you are familiar with github, you've likely already cloned this repo (see [https://guides.github.com/](https://guides.github.com/) to learn more). If you are not, you should find the "Code" button toward the top of the main page of this repository and choose "Download ZIP". Then extract the ZIP into your project folder.

## Structure

The report generator has three major parts and a config file:

- source-data
- code
- outputs
- config.py

source-data and code have one subdirectory for every year of data in the output. outputs has subdirectories for html dashboard summaries, single file reports, and the templates used to make the the summaries and reports.

### source-data

This directory is where all of the input files should be stored. We've found the most success so far separating the files by year.

```
.
├── source-data
    ├── 2017-2018
    │    ├── permenant-records
    │    ├── enrollment-records
    ⋮    ⋮
    │    └── internship-log
    ├── 2019-2020
    │    ├── permenant-records
    │    ├── enrollment-records
    ⋮    ⋮
    │    └── internship-log
    ├── 2018-2019
    │    ├── permenant-records
    │    ├── enrollment-records
    ⋮    ⋮
    │    └── internship-log
    └── 2020-2021
    │    ├── permenant-records
    │    ├── enrollment-records
    ⋮    ⋮
    │    └── internship-log
```

### code

This directory is for the code that will calculate each of the metrics. We've found the most success so far separating each metric into its own file, and grouping the files by year.

```
.
├── code
    ├── 2017-2018
    │    ├── 01_first-metric
    │    ├── 02_second-metric
    ⋮    ⋮
    │    └── 10_final-metric
    ├── 2018-2019
    │    ├── 01_first-metric
    │    ├── 02_second-metric
    ⋮    ⋮
    │    └── 10_final-metric
    ├── 2019-2020
    │    ├── 01_first-metric
    │    ├── 02_second-metric
    ⋮    ⋮
    │    └── 10_final-metric
    └── 2020-2021
         ├── 01_first-metric
         ├── 02_second-metric
         ⋮
         └── 10_final-metric
```

### outputs

This directory is for the generated reports and has two major components: dashboard summaries and reports.

The dashboard summaries are html pages that allow readers to see a preview of the final report including previous years' numbers and trends, as well as drilling into the details of any particular number including the raw data and how it was calculated. These detail pages offer sortable, searchable tables.

The reports are Excel spreadsheets that conform with how data was collected and presented in the past. Each spreadsheet shows only the metric summaries. Note that because the generation is automatic and the goal is fitting all necessary information on a single page, manual polishing of the final reports is often needed before they are presentation-ready.

```
.
├── output
    ├── dashboard-summaries
    │    ├── 2017-2018
    │    │    ├── individual-metrics
    │    │    │    ├── 01_first-metric.html
    │    │    │    ├── 02_second-metric.html
    ⋮    ⋮     ⋮    ⋮
    │    │    │    └── 10_final-metric.html
    │    │    └── 2017-2018_summary.html
    │    │
    │    ├── 2018-2019
    │    │    ├── individual-metrics
    │    │    │    ├── 01_first-metric.html
    │    │    │    ├── 02_second-metric.html
    ⋮    ⋮     ⋮    ⋮
    │    │    │    └── 10_final-metric.html
    │    │    └── 2017-2018_summary.html
    │    │
    │    ├── 2019-2020
    │    │    ├── individual-metrics
    │    │    │    ├── 01_first-metric.html
    │    │    │    ├── 02_second-metric.html
    ⋮    ⋮     ⋮    ⋮
    │    │    │    └── 10_final-metric.html
    │    │    └── 2017-2018_summary.html
    │    │
    │    └── 2020-2021
    │         ├── individual-metrics
    │         │    ├── 01_first-metric.html
    │         │    ├── 02_second-metric.html
    ⋮          ⋮    ⋮
    │         │    └── 10_final-metric.html
    │         └── 2017-2018_summary.html
    │
    └── reports
         ├── 2017-2018_KPIs.xlsx
         ├── 2018-2019_KPIs.xlsx
         ├── 2019-2020_KPIs.xlsx
         └── 2020-2021_KPIs.xlsx
```

### main.py

This file serves as the one file that needs to be run and allows you to choose which years should be fully recalculated.

## Common Issues

Here are a few common problems have come up over the years:

- How we calculate a metric changes; we make a note that it changed or have empty columns
- Raw data for some years are missing; we use metric outputs and notes
- Many of these metrics have outliers and exceptions, how one handles them is an internal decision.

## TODOS

- Choose mock metrics
- Add mock data
  - Choose mock sources
  - Determine columns for each
  - Generate reasonable fake data
  - Use 2017-2018 as the year with missing data
- Add code
  - Use existing code to model the mock code
  - Copy into each year
  - Use 2017-2018 as the year with missing data (so no code)
- Document handling when metrics change and other issues
