
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

``` r
library(tidyr)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(ggplot2)
library(readr)
```

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
#Use Start With to skip the middle columns and get to the columsn that start with Death



av <- av %>%
  mutate(
    times = rowSums(across(c(Death1, Death2, Death3, Death4, Death5)) == "YES")
  )


deaths <- av |> pivot_longer(cols = starts_with("Death"), values_to = "Death")


deaths <- deaths %>% 
  distinct(Name.Alias, .keep_all = TRUE)

deaths <- deaths %>%
  select(-c(name, Return1, Return2, Return3, Return4, Return5))

print(deaths$times)
```

    ##   [1] 1 1 1 1 2 0 1 2 1 1 1 0 0 1 0 1 1 0 2 1 2 1 1 0 0 0 0 0 0 3 0 0 5 0 1 0 0
    ##  [38] 0 1 1 1 0 0 0 1 0 0 1 0 1 1 2 2 2 1 2 1 0 0 1 0 1 0 0 0 0 1 1 0 0 2 0 0 1
    ##  [75] 0 2 0 1 1 0 0 0 2 0 0 1 1 1 2 0 0 0 0 1 0 1 0 0 0 2 0 0 0 0 0 0 1 1 0 1 0
    ## [112] 1 0 1 0 0 0 0 0 0 0 0 0 0 1 1 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 1 0 1 1 1
    ## [149] 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0

``` r
print(deaths$Death)
```

    ##   [1] "YES" "YES" "YES" "YES" "YES" "NO"  "YES" "YES" "YES" "YES" "YES" "NO" 
    ##  [13] "NO"  "YES" "NO"  "YES" "YES" "NO"  "YES" "YES" "YES" "YES" "YES" "NO" 
    ##  [25] "NO"  "NO"  "NO"  "NO"  "NO"  "YES" "NO"  "NO"  "YES" "NO"  "YES" "NO" 
    ##  [37] "NO"  "NO"  "YES" "YES" "YES" "NO"  "NO"  "NO"  "YES" "NO"  "NO"  "YES"
    ##  [49] "NO"  "YES" "YES" "YES" "YES" "YES" "YES" "YES" "YES" "NO"  "NO"  "YES"
    ##  [61] "NO"  "YES" "NO"  "NO"  "NO"  "NO"  "YES" "YES" "NO"  "NO"  "YES" "NO" 
    ##  [73] "NO"  "YES" "NO"  "YES" "NO"  "YES" "YES" "NO"  "NO"  "NO"  "YES" "NO" 
    ##  [85] "NO"  "YES" "YES" "YES" "YES" "NO"  "NO"  "NO"  "NO"  "YES" "NO"  "YES"
    ##  [97] "NO"  "NO"  "NO"  "YES" "NO"  "NO"  "NO"  "NO"  "NO"  "NO"  "YES" "YES"
    ## [109] "NO"  "YES" "NO"  "YES" "NO"  "YES" "NO"  "NO"  "NO"  "NO"  "NO"  "NO" 
    ## [121] "NO"  "NO"  "NO"  "NO"  "YES" "YES" "NO"  "NO"  "NO"  "NO"  "NO"  "NO" 
    ## [133] "NO"  "NO"  "NO"  "NO"  "NO"  "YES" "NO"  "NO"  "NO"  "NO"  "NO"  "YES"
    ## [145] "NO"  "YES" "YES" "YES" "NO"  "NO"  "NO"  "NO"  "NO"  "NO"  "NO"  "YES"
    ## [157] "NO"  "NO"  "NO"  "NO"  "NO"  "NO"  "NO"

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
avgDeaths = mean(deaths$times)

print(avgDeaths)
```

    ## [1] 0.5092025

## Individually

Vaishnavi Devi

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

## Individually

Owen Arnold

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

“Out of 173 listed Avengers, my analysis found that 69 had died at least
one time after they joined the team.5 That’s about 40 percent of all
people who have ever signed on to the team.”

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
allDead <- deaths %>%
  filter(Death == "YES")

numDead = nrow(allDead)
numTotal = nrow(deaths)

percentDead = (numDead / numTotal) * 100

print(numDead)
```

    ## [1] 64

``` r
print(percentDead)
```

    ## [1] 39.2638

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

### Response

The claim is true that about %40 of avengers die, my calculations
yielded %39.26. The statement that 69 have died could be a flaw in the
dataset, some avengers may not be included.
