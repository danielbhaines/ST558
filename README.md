R Project 1
================

  - [Required Packages](#required-packages)
  - [Contact the NHL records API and the NHL stats
    API](#contact-the-nhl-records-api-and-the-nhl-stats-api)
      - [nhl\_records function:](#nhl_records-function)
      - [nhl\_stats function:](#nhl_stats-function)
      - [nhl\_data function:](#nhl_data-function)
  - [Some exploratory data analysis](#some-exploratory-data-analysis)
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

# Some exploratory data analysis

## Do a join on two returned datasets from different API endpoints

``` r
# Import the first data set using 'nhl_records' via the wrapper function
dat_1 <- nhl_data('records', endpoint='Franchise', team_name='Boston Bruins')

# Import the second data set using 'nhl_stats' via the wrapper function 
dat_2 <- nhl_data('stats', modifier='team_stats', team_id=6)

# Rename the full team name variable 'name' in 'dat_2' to prepare to merge the data sets
dat_2 <- dplyr::rename(dat_2, teamCommonName=name)

# Merge the two datasets
dat_3 <- merge(dat_1, dat_2, by='teamCommonName')

# Partial output
dat_3[1:5]
```

    ##   teamCommonName firstSeasonId lastSeasonId mostRecentTeamId            link
    ## 1  Boston Bruins      19241925           NA                6 /api/v1/teams/6

## Create at least two new variables

  - New variable: **Average games started**  
    This variable will look at the average amount of games started by
    Carolina Hurricane goaltenders.

<!-- end list -->

``` r
# Create a new variable, average_games_started
car_goal <- nhl_data('records', endpoint='GoalieRecords', team_name='Carolina Hurricanes')
car_avg_game <- mean(car_goal$gamesPlayed)
average_games_started <- rep(car_avg_game, times=nrow(car_goal))

# Add a new column corresponding to the average number of games a goaltender starts as a Hurricane
car_goal <- cbind(car_goal, average_games_started)

# Display a partial output
head(car_goal[,c(2,7,4,6,29)])
```

    ##   firstName    lastName       franchiseName gamesPlayed average_games_started
    ## 1       Cam        Ward Carolina Hurricanes         668                  81.5
    ## 2    Arturs        Irbe Carolina Hurricanes         309                  81.5
    ## 3       Tom    Barrasso Carolina Hurricanes          34                  81.5
    ## 4   Richard     Brodeur Carolina Hurricanes           6                  81.5
    ## 5      Sean       Burke Carolina Hurricanes         256                  81.5
    ## 6      Mark Fitzpatrick Carolina Hurricanes           3                  81.5

  - New variable: **Hat trick**  
    A hat trick is three or more goals scored by a player in a single
    game. This variable will determine if a player has scored a hat
    trick in their career.

<!-- end list -->

``` r
# Create a new variable, hat_trick
bos_skate <- nhl_data('records', endpoint='SkaterRecords', team_name='Boston Bruins')
hat_trick <- ifelse(bos_skate$mostGoalsOneGame > 2, TRUE, FALSE)

# Add a column corresponding to the new variable hat_trick
bos_skate <- cbind(bos_skate, hat_trick)

# A partial output including our new variable
head(bos_skate[,c(3,9,5,8,15,30)])
```

    ##   firstName lastName franchiseName goals mostGoalsOneGame hat_trick
    ## 1    Johnny    Bucyk Boston Bruins   545                4      TRUE
    ## 2       Ray  Bourque Boston Bruins   395                3      TRUE
    ## 3     Terry O'Reilly Boston Bruins   204                3      TRUE
    ## 4      Phil Esposito Boston Bruins   459                4      TRUE
    ## 5     Bobby      Orr Boston Bruins   264                3      TRUE
    ## 6       Jay   Miller Boston Bruins    13                2     FALSE

## Create some contingency tables

``` r
# Import the entire 'SkaterRecords' data frame for the original six teams
original_six <- c('Chicago Blackhawks', 'Boston Bruins', 'Montréal Canadiens', 'Detroit Red Wings', 
                  'New York Rangers', 'Toronto Maple Leafs')
origsix_skate <- nhl_data('records', endpoint='SkaterRecords', team_name=original_six)
```

    ## Warning in franchiseName == team_name: longer object length is not a multiple of
    ## shorter object length

``` r
# Create a contingency table of original six team players by position
table(origsix_skate$franchiseName, origsix_skate$positionCode) 
```

    ##                      
    ##                        C  D  L  R
    ##   Boston Bruins       36 35 34 27
    ##   Chicago Blackhawks  40 53 28 28
    ##   Detroit Red Wings   39 49 35 25
    ##   Montréal Canadiens  34 37 38 26
    ##   New York Rangers    37 50 39 45
    ##   Toronto Maple Leafs 40 33 30 40

*The above table is a contingency table with the counts of players from
each team in the data set by their position.*

    ##                      
    ##                       FALSE TRUE
    ##   Boston Bruins         122   10
    ##   Chicago Blackhawks    139   10
    ##   Detroit Red Wings     139    9
    ##   Montréal Canadiens    128    7
    ##   New York Rangers      163    8
    ##   Toronto Maple Leafs   132   11

*The above table is a contingency table with counts of players from
original six teams that are either active or retired.*

## Create numerical summaries for some quantitative variables at each setting of some of your categorical variables

## Create at least five plots utilizing coloring, grouping, etc. All plots should have nice labels, titles, etc. You should have at least one bar plot, one histogram, one box plot, and one scatterplot.

NOTE THAT CODE SHOULD BE SHOWN IN THE FINAL DOCUMENT UNLESS IT IS BEHIND
THE SCENES OR UNIMPORTANT FOR THE OUTPUT WE ARE SEEING
