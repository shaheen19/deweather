
<!-- Edit the README.Rmd only!!! The README.md is generated automatically from README.Rmd. -->
deweather: an R package to remove meteorological variation from air quality data
================================================================================

<img src="inst/plume.png" alt="openair logo" width="35%" />

**deweather** is an R package developed for the purpose of 'removing' the influence of meteorology from air quality time series data. It is part of the [openair](http://davidcarslaw.github.io/openair/) suite of packages designed to support the analysis of air quality data and related data.

The **deweather** package uses a *boosted regression tree* approach for modelling air quality data. These and similar techniques provide powerful tools for building statistical models of air quality data. They are able to take account of the many complex interations between variables as well as non-linear relationships between the variables.

The modelling can be computatuonally intensive and therefore **deweather** makes use of the parallel processing, which should work on Windows, linux and Mac OSX.

Installation
------------

Installation of **deweather** from GitHub should be easy using the devtools package.

``` r
require(devtools)
install_github('davidcarslaw/deweather')
```

Description
-----------

Meteorology play a central role in affecting teh concentrations of pollutants in teh atmosphere. When considering trends in air pollutants it can be very difficult to know whether a change in concentration is due to emissions or meteorology.

The **deweather** package uses a powerful statistical technique based on *boosted regression trees* using the **gbm** package (Ridgeway, 2017). Statistical models are developed to explain concentrations using meteorological and other variables. These models can be tested on randomly withheld data with the aim of developing the most appropriate model.

Example data set
----------------

The **deweather** package comes with a comprehensive data set of air quality and meteorological data. The air quality data is from Marylebone Road in central London (obtained from teh **openair** package) and the meteorological data from Heathrow Airport (obtained from the **worldmet** package).

``` r
# library(openair)
# kc1 <- importAURN(site = "kc1", year = 2011:2012)
# head(kc1)
```

For those interested in obtaining the data directly, the following code can be used.

``` r
library(openair)
library(worldmet)
library(dplyr)
library(deweather)

# import AQ data
road_data <- importAURN(site = "my1", year = 1998:2016, hc = TRUE)

# import met data
met <- importNOAA(year = 1998:2016)

# join together but ignore met data in road_data because it is modelled
road_data <- left_join(select(road_data, -ws, -wd), met, by = "date")

road_data <- select(road_data, date, nox, no2, ethane, isoprene, 
                    benzene, ws, wd, air_temp, RH, cl)
```

``` r
#windRose(mydata)
```

Construct and test model(s)
---------------------------

Build a model
-------------

Examine the partial dependencies
--------------------------------

Apply meteorological averaging
------------------------------

References
----------

Carslaw, D.C. and P.J. Taylor (2009). Analysis of air pollution data at a mixed source location using boosted regression trees. Atmospheric Environment. Vol. 43, pp. 3563–3570.

Carslaw, D.C., Williams, M.L. and B. Barratt A short-term intervention study — impact of airport closure on near-field air quality due to the eruption of Eyjafjallajökull. (2012) Atmospheric Environment, Vol. 54, 328–336.

Greg Ridgeway with contributions from others (2017). gbm: Generalized Boosted Regression Models. Rpackage version 2.1.3. (<https://CRAN.R-project.org/package=gbm>)
