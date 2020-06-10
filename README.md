cmdcR
================

Welcome to the R client library for accessing the COVID Modeling Data
Collaborative (CMDC) database.

Links:

  - [Repository](https://github.com/valorumdata/cmdcR)
  - [Website](https://covid.valorum.ai/)
  - [Python](https://github.com/valorumdata/cmdc.py) and
    [Julia](https://github.com/valorumdata/CMDC.jl) clients
  - [Raw REST API](https://covid.valorum.ai/rest-api)
  - [GraphQL API](https://covid.valorum.ai/graphql-api)

As of right now, this library is a wrapper around the
[cmdc.py](https://github.com/valorumdata/cmdc.py) Python client. For
more examples and documentation, please see that library. If you are an
R programmer and are willing to contribute to making a native library,
please reach out at our
[repository](https://github.com/valorumdata/cmdcR)\!

Also, please see the [project website](https://covid.valorum.ai) for
more information.

## COVID Modeling Data Collaborative

The COVID Modeling Data Collaborative (CMDC) is a project funded by
[Schmidt Futures](https://schmidtfutures.com/) and seeks to simplify the
data ingestion process for researchers and policy makers who are working
to enact and understand COVID-19 related policies. We accomplish this
goal in several ways:

  - Collect unique, hard-to-acquire, datasets that are not widely
    distributed
  - Aggregate data collected by other related organizations into a
    centralized database
  - Work with other related organizations to expand and improve their
    data collection processes
  - Build tools, such as this library (and
    [Python](https://github.com/valorumdata/cmdc.py) and
    [Julia](https://github.com/valorumdata/CMDC.jl) equivalents), to
    simplify the data ingestion process

More information about our project and what data is collected can be
found on our [website](https://covid.valorum.ai/).

We are always looking to hear from both those who would like to help us
build CMDC and those who would like use CMDC. [Please reach out to
us](https://covid.valorum.ai/contact)\!

## Installation

Please install this package using `devtools::install_github` as follows

``` r
devtools::install_github("valorumdata/cmdcR")
```

After installing the package, you need to make sure that the underlying
python package is installed.

To do this, use

``` r
cmdcR::install_cmdcPY()
```

During the installation process, R will check if you have an existing
Python installation that can be used

You may be prompted to accept the installation of a dedicated Python
(via miniconda) for R to use

We recommend that you accept this request, but if you are comfortable
managing your own Python installation you can say no

## Creating a Client

Once the package is installed, the first step is to create an API
client:

``` r
library(cmdcR)
```

``` r
cl <- client()
```

## Datasets

You can see a list of currently available datasets using:

``` r
datasets(cl)
#> [1] "counties"           "covid"              "covid_historical"   "demographics"       "economics"          "mobility_devices"   "mobility_locations" "states"             "swagger.json"
```

Each dataset has an associated function

You can get detailed information on a specific dataset using the `info`
method. For example

``` r
info(cl)
#> CMDC Client
#> [1] "Datasets are:"
#> 
#> - counties
#> - covid
#> - covid_historical
#> - demographics
#> - economics
#> - mobility_devices
#> - mobility_locations
#> - states
#> - swagger.json
#> CMDC Client

info(cl, "demographics")
#> Request builder for demographics endpoint
#> Valid filters are meta_date, fips, variable, value, select, order, Range, Range-Unit, offset, limit, Prefer
#> 
#> 
#> Currently, the following variables are collected in the database
#> 
#> * Total population
#> * Median age
#> * Fraction of the population over 65
#> * Fraction of the population who identify as various races or as Hispanic/Latino
#> * Fraction of the population with various degrees of education
#> * Fraction of the population that commutes in various ways
#> * Mean travel time to work (minutes)
#> * Median household income
#> * Mean household income
#> * Fraction of the (civilian) population with/without health insurance
#> * Fraction of families who had an income less than poverty level in the last year
#> 
#> These variables are collected from the 2018 American Community Survey (5 year) in order to ensure that we have data for each county.
#> Please note that we are willing (and easily able!) to add other years or variables if there is interest --- The variables that we do include are because people have asked about them.
#> 
#> Source(s):
#> 
#> US Census American Community Survey (https://www.census.gov/programs-surveys/acs)
#> CMDC Client

info(cl, "covid_historical")
#> Request builder for covid_historical endpoint
#> Valid filters are vintage, dt, fips, variable, value, select, order, Range, Range-Unit, offset, limit, Prefer
#> 
#> 
#> This table returns all vintages (data from different collection dates) of data in our system.
#> 
#> For only the most recent data, please use the `covid` endpoint.
#> 
#> Currently, the following variables are collected in the database
#> 
#> * `cases_suspected`: Total number of suspected cases
#> * `cases_confirmed`: Total number of confirmed cases
#> * `cases_total`: The number of suspected or confirmed cases
#> * `deaths_suspected`: The number of deaths that are suspected to have been caused by COVID-19
#> * `deaths_confirmed`: The number of deaths that are confirmed to have been caused by COVID-19
#> * `deaths_total`: The number of deaths that are either suspected or confirmed to have been caused by COVID-19
#> * `positive_tests_total`: The total number of tests that have been positive
#> * `negative_tests_total`: The total number of tests that have been negative
#> * `icu_beds_capacity_count`: The number of ICU beds available in the geography
#> * `icu_beds_in_use_any`: The number of ICU beds currently in use
#> * `icu_beds_in_use_covid_suspected`: The number of ICU beds currently in use by a patient suspected of COVID-19
#> * `icu_beds_in_use_covid_confirmed`: The number of ICU beds currently in use by a patient confirmed to have COVID-19
#> * `icu_beds_in_use_covid_total`: The number of ICU beds currently in use by a patient who is suspected of having or confirmed to have COVID-19
#> * `icu_beds_in_use_covid_new`: The number of ICU beds occupied by an individual suspected or confirmed of having COVID-19 that have been admitted today
#> * `hospital_beds_capacity_count`: The number of hospital beds available in the geography
#> * `hospital_beds_in_use_any`: The number of hospital beds currently in use
#> * `hospital_beds_in_use_covid_suspected`: The number of hospital beds currently in use by a patient suspected of COVID-19
#> * `hospital_beds_in_use_covid_confirmed`: The number of hospital beds currently in use by a patient confirmed to have COVID-19
#> * `hospital_beds_in_use_covid_total`: The number of hospital beds currently in use by a patient who is suspected of having or confirmed to have COVID-19
#> * `hospital_beds_in_use_covid_new`: The number of hospital beds occupied by an individual suspected or confirmed of having COVID-19 that have been admitted today
#> * `ventilators_capacity_count`: The number of individuals who can be supported by a ventilator
#> * `ventilators_in_use_any`: The number of individuals who are currently on a ventilator
#> * `ventilators_in_use_covid_suspected`: The number of individuals who are suspected of having COVID-19 that are currently on a ventilator
#> * `ventilators_in_use_covid_confirmed`: The number of individuals who are confirmed to have COVID-19 that are currently on a ventilator
#> * `ventilators_in_use_covid_total`: The number of individuals who are either suspected of having or confirmed to have COVID-19 that are on a ventilator
#> * `ventilators_in_use_covid_new`: The number of ventilators that are currently on a ventilator that are suspected of having or confirmed to have COVID-19 that started the ventilator today
#> * `recovered_total`: The number of individuals who tested positive for COVID-19 and no longer test positive
#> * `active_total`: The number of currently active COVID-19 cases
#> 
#> These variables are only collected from official US federal/state/county government sources
#> CMDC Client
```

## Requesting Data

Requesting a dataset has three parts:

1.  Create a client
2.  Build a request with desired datasets
3.  `fetch` the datasets

### 1\. Create a client

To create a client, use the `client` function as shown above

``` r
cl <- client()
```

You can optionally pass in an API key if you have one (see section on
API keys below)

``` r
cl <- client("my api key")
```

If you have previously registered for an API key (again, see below) on
your current machine, it will be loaded and used automatically for you

In practice you should rarely need to pass the apikey by hand unless you
are loading the key from an environment variable or another source

### 2\. Build a request

Each of the datasets in the API have an associated function

To add datasets to the current request, `datasetName(client)` function:

``` r
covid(cl, state="CA")
#> CMDC Client. Current request:
#>   -covid: {'fips': [6001, 6003, 6005, 6007, 6009, 6011, 6013, 6015, 6017, 6019, 6021, 6023, 6025, 6027, 6029, 6031, 6033, 6035, 6037, 6039, 6041, 6043, 6045, 6047, 6049, 6051, 6053, 6055, 6057, 6059, 6061, 6063, 6065, 6067, 6069, 6071, 6073, 6075, 6077, 6079, 6081, 6083, 6085, 6087, 6089, 6091, 6093, 6095, 6097, 6099, 6101, 6103, 6105, 6107, 6109, 6111, 6113, 6115, 6]}

demographics(cl)
#> CMDC Client. Current request:
#>   -covid: {'fips': [6001, 6003, 6005, 6007, 6009, 6011, 6013, 6015, 6017, 6019, 6021, 6023, 6025, 6027, 6029, 6031, 6033, 6035, 6037, 6039, 6041, 6043, 6045, 6047, 6049, 6051, 6053, 6055, 6057, 6059, 6061, 6063, 6065, 6067, 6069, 6071, 6073, 6075, 6077, 6079, 6081, 6083, 6085, 6087, 6089, 6091, 6093, 6095, 6097, 6099, 6101, 6103, 6105, 6107, 6109, 6111, 6113, 6115, 6]}
#>   -demographics: {}

cl
#> CMDC Client. Current request:
#>   -covid: {'fips': [6001, 6003, 6005, 6007, 6009, 6011, 6013, 6015, 6017, 6019, 6021, 6023, 6025, 6027, 6029, 6031, 6033, 6035, 6037, 6039, 6041, 6043, 6045, 6047, 6049, 6051, 6053, 6055, 6057, 6059, 6061, 6063, 6065, 6067, 6069, 6071, 6073, 6075, 6077, 6079, 6081, 6083, 6085, 6087, 6089, 6091, 6093, 6095, 6097, 6099, 6101, 6103, 6105, 6107, 6109, 6111, 6113, 6115, 6]}
#>   -demographics: {}
```

You can see that the printed form of the client is updated to show you
what the current request looks like

To clear the current request, use `reset(cl)`:

``` r
reset(cl)
#> CMDC Client
#> CMDC Client
```

Each dataset function will build up a request for the client and will
return the client itself

This allows us to use the pipe operator (`%>%`) to do the above as:

``` r
cl %>% covid(state="CA") %>% demographics()
#> CMDC Client. Current request:
#>   -covid: {'fips': [6001, 6003, 6005, 6007, 6009, 6011, 6013, 6015, 6017, 6019, 6021, 6023, 6025, 6027, 6029, 6031, 6033, 6035, 6037, 6039, 6041, 6043, 6045, 6047, 6049, 6051, 6053, 6055, 6057, 6059, 6061, 6063, 6065, 6067, 6069, 6071, 6073, 6075, 6077, 6079, 6081, 6083, 6085, 6087, 6089, 6091, 6093, 6095, 6097, 6099, 6101, 6103, 6105, 6107, 6109, 6111, 6113, 6115, 6]}
#>   -demographics: {}
```

#### Filtering data

Each of the dataset functions has a number of filters that can be
applied

This allows you to select certain rows and/or columns

For example, in the above example we had `covid(state="CA")`. This
instructs the client to only fetch data for counties in the state of
California

Refer to the `info` for each dataset’s function for more information on
which filters can be passed

Also, check out the examples section at the end for more examples

**NOTE:** If a filter is passed to one dataset in the request but is
applicable to other datasets in the request, it will be applied to *all*
datasets

For example in `cl %>% covid(state="CA") %>% demographics()` we only
specify a `state` filter on the `covid` dataset

However, when the data is collected it will also be applied to
`demographics`

We do this because we end up doing an inner join on all requested
datasets, so when we filter the state in `covid` they also get filtered
in `demographics`

### 3\. Fetch the data

Now for the easy part\!

When you are ready with your current

To fetch the data, call the `fetch` function on the client:

``` r
df <- fetch(cl)
df
#> # A tibble: 2,941 x 44
#>    dt                   fips vintage             cases_total deaths_total hospital_beds_i… hospital_beds_i… hospital_beds_i… icu_beds_in_use… icu_beds_in_use… icu_beds_in_use… negative_tests_…
#>    <dttm>              <dbl> <dttm>                    <dbl>        <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>
#>  1 2020-03-03 19:00:00     6 2020-06-09 20:00:00         NaN          NaN              NaN              NaN              NaN              NaN              NaN              NaN              462
#>  2 2020-03-04 19:00:00     6 2020-06-09 20:00:00         NaN          NaN              NaN              NaN              NaN              NaN              NaN              NaN              462
#>  3 2020-03-05 19:00:00     6 2020-06-09 20:00:00         NaN          NaN              NaN              NaN              NaN              NaN              NaN              NaN              462
#>  4 2020-03-06 19:00:00     6 2020-06-09 20:00:00         NaN          NaN              NaN              NaN              NaN              NaN              NaN              NaN              462
#>  5 2020-03-07 19:00:00     6 2020-06-09 20:00:00         NaN          NaN              NaN              NaN              NaN              NaN              NaN              NaN              462
#>  6 2020-03-08 20:00:00     6 2020-06-09 20:00:00         NaN          NaN              NaN              NaN              NaN              NaN              NaN              NaN              690
#>  7 2020-03-09 20:00:00     6 2020-06-09 20:00:00         NaN          NaN              NaN              NaN              NaN              NaN              NaN              NaN              690
#>  8 2020-03-10 20:00:00     6 2020-06-09 20:00:00         NaN          NaN              NaN              NaN              NaN              NaN              NaN              NaN              916
#>  9 2020-03-11 20:00:00     6 2020-06-09 20:00:00         NaN            4              NaN              NaN              NaN              NaN              NaN              NaN              916
#> 10 2020-03-12 20:00:00     6 2020-06-09 20:00:00         NaN            4              NaN              NaN              NaN              NaN              NaN              NaN              916
#> # … with 2,931 more rows, and 32 more variables: positive_tests_total <dbl>, meta_date <dttm>, `Fraction of population over 65_2018-01-01` <dbl>, `Mean household income_2018-01-01` <dbl>,
#> #   `Mean travel time to work (minutes)_2018-01-01` <dbl>, `Median age_2018-01-01` <dbl>, `Median household income_2018-01-01` <dbl>, `Percent Asian_2018-01-01` <dbl>, `Percent
#> #   Hispanic/Latino (any race)_2018-01-01` <dbl>, `Percent Native American or Alaska Native_2018-01-01` <dbl>, `Percent Native Hawaiian or other Pacific Islander_2018-01-01` <dbl>, `Percent
#> #   black_2018-01-01` <dbl>, `Percent of 16+ commute carpool_2018-01-01` <dbl>, `Percent of 16+ commute driving alone_2018-01-01` <dbl>, `Percent of 16+ commute other way_2018-01-01` <dbl>,
#> #   `Percent of 16+ commute public transit_2018-01-01` <dbl>, `Percent of 16+ commute walk_2018-01-01` <dbl>, `Percent of 16+ commute work at home_2018-01-01` <dbl>, `Percent of 25+ with
#> #   Associate degree_2018-01-01` <dbl>, `Percent of 25+ with Bachelor degree_2018-01-01` <dbl>, `Percent of 25+ with HS degree but no college_2018-01-01` <dbl>, `Percent of 25+ with less than
#> #   9th grade education_2018-01-01` <dbl>, `Percent of 25+ with professional degree_2018-01-01` <dbl>, `Percent of 25+ with some HS but no HS degree_2018-01-01` <dbl>, `Percent of 25+ with
#> #   some college but no degree_2018-01-01` <dbl>, `Percent of civilian population with health insurance_2018-01-01` <dbl>, `Percent of civilian population with no health
#> #   insurance_2018-01-01` <dbl>, `Percent of families w/ income < poverty lvl in last year_2018-01-01` <dbl>, `Percent other race_2018-01-01` <dbl>, `Percent two or more
#> #   races_2018-01-01` <dbl>, `Percent white_2018-01-01` <dbl>, `Total population_2018-01-01` <dbl>

names(df)
#>  [1] "dt"                                                                  "fips"                                                               
#>  [3] "vintage"                                                             "cases_total"                                                        
#>  [5] "deaths_total"                                                        "hospital_beds_in_use_covid_confirmed"                               
#>  [7] "hospital_beds_in_use_covid_suspected"                                "hospital_beds_in_use_covid_total"                                   
#>  [9] "icu_beds_in_use_covid_confirmed"                                     "icu_beds_in_use_covid_suspected"                                    
#> [11] "icu_beds_in_use_covid_total"                                         "negative_tests_total"                                               
#> [13] "positive_tests_total"                                                "meta_date"                                                          
#> [15] "Fraction of population over 65_2018-01-01"                           "Mean household income_2018-01-01"                                   
#> [17] "Mean travel time to work (minutes)_2018-01-01"                       "Median age_2018-01-01"                                              
#> [19] "Median household income_2018-01-01"                                  "Percent Asian_2018-01-01"                                           
#> [21] "Percent Hispanic/Latino (any race)_2018-01-01"                       "Percent Native American or Alaska Native_2018-01-01"                
#> [23] "Percent Native Hawaiian or other Pacific Islander_2018-01-01"        "Percent black_2018-01-01"                                           
#> [25] "Percent of 16+ commute carpool_2018-01-01"                           "Percent of 16+ commute driving alone_2018-01-01"                    
#> [27] "Percent of 16+ commute other way_2018-01-01"                         "Percent of 16+ commute public transit_2018-01-01"                   
#> [29] "Percent of 16+ commute walk_2018-01-01"                              "Percent of 16+ commute work at home_2018-01-01"                     
#> [31] "Percent of 25+ with Associate degree_2018-01-01"                     "Percent of 25+ with Bachelor degree_2018-01-01"                     
#> [33] "Percent of 25+ with HS degree but no college_2018-01-01"             "Percent of 25+ with less than 9th grade education_2018-01-01"       
#> [35] "Percent of 25+ with professional degree_2018-01-01"                  "Percent of 25+ with some HS but no HS degree_2018-01-01"            
#> [37] "Percent of 25+ with some college but no degree_2018-01-01"           "Percent of civilian population with health insurance_2018-01-01"    
#> [39] "Percent of civilian population with no health insurance_2018-01-01"  "Percent of families w/ income < poverty lvl in last year_2018-01-01"
#> [41] "Percent other race_2018-01-01"                                       "Percent two or more races_2018-01-01"                               
#> [43] "Percent white_2018-01-01"                                            "Total population_2018-01-01"
```

Notice that after each successful request, the client is reset so there
are no “built-up” requests:

``` r
cl
#> CMDC Client
```

## API keys

Our API is and always will be free for unlimited public use

However, we have an API key system in place to help us understand the
needs of our users

We kindly request that you register for an API key so we can understand
how to prioritize future work

In order to do so, you can use the `register` function

``` r
register(cl)
```

By default, function will prompt you to input an email address

You can also pass the email address as the second argument for
non-interactive use

``` r
register(cl, "me@me.com")
```

After you `register` for an API key it will be added to the client. All
future requests with this client will use the API key

We also save the key to a file at `~/.cmdc/apikey`

If this file exists, each time you call `client` and do not explicitly
pass an apikey we will read the key from `~/.cmdc/apikey` and
automatically apply it for you

Thus, to use the key in future sessions you just need to do `cl <-
client()` and we’ll handle the key for you\!

## Final thoughts

Due to the urgency of the COVID-19 crisis and the need for researchers,
modelers, and policy makers to have accurate data quickly, this project
moves fast\!

We have created this library so that as we add new datasets to our
backend, they automatically appear here and are accessible via this
library

Please check back often and see what has been updated

### Examples

``` r
# Single dataset all
cl %>% mobility_devices() %>% fetch()
#> # A tibble: 279,315 x 6
#>    dt                   fips   dex dex_a num_devices num_devices_a
#>    <dttm>              <dbl> <dbl> <dbl>       <dbl>         <dbl>
#>  1 2020-01-19 19:00:00     1  169.  157.      499012        537243
#>  2 2020-01-19 19:00:00     2  138.  129.       30999         33130
#>  3 2020-01-19 19:00:00     4  158.  146.      443186        478038
#>  4 2020-01-19 19:00:00     5  159.  147.      266296        287199
#>  5 2020-01-19 19:00:00     6  184.  171.     1758372       1889100
#>  6 2020-01-19 19:00:00     8  237.  218.      349252        381059
#>  7 2020-01-19 19:00:00     9  139.  129.      204366        221176
#>  8 2020-01-19 19:00:00    10  167.  151.       61097         67674
#>  9 2020-01-19 19:00:00    11  166.  120.       22766         31604
#> 10 2020-01-19 19:00:00    12  294.  270.     1694376       1851355
#> # … with 279,305 more rows
```

``` r
# Single dataset filter on deaths
cl %>% covid(fips="<100", variable="deaths_total", value=">100") %>% fetch()
#> # A tibble: 2,406 x 4
#>    dt                   fips vintage             deaths_total
#>    <dttm>              <dbl> <dttm>                     <dbl>
#>  1 2020-03-19 20:00:00    53 2020-06-09 20:00:00          104
#>  2 2020-03-20 20:00:00    53 2020-06-09 20:00:00          111
#>  3 2020-03-21 20:00:00    36 2020-06-09 20:00:00          114
#>  4 2020-03-21 20:00:00    53 2020-06-09 20:00:00          126
#>  5 2020-03-22 20:00:00    36 2020-06-09 20:00:00          114
#>  6 2020-03-22 20:00:00    53 2020-06-09 20:00:00          137
#>  7 2020-03-23 20:00:00    36 2020-06-09 20:00:00          210
#>  8 2020-03-23 20:00:00    53 2020-06-09 20:00:00          146
#>  9 2020-03-24 20:00:00    36 2020-06-09 20:00:00          285
#> 10 2020-03-24 20:00:00    53 2020-06-09 20:00:00          159
#> # … with 2,396 more rows
```

``` r
# Single dataset single states with all counties
# OR: `cl %>% mobility_devices(state=as.integer(48)) %>% fetch()`
# OR: `cl %>% mobility_devices(state="TX") %>% fetch()`
cl %>% mobility_devices(state="48") %>% fetch()
#> # A tibble: 19,980 x 6
#>    dt                   fips   dex dex_a num_devices num_devices_a
#>    <dttm>              <dbl> <dbl> <dbl>       <dbl>         <dbl>
#>  1 2020-01-19 19:00:00    48 380.  350.      2286293       2484430
#>  2 2020-01-19 19:00:00 48001 119.  110.         4542          4895
#>  3 2020-01-19 19:00:00 48003  81.4  74.7        1807          1967
#>  4 2020-01-19 19:00:00 48005 209.  195.         7748          8291
#>  5 2020-01-19 19:00:00 48007  95.5  84.0        2718          3090
#>  6 2020-01-19 19:00:00 48013 112.  102.         4754          5197
#>  7 2020-01-19 19:00:00 48015 156.  144.         3024          3286
#>  8 2020-01-19 19:00:00 48019 136.  125.         2266          2473
#>  9 2020-01-19 19:00:00 48021 185.  166.         7492          8375
#> 10 2020-01-19 19:00:00 48025  91.6  83.4        1999          2194
#> # … with 19,970 more rows
```

``` r
# Single dataset multiple states with all counties
cl %>% mobility_devices(state=c("CA", "TX")) %>% fetch()
#> # A tibble: 26,865 x 6
#>    dt                   fips   dex dex_a num_devices num_devices_a
#>    <dttm>              <dbl> <dbl> <dbl>       <dbl>         <dbl>
#>  1 2020-01-19 19:00:00     6 184.  171.      1758372       1889100
#>  2 2020-01-19 19:00:00    48 380.  350.      2286293       2484430
#>  3 2020-01-19 19:00:00  6001 176.  159.        59993         66390
#>  4 2020-01-19 19:00:00  6005  59.9  56.3        2073          2207
#>  5 2020-01-19 19:00:00  6007 113.  104.        10931         11892
#>  6 2020-01-19 19:00:00  6009  46.8  45.0        3100          3227
#>  7 2020-01-19 19:00:00  6013 158.  150.        50593         53338
#>  8 2020-01-19 19:00:00  6015  41.7  40.4        1334          1379
#>  9 2020-01-19 19:00:00  6017 139.  134.        11561         12033
#> 10 2020-01-19 19:00:00  6019 120.  111.        45808         49425
#> # … with 26,855 more rows
```

``` r
# Single dataset variable select
cl %>% demographics(variable = c("Total population", "Fraction of population over 65", "Median age")) %>% fetch()
#> # A tibble: 878 x 5
#>     fips meta_date           `Fraction of population over 65_2018-01-01` `Median age_2018-01-01` `Total population_2018-01-01`
#>    <dbl> <dttm>                                                    <dbl>                   <dbl>                         <dbl>
#>  1     1 2017-12-31 19:00:00                                        17                      39.3                       4887871
#>  2     2 2017-12-31 19:00:00                                        11.9                    34.9                        737438
#>  3     4 2017-12-31 19:00:00                                        17.6                    38                         7171646
#>  4     5 2017-12-31 19:00:00                                        16.8                    38.1                       3013825
#>  5     6 2017-12-31 19:00:00                                        14.3                    36.7                      39557044
#>  6     8 2017-12-31 19:00:00                                        14.2                    36.9                       5695564
#>  7     9 2017-12-31 19:00:00                                        17.2                    41.1                       3572665
#>  8    10 2017-12-31 19:00:00                                        18.7                    41.1                        967171
#>  9    11 2017-12-31 19:00:00                                        12.2                    33.9                        702455
#> 10    12 2017-12-31 19:00:00                                        20.5                    42.2                      21299324
#> # … with 868 more rows
```

``` r
# Multiple datasets all data
cl %>% demographics() %>% covid() %>% fetch()
#> # A tibble: 8,622 x 47
#>     fips meta_date           `Fraction of po… `Mean household… `Mean travel ti… `Median age_201… `Median househo… `Percent Asian_… `Percent Hispan… `Percent Native… `Percent Native…
#>    <dbl> <dttm>                         <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>
#>  1     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  2     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  3     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  4     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  5     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  6     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  7     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  8     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  9     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#> 10     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#> # … with 8,612 more rows, and 36 more variables: `Percent black_2018-01-01` <dbl>, `Percent of 16+ commute carpool_2018-01-01` <dbl>, `Percent of 16+ commute driving alone_2018-01-01` <dbl>,
#> #   `Percent of 16+ commute other way_2018-01-01` <dbl>, `Percent of 16+ commute public transit_2018-01-01` <dbl>, `Percent of 16+ commute walk_2018-01-01` <dbl>, `Percent of 16+ commute work
#> #   at home_2018-01-01` <dbl>, `Percent of 25+ with Associate degree_2018-01-01` <dbl>, `Percent of 25+ with Bachelor degree_2018-01-01` <dbl>, `Percent of 25+ with HS degree but no
#> #   college_2018-01-01` <dbl>, `Percent of 25+ with less than 9th grade education_2018-01-01` <dbl>, `Percent of 25+ with professional degree_2018-01-01` <dbl>, `Percent of 25+ with some HS
#> #   but no HS degree_2018-01-01` <dbl>, `Percent of 25+ with some college but no degree_2018-01-01` <dbl>, `Percent of civilian population with health insurance_2018-01-01` <dbl>, `Percent of
#> #   civilian population with no health insurance_2018-01-01` <dbl>, `Percent of families w/ income < poverty lvl in last year_2018-01-01` <dbl>, `Percent other race_2018-01-01` <dbl>,
#> #   `Percent two or more races_2018-01-01` <dbl>, `Percent white_2018-01-01` <dbl>, `Total population_2018-01-01` <dbl>, dt <dttm>, vintage <dttm>, active_total <dbl>, cases_total <dbl>,
#> #   deaths_total <dbl>, hospital_beds_in_use_covid_confirmed <dbl>, hospital_beds_in_use_covid_suspected <dbl>, hospital_beds_in_use_covid_total <dbl>, icu_beds_in_use_covid_confirmed <dbl>,
#> #   icu_beds_in_use_covid_suspected <dbl>, icu_beds_in_use_covid_total <dbl>, negative_tests_total <dbl>, positive_tests_total <dbl>, recovered_total <dbl>,
#> #   ventilators_in_use_covid_total <dbl>
```

``` r
# Multiple datasets states only
cl %>% demographics() %>% covid(fips="<100") %>% fetch()
#> # A tibble: 4,963 x 40
#>     fips meta_date           `Fraction of po… `Mean household… `Mean travel ti… `Median age_201… `Median househo… `Percent Asian_… `Percent Hispan… `Percent Native… `Percent Native…
#>    <dbl> <dttm>                         <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>
#>  1     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  2     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  3     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  4     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  5     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  6     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  7     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  8     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#>  9     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#> 10     1 2017-12-31 19:00:00               17            69091             25.2             39.3            49861              1.3              4.3              0.5                0
#> # … with 4,953 more rows, and 29 more variables: `Percent black_2018-01-01` <dbl>, `Percent of 16+ commute carpool_2018-01-01` <dbl>, `Percent of 16+ commute driving alone_2018-01-01` <dbl>,
#> #   `Percent of 16+ commute other way_2018-01-01` <dbl>, `Percent of 16+ commute public transit_2018-01-01` <dbl>, `Percent of 16+ commute walk_2018-01-01` <dbl>, `Percent of 16+ commute work
#> #   at home_2018-01-01` <dbl>, `Percent of 25+ with Associate degree_2018-01-01` <dbl>, `Percent of 25+ with Bachelor degree_2018-01-01` <dbl>, `Percent of 25+ with HS degree but no
#> #   college_2018-01-01` <dbl>, `Percent of 25+ with less than 9th grade education_2018-01-01` <dbl>, `Percent of 25+ with professional degree_2018-01-01` <dbl>, `Percent of 25+ with some HS
#> #   but no HS degree_2018-01-01` <dbl>, `Percent of 25+ with some college but no degree_2018-01-01` <dbl>, `Percent of civilian population with health insurance_2018-01-01` <dbl>, `Percent of
#> #   civilian population with no health insurance_2018-01-01` <dbl>, `Percent of families w/ income < poverty lvl in last year_2018-01-01` <dbl>, `Percent other race_2018-01-01` <dbl>,
#> #   `Percent two or more races_2018-01-01` <dbl>, `Percent white_2018-01-01` <dbl>, `Total population_2018-01-01` <dbl>, dt <dttm>, vintage <dttm>, deaths_total <dbl>,
#> #   hospital_beds_in_use_covid_total <dbl>, icu_beds_in_use_covid_total <dbl>, negative_tests_total <dbl>, positive_tests_total <dbl>, ventilators_in_use_covid_total <dbl>
```

``` r
# Multiple datasets counties only
cl %>% demographics() %>% covid(fips=">1000") %>% fetch()
#> # A tibble: 3,659 x 46
#>     fips meta_date           `Fraction of po… `Mean household… `Mean travel ti… `Median age_201… `Median househo… `Percent Asian_… `Percent Hispan… `Percent Native… `Percent Native…
#>    <dbl> <dttm>                         <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>            <dbl>
#>  1  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#>  2  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#>  3  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#>  4  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#>  5  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#>  6  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#>  7  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#>  8  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#>  9  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#> 10  2020 2017-12-31 19:00:00             11.3           106660             19.6             34.3            83648              9.7              9.3              7.6              2.1
#> # … with 3,649 more rows, and 35 more variables: `Percent black_2018-01-01` <dbl>, `Percent of 16+ commute carpool_2018-01-01` <dbl>, `Percent of 16+ commute driving alone_2018-01-01` <dbl>,
#> #   `Percent of 16+ commute other way_2018-01-01` <dbl>, `Percent of 16+ commute public transit_2018-01-01` <dbl>, `Percent of 16+ commute walk_2018-01-01` <dbl>, `Percent of 16+ commute work
#> #   at home_2018-01-01` <dbl>, `Percent of 25+ with Associate degree_2018-01-01` <dbl>, `Percent of 25+ with Bachelor degree_2018-01-01` <dbl>, `Percent of 25+ with HS degree but no
#> #   college_2018-01-01` <dbl>, `Percent of 25+ with less than 9th grade education_2018-01-01` <dbl>, `Percent of 25+ with professional degree_2018-01-01` <dbl>, `Percent of 25+ with some HS
#> #   but no HS degree_2018-01-01` <dbl>, `Percent of 25+ with some college but no degree_2018-01-01` <dbl>, `Percent of civilian population with health insurance_2018-01-01` <dbl>, `Percent of
#> #   civilian population with no health insurance_2018-01-01` <dbl>, `Percent of families w/ income < poverty lvl in last year_2018-01-01` <dbl>, `Percent other race_2018-01-01` <dbl>,
#> #   `Percent two or more races_2018-01-01` <dbl>, `Percent white_2018-01-01` <dbl>, `Total population_2018-01-01` <dbl>, dt <dttm>, vintage <dttm>, active_total <dbl>, cases_total <dbl>,
#> #   deaths_total <dbl>, hospital_beds_in_use_covid_confirmed <dbl>, hospital_beds_in_use_covid_suspected <dbl>, hospital_beds_in_use_covid_total <dbl>, icu_beds_in_use_covid_confirmed <dbl>,
#> #   icu_beds_in_use_covid_suspected <dbl>, icu_beds_in_use_covid_total <dbl>, negative_tests_total <dbl>, positive_tests_total <dbl>, recovered_total <dbl>
```
