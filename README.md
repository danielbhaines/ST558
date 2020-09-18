R Project 1
================

  - [Required Packages](#required-packages)
  - [Contact the NHL records API and the NHL stats
    API](#contact-the-nhl-records-api-and-the-nhl-stats-api)
      - [nhl\_records function:](#nhl_records-function)
      - [nhl\_stats function:](#nhl_stats-function)
      - [nhl\_data function:](#nhl_data-function)
  - [Basic exploratory data analysis (be sure to discuss what each graph
    is
    showing)](#basic-exploratory-data-analysis-be-sure-to-discuss-what-each-graph-is-showing)
      - [Do a join on two returned datasets from different API
        endpoints](#do-a-join-on-two-returned-datasets-from-different-api-endpoints)
      - [Create at least two new
        variables](#create-at-least-two-new-variables)
      - [Create some contingency
        tables](#create-some-contingency-tables)
      - [Create numerical summaries for some quantitative variables at
        each setting of some of your categorical
        variables](#create-numerical-summaries-for-some-quantitative-variables-at-each-setting-of-some-of-your-categorical-variables)
      - [Create at least five plots utilizing coloring, grouping, etc.
        All plots should have nice labels, titles, etc. You should have
        at least one bar plot, one histogram, one box plot, and one
        scatterplot.](#create-at-least-five-plots-utilizing-coloring-grouping-etc.-all-plots-should-have-nice-labels-titles-etc.-you-should-have-at-least-one-bar-plot-one-histogram-one-box-plot-and-one-scatterplot.)

# Required Packages

``` r
library(httr)
library(jsonlite)
library(dplyr)
```

# Contact the NHL records API and the NHL stats API

## nhl\_records function:

``` r
nhl_records(endpoint, team_id=NULL, team_name=NULL)
```

The `nhl_records` function pulls data from `records.nhl.com` and outputs
it to a data frame. It takes on multiple arguments:

  - `endpoint`: can take on five values (`Franchise, TeamTotals,
    Franchise_SeasonTotals, GoalieRecords, SkaterRecords`). This option
    will pull data from different API tables. The value for this
    variable must be passed as a string.  
  - `team_id`: is a unique numeric identifier for each NHL team. By
    passing this, the value returned will only contain data for that
    specific NHL team. By default, this value is set to `NULL`.  
  - `team_name`: is a unique character assignment for each NHL team.
    Team names take on the format ‘<City> <Club Name>’. By passing this,
    the value returned will only contain data for that specific NHL
    team. By default, this value is set to `NULL`.

## nhl\_stats function:

``` r
nhl_stats(modifier, modifier_extension=NULL, team_id=NULL)
```

The `nhl_stats` function pulls data from `www.statsapi.web.nhl.com` and
outputs it to a data frame. It takes on multiple arguments:

  - `modifier`: can take on one of eight values (`team_roster,
    person_names, team_schedule_next, team_schedule_previous,
    team_stats, team_roster_season, team_id, single_season_playoff`).
    This value will run an expand command on the API. The value for this
    variable must be passed as a string.  
  - `modifier_extension`: Allows the user to pass additional search
    options to the API. An example of this would be `&gameType=P` to
    pull data from playoff games only. By default, this value is set to
    `NULL`. The value for this variable must be passed as a string.  
  - `team_id`: is a unique numeric identifier for each NHL team. By
    passing this, the value returned will only contain data for that
    specific NHL team. By default, this value is set to `NULL`.

## nhl\_data function:

``` r
nhl_data(source, ...)
```

The `nhl_data` function serves as a wrapper function for `nhl_records`
and `nhl_stats`.

  - `source`: calls whichever function is needed (`records` or `stats`).
    This argument must be passed a string.  
  - `...`: allows the user to pass all arguments supported by the
    functions `nhl_records` and `nhl_stats`.

# Basic exploratory data analysis (be sure to discuss what each graph is showing)

## Do a join on two returned datasets from different API endpoints

## Create at least two new variables

## Create some contingency tables

## Create numerical summaries for some quantitative variables at each setting of some of your categorical variables

## Create at least five plots utilizing coloring, grouping, etc. All plots should have nice labels, titles, etc. You should have at least one bar plot, one histogram, one box plot, and one scatterplot.

NOTE THAT CODE SHOULD BE SHOWN IN THE FINAL DOCUMENT UNLESS IT IS BEHIND
THE SCENES OR UNIMPORTANT FOR THE OUTPUT WE ARE SEEING
