
<!-- README.md is generated from README.Rmd. Please edit that file -->
<!-- README.md is generated from README.Rmd. Please edit that file -->
fitzRoy <img src="man/figures/fitz_hex.png" align="right" width="120" height="139"/>
====================================================================================

[![Build Status](https://travis-ci.org/jimmyday12/fitzRoy.svg?branch=master)](https://travis-ci.org/jimmyday12/fitzRoy) [![Coverage status](https://codecov.io/gh/jimmyday12/FitzRoy/branch/master/graph/badge.svg)](https://codecov.io/github/jimmyday12/FitzRoy?branch=master) [![packageversion](https://img.shields.io/badge/Package%20version-0.1.6-orange.svg?style=flat-square)](commits/master) [![Project Status](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active) [![Last-changedate](https://img.shields.io/badge/last%20change-2018--09--05-yellowgreen.svg)](/commits/master)

Overview
--------

The goal of fitzRoy is to provide a set of functions that allows for users to easily get access to AFL data from sources such as afltables.com and footywire.com. There are also tools for processing and cleaning that data. Future versions will include basic ELO processing functions.

Installation
------------

You can install fitzRoy from github with:

``` r
# install.packages("devtools")
devtools::install_github("jimmyday12/fitzRoy")
```

Usage
-----

The `fitzRoy` package can be used to simply get data from various sources. Some minimal working examples are below.

### Getting Data

Primarily, the tool can be used to access data from various sources. Data is included in the package and can be access directly however this will not be up to date. Each source of data has functions for updating data during the season.

### AFL Tables match results

You can access the basic afl tables match results data. This includes all matches from 1897-current. It is generally updated on the day after a round finishes.

You can access the data directly from the package using `match_results`. This will be updated periodically but you will need to update your R package to get access to the latest data. It is better to use `get_match_results` directly, as this will give you up to date results.

``` r
library(fitzRoy)
results <- get_match_results()

tail(results)
#> # A tibble: 6 x 16
#>    Game Date       Round Home.Team Home.Goals Home.Behinds Home.Points
#>   <dbl> <date>     <chr> <chr>          <int>        <int>       <int>
#> 1 15393 2018-08-25 R23   Fremantle          9           13          67
#> 2 15394 2018-08-25 R23   Carlton            8           13          61
#> 3 15395 2018-08-25 R23   Sydney            10           14          74
#> 4 15396 2018-08-26 R23   Brisbane…         11            6          72
#> 5 15397 2018-08-26 R23   Melbourne         15           12         102
#> # ... with 1 more row, and 9 more variables: Away.Team <chr>,
#> #   Away.Goals <int>, Away.Behinds <int>, Away.Points <int>, Venue <chr>,
#> #   Margin <int>, Season <dbl>, Round.Type <chr>, Round.Number <int>
```

You can also convert this format into a more analysis friendly "long" format using the helper function `convert_results`.

``` r
results_long <- convert_results(results)

head(results_long)
#> # A tibble: 6 x 13
#>    Game Date       Round Venue Margin Season Round.Type Round.Number Status
#>   <dbl> <date>     <chr> <chr>  <dbl>  <dbl> <chr>             <int> <chr> 
#> 1     1 1897-05-08 R1    Brun…     33   1897 Regular               1 Home  
#> 2     1 1897-05-08 R1    Brun…    -33   1897 Regular               1 Away  
#> 3     2 1897-05-08 R1    Vict…     25   1897 Regular               1 Home  
#> 4     2 1897-05-08 R1    Vict…    -25   1897 Regular               1 Away  
#> 5     3 1897-05-08 R1    Cori…    -23   1897 Regular               1 Home  
#> # ... with 1 more row, and 4 more variables: Behinds <chr>, Goals <chr>,
#> #   Points <chr>, Team <chr>
```

### AFL Tables player results

A new function will return all detailed player stats from afltables.com. Primarily, the easiest way to use this is simply to call `get_afltables_stats` with your required `start_date` and `end_date`.

``` r
stats <- get_afltables_stats(start_date = "2000-01-01", end_date = "2018-06-01")
#> Returning data from 2000-01-01 to 2018-06-01
#> Finished getting afltables data

tail(stats)
#> # A tibble: 6 x 59
#> # Groups:   Season, Round, Home.team, Away.team [1]
#>   Season Round Date       Local.start.time Venue Attendance Home.team  HQ1G
#>    <dbl> <chr> <date>                <int> <chr>      <int> <chr>     <int>
#> 1   2018 10    2018-05-27             1440 "Per…      37575 Fremantle     3
#> 2   2018 10    2018-05-27             1440 "Per…      37575 Fremantle     3
#> 3   2018 10    2018-05-27             1440 "Per…      37575 Fremantle     3
#> 4   2018 10    2018-05-27             1440 "Per…      37575 Fremantle     3
#> 5   2018 10    2018-05-27             1440 "Per…      37575 Fremantle     3
#> # ... with 1 more row, and 51 more variables: HQ1B <int>, HQ2G <int>,
#> #   HQ2B <int>, HQ3G <int>, HQ3B <int>, HQ4G <int>, HQ4B <int>,
#> #   Home.score <int>, Away.team <chr>, AQ1G <int>, AQ1B <int>, AQ2G <int>,
#> #   AQ2B <int>, AQ3G <int>, AQ3B <int>, AQ4G <int>, AQ4B <int>,
#> #   Away.score <int>, First.name <chr>, Surname <chr>, ID <int>,
#> #   Jumper.No. <dbl>, Playing.for <chr>, Kicks <dbl>, Marks <dbl>,
#> #   Handballs <dbl>, Goals <dbl>, Behinds <dbl>, Hit.Outs <dbl>,
#> #   Tackles <dbl>, Rebounds <dbl>, Inside.50s <dbl>, Clearances <dbl>,
#> #   Clangers <dbl>, Frees.For <dbl>, Frees.Against <dbl>,
#> #   Brownlow.Votes <dbl>, Contested.Possessions <dbl>,
#> #   Uncontested.Possessions <dbl>, Contested.Marks <dbl>,
#> #   Marks.Inside.50 <dbl>, One.Percenters <dbl>, Bounces <dbl>,
#> #   Goal.Assists <dbl>, Time.on.Ground.. <int>, Substitute <int>,
#> #   Umpire.1 <chr>, Umpire.2 <chr>, Umpire.3 <chr>, Umpire.4 <chr>,
#> #   group_id <int>
```

### Fixture

You can access the fixture using `get_fixture` function. This will download the fixture for the current calendar year by default.

``` r
fixture <- get_fixture()

head(fixture)
#> # A tibble: 6 x 7
#>   Date                Season Season.Game Round Home.Team  Away.Team Venue 
#>   <dttm>               <int>       <int> <int> <chr>      <chr>     <chr> 
#> 1 2018-03-22 19:25:00   2018           1     1 Richmond   Carlton   MCG   
#> 2 2018-03-23 19:50:00   2018           1     1 Essendon   Adelaide  Etiha…
#> 3 2018-03-24 15:35:00   2018           1     1 St Kilda   Brisbane… Etiha…
#> 4 2018-03-24 16:05:00   2018           1     1 Port Adel… Fremantle Adela…
#> 5 2018-03-24 18:25:00   2018           1     1 Gold Coast North Me… Cazal…
#> # ... with 1 more row
```

### Footywire Advanced Player Stats

Footywire data is available in the form of advanced player match statistics from 2010 games onwards. This is when advanced statistics became available.

Footywire data from 2010-2017 is included in the package. This will be updated periodically but you will need to update your R package to get access to the latest data.

``` r
## Show the top of player_stats
head(fitzRoy::player_stats)
#>         Date Season   Round Venue         Player     Team Opposition
#> 1 2010-03-25   2010 Round 1   MCG Daniel Connors Richmond    Carlton
#> 2 2010-03-25   2010 Round 1   MCG Daniel Jackson Richmond    Carlton
#> 3 2010-03-25   2010 Round 1   MCG  Brett Deledio Richmond    Carlton
#> 4 2010-03-25   2010 Round 1   MCG    Ben Cousins Richmond    Carlton
#> 5 2010-03-25   2010 Round 1   MCG  Trent Cotchin Richmond    Carlton
#> 6 2010-03-25   2010 Round 1   MCG  Dustin Martin Richmond    Carlton
#>   Status Match_id CP UP ED   DE CM GA MI5 One.Percenters BO TOG  K HB  D M
#> 1   Home     5089  8 15 16 66.7  0  0   0              1  0  69 14 10 24 3
#> 2   Home     5089 11 10 14 60.9  1  0   0              0  0  80 11 12 23 2
#> 3   Home     5089  7 14 16 76.2  0  0   0              0  0  89 12  9 21 5
#> 4   Home     5089  9 10 11 57.9  0  1   0              0  0  69 13  6 19 1
#> 5   Home     5089  8 10 13 68.4  1  0   0              0  1  77 11  8 19 6
#> 6   Home     5089  6 12 16 88.9  0  0   0              1  0  81  5 13 18 4
#>   G B T HO GA1 I50 CL CG R50 FF FA AF SC CCL SCL SI MG TO ITC T5
#> 1 0 0 1  0   0   2  2  4   6  2  0 77 85  NA  NA NA NA NA  NA NA
#> 2 0 0 5  0   0   8  5  4   1  2  0 85 89  NA  NA NA NA NA  NA NA
#> 3 1 0 6  0   0   4  3  4   3  1  2 94 93  NA  NA NA NA NA  NA NA
#> 4 1 0 1  0   1   1  2  3   4  1  0 65 70  NA  NA NA NA NA  NA NA
#> 5 0 0 1  0   0   2  3  3   2  0  2 65 63  NA  NA NA NA NA  NA NA
#> 6 0 0 3  0   0   2  3  1   0  0  1 62 72  NA  NA NA NA NA  NA NA
```

We can also use the `update_footywire_stats` function to get the most up to date data. This will merge data from 2010-current with any new data points.

``` r
## Update footywire data
dat <- update_footywire_stats()
#> Getting match ID's...
#> Downloading new data for 36 matches...
#> 
#> Checking Github
#> Finished getting data

tail(dat)
#>             Date Season    Round Venue          Player Team Opposition
#> 80119 2018-08-26   2018 Round 23   MCG       Rory Lobb  GWS  Melbourne
#> 80120 2018-08-26   2018 Round 23   MCG     Samuel Reid  GWS  Melbourne
#> 80121 2018-08-26   2018 Round 23   MCG  Lachlan Keeffe  GWS  Melbourne
#> 80122 2018-08-26   2018 Round 23   MCG Matthew Buntine  GWS  Melbourne
#> 80123 2018-08-26   2018 Round 23   MCG     Zac Langdon  GWS  Melbourne
#> 80124 2018-08-26   2018 Round 23   MCG    Daniel Lloyd  GWS  Melbourne
#>       Status Match_id CP UP ED   DE CM GA MI5 One.Percenters BO TOG K HB
#> 80119   Away     9709  2  7  7 70.0  0  0   1              9  0  67 7  3
#> 80120   Away     9709  4  5  6 66.7  2  1   2              0  0  75 4  5
#> 80121   Away     9709  1  8  6 75.0  0  0   0              6  0  88 2  6
#> 80122   Away     9709  4  5  6 75.0  1  0   0              4  0  79 4  4
#> 80123   Away     9709  4  4  6 75.0  0  0   1              0  0  76 3  5
#> 80124   Away     9709  0  9  6 85.7  0  0   0              1  0  80 5  2
#>        D M G B T HO GA1 I50 CL CG R50 FF FA AF SC CCL SCL SI  MG TO ITC T5
#> 80119 10 4 1 0 2 12   0   3  1  0   1  1  0 66 91   0   1  4 225  1   0  0
#> 80120  9 6 0 1 5  0   1   2  0  0   0  0  0 61 54   0   0  4 123  2   1  2
#> 80121  8 3 0 0 0  1   0   0  0  1   3  0  0 28 35   0   0  0  82  2   2  0
#> 80122  8 2 0 0 1  0   0   1  0  1   2  1  0 31 41   0   0  0 174  2   3  0
#> 80123  8 2 1 0 2  0   0   2  0  3   0  1  3 31 25   0   0  1 110  4   1  0
#> 80124  7 5 0 0 1  0   0   1  0  1   0  0  1 35 30   0   0  2 164  1   0  0
```

### Weather

We have also included weather data for the 2017 season. This is a work in progress but includes rainfall data from the nearest observation station to each ground. This data is included in the package as `results_weather`.

``` r
library(ggplot2)
library(dplyr)

# Get 2017 weather data
weather <- fitzRoy::results_weather %>%
  filter(Season == 2017)

# Plot total rainfal for each home team
ggplot(dat = weather, aes(x = Home.Team, y = Rainfall)) +
  geom_col() + 
  coord_flip()
```

![](README-weather-1.png)

### Squiggle Data

You can access data from the [Squiggle API](api.squiggle.com.au) where the tips of well known AFL tipping models are collected. See full instructions on the above link.

``` r
# You can get the sources
sources <- get_squiggle_data("sources")
head(sources)
#>   id                  name                                url
#> 1  1              Squiggle      https://live.squiggle.com.au/
#> 2  2               The Arc           https://thearcfooty.com/
#> 3  3        Figuring Footy          http://figuringfooty.com/
#> 4  4       Matter of Stats      http://www.matterofstats.com/
#> 5  5               Punters                                   
#> 6  6 Footy Maths Institute https://footymaths.blogspot.com.au
```

``` r
# Get all tips
tips <- get_squiggle_data("tips")
head(tips)  
#>   round sourceid margin          source ateamid       hteam
#> 1     1        1   1.00        Squiggle      14     Carlton
#> 2     1        3     NA  Figuring Footy      14     Carlton
#> 3     1        4   5.39 Matter of Stats      14     Carlton
#> 4     1        4  10.31 Matter of Stats      18 Collingwood
#> 5     1        1  17.00        Squiggle      18 Collingwood
#> 6     1        1   3.00        Squiggle       9    Adelaide
#>                tip hteamid    bits                  ateam correct
#> 1         Richmond       3  0.0000               Richmond       1
#> 2         Richmond       3  0.2141               Richmond       1
#> 3          Carlton       3 -0.2076               Richmond       0
#> 4 Western Bulldogs       4  0.3265       Western Bulldogs       1
#> 5 Western Bulldogs       4  0.3103       Western Bulldogs       1
#> 6         Adelaide       1  0.0000 Greater Western Sydney       1
#>                  date gameid year         venue tipteamid hconfidence
#> 1 2017-03-23 19:20:00      1 2017        M.C.G.        14        50.0
#> 2 2017-03-23 19:20:00      1 2017        M.C.G.        14        42.0
#> 3 2017-03-23 19:20:00      1 2017        M.C.G.         3        56.7
#> 4 2017-03-24 19:50:00      2 2017        M.C.G.        18        37.3
#> 5 2017-03-24 19:50:00      2 2017        M.C.G.        18        38.0
#> 6 2017-03-26 15:20:00      8 2017 Adelaide Oval         1        50.0
#>               updated   err confidence
#> 1 2017-07-11 13:59:46 42.00       50.0
#> 2 2017-04-10 12:18:02    NA       58.0
#> 3 2017-07-11 13:59:46 48.39       56.7
#> 4 2017-07-11 13:59:46  3.69       62.7
#> 5 2017-07-11 13:59:46  3.00       62.0
#> 6 2017-07-11 13:59:46 53.00       50.0
```

``` r
# Get` just tips from round 1, 2018
tips <- get_squiggle_data("tips", round = 1, year = 2018)
head(tips)
#>   round ateamid sourceid   err year tipteamid             updated      tip
#> 1     1       1        1 23.00 2018         1 2018-03-23 22:54:38 Adelaide
#> 2     1       1        2 21.00 2018         1 2018-03-23 22:54:38 Adelaide
#> 3     1       1        4 21.78 2018         1 2018-03-23 22:54:38 Adelaide
#> 4     1       1        5    NA 2018         5 2018-03-23 22:54:38 Essendon
#> 5     1       1        6 33.00 2018         1 2018-03-23 22:54:38 Adelaide
#> 6     1       1        7 20.00 2018         1 2018-03-23 22:54:38 Adelaide
#>                  date     venue hconfidence correct confidence
#> 1 2018-03-23 19:50:00 Docklands       44.00       0      56.00
#> 2 2018-03-23 19:50:00 Docklands       40.20       0      59.80
#> 3 2018-03-23 19:50:00 Docklands       40.50       0      59.50
#> 4 2018-03-23 19:50:00 Docklands       52.08       1      52.08
#> 5 2018-03-23 19:50:00 Docklands       34.00       0      66.00
#> 6 2018-03-23 19:50:00 Docklands       44.84       0      55.16
#>                  source    hteam    bits    ateam margin gameid hteamid
#> 1              Squiggle Essendon -0.1844 Adelaide  11.00    373       5
#> 2               The Arc Essendon -0.3147 Adelaide   9.00    373       5
#> 3       Matter of Stats Essendon -0.3040 Adelaide   9.78    373       5
#> 4               Punters Essendon  0.0588 Adelaide     NA    373       5
#> 5 Footy Maths Institute Essendon -0.5564 Adelaide  21.00    373       5
#> 6            PlusSixOne Essendon -0.1571 Adelaide   8.00    373       5
```

------------------------------------------------------------------------

Docker Support
--------------

fitzRoy now provides [Docker](https://www.docker.com/get-started) support in the form of an image hosted on [DockerHub](https://hub.docker.com/r/jimmyday12/fitzroy/).

### Usage

Once you have the images, (e.g. `docker pull jimmyday12/fitzroy:latest`) run one of the following commands.

-   To start [RStudio](https://www.rstudio.com/) with Fitzroy ready to use:
-   `docker run -d -p 8787:8787 --name fitzroy jimmyday12/fitzroy:latest` and open <http://localhost:8787>. \*(Username: `rstudio`, Password: `rstudio`)
-   To start an R terminal prompt with fitzRoy ready to use:
-   `docker run -it jimmyday12/fitzroy:latest R` to start with an R terminal prompt.
-   Run `quit()` to exit the container

### Building the image locally

To build the Docker image run the following from the root of the repository. \* `docker build -t jimmyday12/fitzroy:latest -f docker/rstudio/Dockerfile .`

------------------------------------------------------------------------

Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.
