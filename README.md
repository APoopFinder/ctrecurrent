
<!-- README.md is generated from README.Rmd. Please edit that file -->

# ctrecurrent

<!-- badges: start -->
<!-- badges: end -->

The goal of ctrecurrent is to transform the camera trap data into a
format suitable for recurrent event analysis. It contains the function
recurrent() to do so, requiring a dataframe with the following
information for each observation: Site ID, Timestamp (Date and Time) and
Species.

## Installation

You can install the development version of ctrecurrent from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("APoopFinder/ctrecurrent")
```

## Example

``` r
library(ctrecurrent)
library(dplyr)

# See data's vignette for details
df = Murphy2021_ct_data

# Define the pool of primary species (affecting secondary species, stopping the survey if observed)
Primary = c("Fawn", "Deer", "Bear", "Bobcat", "Human", "Motorized")

# Define the secondary species (affected bby the primary species, only one)
Secondary = c("Coyote")

# Define the end of study
End_survey = max(df$DateTime)

# Convert into recurrent event
recu = recurrent(df, Primary, Secondary, End_survey, survey_duration = 7)

# Select only one primary species for recurrent event analysis
recu_deer = recu %>% filter(Primary == "Deer")
head(recu_deer)
#>       Site         Id Primary    DateTime_Primary            DateTime   t.start
#> 1 2016BE15 2016BE15-3    Deer 2016-08-05 01:34:00 2016-08-15 20:23:00 0.0000000
#> 2 2016BE22 2016BE22-6    Deer 2016-08-26 09:13:00 2016-08-26 17:29:00 0.0000000
#> 3 2016BE22 2016BE22-6    Deer 2016-08-26 09:13:00 2016-08-27 07:45:00 0.3444444
#> 4 2016BE22 2016BE22-9    Deer 2016-09-01 17:12:00 2016-09-07 14:33:00 0.0000000
#> 5 2016BE22 2016BE22-9    Deer 2016-09-01 17:12:00 2016-09-09 02:04:00 5.8895833
#> 6 2016BE98 2016BE98-3    Deer 2016-08-17 01:36:00 2016-09-04 00:31:00 0.0000000
#>      t.stop event enum status                          Event_type matrix
#> 1 7.0000000     0    1      0        Censoring event - Survey end  agdev
#> 2 0.3444444     1    1      0                              Coyote  agdev
#> 3 0.9388889     0    2      0 Censoring event - Following Primary  agdev
#> 4 5.8895833     1    1      0                              Coyote  agdev
#> 5 7.0000000     0    2      0        Censoring event - Survey end  agdev
#> 6 7.0000000     0    1      0        Censoring event - Survey end  agdev
```
