## Introduction

This is a command line tool that will
fetch various metrics pertaining to a  Jira project's
current sprint or for the past N sprints. These metrics will be appended into a 
csv file that can be exported into 
any other external tools for further analysis 
or visualizations.

Additionally, one can also choose to run another script here as well to
generate a visualization of the report that gets created.

## Configuration

### Python Dependencies

Python 3.7.1+ is required for running these scripts.

[Poetry](https://python-poetry.org/) is the package manager utilized for this 
project. To install poetry the following can be ran:
```bash
make install
```
This will both install poetry if its not installed already and then use poetry
to install all of the dependent packages. 

**Recommended** to set up a virtual environment and activate that first before 
running poetry to install the dependent packages for these scripts.

### Environment Variables

Copy over the `TEMPLATE.env` file and create
a `.env` file with the templated values filled out.

* JIRA_TOKEN - API token generated by JIRA
* JIRA_EMAIL - Email used to generate the 
  API token
* JIRA_SERVER - Host url for the JIRA server

The following `make` command can be utlized to create a `.env` file based
upon the `TEMPLATE.env` file:
```bash
make env
```

### Command Line Args and Config File

The following are the command line args that need to be supplied to the scraper:
```bash
optional arguments:
  -h, --help            show this help message and exit
  --config-file CONFIG_FILENAME
                        Optional config file containing the configuration by
                        which to generate a report with, if provided then the
                        other args not required.
  --planned-capacities [PLANNED_CAPACITIES [PLANNED_CAPACITIES ...]]
                        The team's planned capacities for previous sprints. In
                        order of most current to the oldest sprint's planned
                        capacities.
  --board-id BOARD_ID   JIRA Board ID
  --project-name PROJECT_NAME
                        JIRA Project Name
  --story-points-field STORY_POINTS_FIELD
                        JIRA Story Points API field name
  --complete-status COMPLETE_STATUS
                        JIRA Issue status to indicate an Issue is done
  --priority-epics [PRIORITY_EPICS [PRIORITY_EPICS ...]]
                        Optional list of keys of the priority epics
  --past-n-sprints PAST_N_SPRINTS
                        Optionally grab metrics for the past N sprints, if
                        left out then only the currently active sprint's
                        metrics will be fetched
```

A config file can be supplied in the form of the `--config-file` arg to the
jira scraper instead of passing a list of args everytime one wants to run the script. 
A templated config yaml file can be found in `configs/TEMPLATE.yaml`. Running
the following make command can be used to create a copy of the `TEMPLATE.yaml`
```bash
make config PROJECT_NAME=<PROJECT_NAME>
```
Then one just needs to fill in the config values.


## To Run

To generate the report one needs to run `jira_scraper.py` script:
```bash
➜ python jira_scraper.py --help
usage: jira_scraper.py [-h] [--config-file CONFIG_FILENAME]
                       [--planned-capacities [PLANNED_CAPACITIES [PLANNED_CAPACITIES ...]]]
                       [--board-id BOARD_ID] [--project-name PROJECT_NAME]
                       [--story-points-field STORY_POINTS_FIELD]
                       [--complete-status COMPLETE_STATUS]
                       [--priority-epics [PRIORITY_EPICS [PRIORITY_EPICS ...]]]
                       [--past-n-sprints PAST_N_SPRINTS]

optional arguments:
  -h, --help            show this help message and exit
  --config-file CONFIG_FILENAME
                        Optional config file containing the configuration by
                        which to generate a report with, if provided then the
                        other args not required.
  --planned-capacities [PLANNED_CAPACITIES [PLANNED_CAPACITIES ...]]
                        The team's planned capacities for previous sprints. In
                        order of most current to the oldest sprint's planned
                        capacities.
  --board-id BOARD_ID   JIRA Board ID
  --project-name PROJECT_NAME
                        JIRA Project Name
  --story-points-field STORY_POINTS_FIELD
                        JIRA Story Points API field name
  --complete-status COMPLETE_STATUS
                        JIRA Issue status to indicate an Issue is done
  --priority-epics [PRIORITY_EPICS [PRIORITY_EPICS ...]]
                        Optional list of keys of the priority epics
  --past-n-sprints PAST_N_SPRINTS
                        Optionally grab metrics for the past N sprints, if
                        left out then only the currently active sprint's
                        metrics will be fetched
```

To generate data visualizations based upon the report created by the scraper then
one needs to run the `visualization.py` script:
```bash
➜ python visualization.py --help
usage: visualization.py [-h] [--config-file CONFIG_FILENAME]
                        [--project-name PROJECT_NAME]

optional arguments:
  -h, --help            show this help message and exit
  --config-file CONFIG_FILENAME
                        Optional config file that can be supplied and then the
                        otherargs are not required
  --project-name PROJECT_NAME
                        JIRA Project Name
```

### Makefile Commands

* `make all` - Clean out the `.pyc` files and install dependencies
* `make install` - Install `poetry` and the dependent packages for this repo
* `make env` - Create a `.env` file based on the `TEMPLATE.env` file
* `make config PROJECT_NAME=<project_name>` - Create a new config file based upon the
  `TEMPLATE.yaml` file and name the new config file after the given project name
* `make clean` - Clean out all `.pyc` files
* `make format` - Uses the [black](https://github.com/psf/black) formatter to format
* `make lint` - Runs black and flake8 on the project
all the python files in this project, these are the same checks ran on pull requests
  with Github actions
  
* `make dependency-tree` - Outputs a all the dependencies for this project

### Credits

Credits to all the packages outputted by `make dependency-tree`.
