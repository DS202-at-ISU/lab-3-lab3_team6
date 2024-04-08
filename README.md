
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 4.3.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyr)
```

    ## Warning: package 'tidyr' was built under R version 4.3.3

``` r
library(stringr)

av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)

# reshaping the data for deaths
deaths <- av %>% select(URL, starts_with("Death")) %>% pivot_longer(cols = Death1:Death5,
                                                                           names_to = "death",
                                                                           values_to = "result") %>% group_by(URL) %>% summarise(death_count = sum(result == "YES"))  %>% summarise(death_mean = mean(death_count))

print(deaths)
```

    ## # A tibble: 1 × 1
    ##   death_mean
    ##        <dbl>
    ## 1      0.514

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

### Include the code

### Include your answer

Vedant - “Out of 173 listed Avengers, my analysis found that 69 had died
at least one time after they joined the team.5 That’s about 40 percent
of all people who have ever signed on to the team. Let’s put it this
way: If you fall from four or five stories up, there’s a 50 percent
chance you die. Getting a membership card in the Avengers is roughly
like jumping off a four-story building.”

``` r
# filter avengers who died at least once

av_deaths <- av %>%
  filter(Death1 == "YES" | Death2 == "YES" | Death3 == "YES" | Death4 == "YES" | Death5 == "YES")

num_deaths <- nrow(av_deaths)

percentage_deaths <- (num_deaths / nrow(av)) * 100

percentage_deaths
```

    ## [1] 39.88439

If they joined the avengers team, there is a 40% chance that the
character died. I did this by calculating the number of total death and
divide by total avangers which gives us 39.88 or approx 40%. Hence we
have verified the fact.

Abhi - There’s a 2-in-3 chance that a member of the Avengers returned
from their first stint in the afterlife

``` r
# Filters avengers who have died and gets the total of "YES" divided by the amount of avengers that have died
av %>% filter(Death1 == "YES") %>% select(URL, Return1) %>% summarise(n = sum(Return1 == "YES")/n())
```

    ##           n
    ## 1 0.6666667

``` r
# The data proves that there is a 2-in-3 chance that they return after death for the first time
```

Jennifer - “I counted 89 total deaths — some unlucky Avengers are
basically Meat Loaf with an E-ZPass — and on 57 occasions the individual
made a comeback.”

``` r
# grouping death and return columns for calculations

death_cols <- av[, c("Death1", "Death2", "Death3", "Death4", "Death5")]
return_cols <- av[, c("Return1", "Return2", "Return3", "Return4",
"Return5")]

#counting "YES" values in the Death1-5 and Return1-5 columns death_count
death_count<- rowSums(death_cols == "YES", na.rm = TRUE) 
return_count <- rowSums(return_cols == "YES", na.rm = TRUE)

# summing total count of "YES" values across Death1 to Death5 columns

total_death_count <- sum(death_count) 
total_return_count <- sum(return_count)

# displaying the total count of deaths and returns

print(total_death_count) 
```

    ## [1] 89

``` r
print(total_return_count) 
```

    ## [1] 57

Bela - “only a 50 percent chance they recovered from a second or third
death.”

``` r
#using the death_count computed by jen I will create a new sum of when a hero died more than twice 

# Avengers who died at least twice
av_deaths_twice <- av %>%
  filter(rowSums(death_cols == "YES", na.rm = TRUE) >= 2)

# Avengers who died at least twice and returned at least twice 
av_deaths_twice_with_return <- av %>%
  filter(rowSums(death_cols == "YES", na.rm = TRUE) >= 2 & rowSums(return_cols == "YES", na.rm = TRUE) >= 2)

# number of Avengers - satisfy previous requirements
num_deaths_twice_with_returns <- nrow(av_deaths_twice_with_return)

# percentage of Avengers who returned at least once after dying at least twice
percentage_deaths_twice_with_returns <- (num_deaths_twice_with_returns / nrow(av_deaths_twice)) * 100

print(percentage_deaths_twice_with_returns)
```

    ## [1] 50

The data supports the article’s claim that there were 89 total deaths
and 57 total returns. I calculated this by summing all the Death columns
(Death1, Death2,…, Death5) and Return columns (Return1, Return2,…,
Return5).

Upload your changes to the repository. Discuss and refine answers as a
team.
