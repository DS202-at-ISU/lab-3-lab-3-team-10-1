
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

print(av)
```

    ##                                                                     URL
    ## 1                         http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2                    http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3                     http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4               http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5                      http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6                     http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ## 7                     http://marvel.wikia.com/Steven_Rogers_(Earth-616)
    ## 8                      http://marvel.wikia.com/Clint_Barton_(Earth-616)
    ## 9                   http://marvel.wikia.com/Pietro_Maximoff_(Earth-616)
    ## 10                   http://marvel.wikia.com/Wanda_Maximoff_(Earth-616)
    ## 11                 http://marvel.wikia.com/Jacques_Duquesne_(Earth-616)
    ## 12                         http://marvel.wikia.com/Hercules_(Earth-616)
    ## 13                       http://marvel.wikia.com/T%27Challa_(Earth-616)
    ## 14                           http://marvel.wikia.com/Vision_(Earth-616)
    ## 15                                 http://marvel.wikia.com/Dane_Whitman
    ## 16                http://marvel.wikia.com/Natalia_Romanova_(Earth-616)#
    ## 17                          http://marvel.wikia.com/Mantis_(Earth-616)#
    ## 18                     http://marvel.wikia.com/Henry_McCoy_(Earth-616)#
    ## 19                 http://marvel.wikia.com/Heather_Douglas_(Earth-616)#
    ## 20                 http://marvel.wikia.com/Patricia_Walker_(Earth-616)#
    ## 21                    http://marvel.wikia.com/Matthew_Hawk_(Earth-616)#
    ## 22                    http://marvel.wikia.com/Robert_Frank_(Earth-616)#
    ## 23                  http://marvel.wikia.com/Simon_Williams_(Earth-616)#
    ## 24                    http://marvel.wikia.com/Yondu_Udonta_(Earth-691)#
    ## 25               http://marvel.wikia.com/Martinex_T%27Naga_(Earth-691)#
    ## 26                      http://marvel.wikia.com/Charlie-27_(Earth-691)#
    ## 27                 http://marvel.wikia.com/Nicholette_Gold_(Earth-691)#
    ## 28                    http://marvel.wikia.com/Stakar_Ogord_(Earth-691)#
    ## 29                     http://marvel.wikia.com/Vance_Astro_(Earth-691)#
    ## 30                        http://marvel.wikia.com/Mar-Vell_(Earth-616)#
    ## 31                   http://marvel.wikia.com/Carol_Danvers_(Earth-616)#
    ## 32                   http://marvel.wikia.com/Samuel_Wilson_(Earth-616)#
    ## 33                         http://marvel.wikia.com/Jocasta_(Earth-616)#
    ## 34                    http://marvel.wikia.com/Greer_Nelson_(Earth-616)#
    ## 35                http://marvel.wikia.com/Jennifer_Walters_(Earth-616)#
    ## 36                  http://marvel.wikia.com/Monica_Rambeau_(Earth-616)#
    ## 37                            http://marvel.wikia.com/Eros_(Earth-616)#
    ## 38                    http://marvel.wikia.com/James_Rhodes_(Earth-616)#
    ## 39                   http://marvel.wikia.com/Barbara_Morse_(Earth-616)#
    ## 40                   http://marvel.wikia.com/Moira_Brandon_(Earth-616)#
    ## 41                  http://marvel.wikia.com/Benjamin_Grimm_(Earth-616)#
    ## 42                   http://marvel.wikia.com/Bonita_Juarez_(Earth-616)#
    ## 43                    http://marvel.wikia.com/Marc_Spector_(Earth-616)#
    ## 44                     http://marvel.wikia.com/John_Walker_(Earth-616)#
    ## 45           http://marvel.wikia.com/Human_Torch_(Android)_(Earth-616)#
    ## 46                   http://marvel.wikia.com/Miguel_Santos_(Earth-616)#
    ## 47                 http://marvel.wikia.com/Julia_Carpenter_(Earth-616)#
    ## 48                    http://marvel.wikia.com/2ZP45-9-X-51_(Earth-616)#
    ## 49                    http://marvel.wikia.com/Chris_Powell_(Earth-616)#
    ## 50                  http://marvel.wikia.com/Namor_McKenzie_(Earth-616)#
    ## 51                      http://marvel.wikia.com/Scott_Lang_(Earth-616)#
    ## 52                   http://marvel.wikia.com/Anthony_Druid_(Earth-616)#
    ## 53               http://marvel.wikia.com/Marrina_Smallwood_(Earth-616)#
    ## 54              http://marvel.wikia.com/Ravonna_Renslayer_(Earth-6311)#
    ## 55                     http://marvel.wikia.com/Rita_DeMara_(Earth-616)#
    ## 56                   http://marvel.wikia.com/Dennis_Dunphy_(Earth-616)#
    ## 57                       http://marvel.wikia.com/Gilgamesh_(Earth-616)#
    ## 58                   http://marvel.wikia.com/Reed_Richards_(Earth-616)#
    ## 59                     http://marvel.wikia.com/Susan_Storm_(Earth-616)#
    ## 60                  http://marvel.wikia.com/Wendell_Vaughn_(Earth-616)#
    ## 61                 http://marvel.wikia.com/Ashley_Crawford_(Earth-616)#
    ## 62                      http://marvel.wikia.com/Dinah_Soar_(Earth-616)#
    ## 63                    http://marvel.wikia.com/DeMarr_Davis_(Earth-616)#
    ## 64                     http://marvel.wikia.com/Val_Ventura_(Earth-616)#
    ## 65                    http://marvel.wikia.com/Craig_Hollis_(Earth-616)#
    ## 66                    http://marvel.wikia.com/Gene_Lorrene_(Earth-616)#
    ## 67                    http://marvel.wikia.com/Doreen_Green_(Earth-616)#
    ## 68                      http://marvel.wikia.com/Monkey_Joe_(Earth-616)#
    ## 69                    http://marvel.wikia.com/Doug_Taggert_(Earth-616)#
    ## 70                       http://marvel.wikia.com/Tippy-Toe_(Earth-616)#
    ## 71                     http://marvel.wikia.com/Wade_Wilson_(Earth-616)#
    ## 72                     http://marvel.wikia.com/Greg_Willis_(Earth-616)#
    ## 73                           http://marvel.wikia.com/Sersi_(Earth-616)#
    ## 74                    http://marvel.wikia.com/Peter_Parker_(Earth-616)#
    ## 75                   http://marvel.wikia.com/Walter_Newell_(Earth-616)#
    ## 76                   http://marvel.wikia.com/Elvin_Haliday_(Earth-616)#
    ## 77                   http://marvel.wikia.com/William_Baker_(Earth-616)#
    ## 78             http://marvel.wikia.com/Crystalia_Amaquelin_(Earth-616)#
    ## 79                  http://marvel.wikia.com/Eric_Masterson_(Earth-616)#
    ## 80                   http://marvel.wikia.com/Philip_Javert_(Earth-921)#
    ## 81                 http://marvel.wikia.com/Melissa_Darrow_(Earth-9201)#
    ## 82                        http://marvel.wikia.com/Deathcry_(Earth-616)#
    ## 83                 http://marvel.wikia.com/Anthony_Stark_(Earth-96020)#
    ## 84       http://marvel.wikia.com/Giuletta_Nefaria_(Masque)_(Earth-616)#
    ## 85                  http://marvel.wikia.com/Vance_Astrovik_(Earth-616)#
    ## 86                  http://marvel.wikia.com/Angelica_Jones_(Earth-616)#
    ## 87              http://marvel.wikia.com/Delroy_Garrett_Jr._(Earth-616)#
    ## 88     http://marvel.wikia.com/Maria_de_Guadalupe_Santiago_(Earth-616)#
    ## 89                   http://marvel.wikia.com/Jonathan_Hart_(Earth-616)#
    ## 90                    http://marvel.wikia.com/Kelsey_Leigh_(Earth-616)#
    ## 91                       http://marvel.wikia.com/Luke_Cage_(Earth-616)#
    ## 92                         http://marvel.wikia.com/Veranke_(Earth-616)#
    ## 93                   http://marvel.wikia.com/James_Howlett_(Earth-616)#
    ## 94                 http://marvel.wikia.com/Robert_Reynolds_(Earth-616)#
    ## 95                      http://marvel.wikia.com/Maya_Lopez_(Earth-616)#
    ## 96  http://marvel.wikia.com/Nathaniel_Richards_(Iron_Lad)_(Earth-6311)#
    ## 97                  http://marvel.wikia.com/Elijah_Bradley_(Earth-616)#
    ## 98                  http://marvel.wikia.com/William_Kaplan_(Earth-616)#
    ## 99                     http://marvel.wikia.com/Dorrek_VIII_(Earth-616)#
    ## 100                 http://marvel.wikia.com/Cassandra_Lang_(Earth-616)#
    ## 101               http://marvel.wikia.com/Katherine_Bishop_(Earth-616)#
    ## 102                 http://marvel.wikia.com/Vision_(Jonas)_(Earth-616)#
    ## 103                http://marvel.wikia.com/Thomas_Shepherd_(Earth-616)#
    ## 104                    http://marvel.wikia.com/Daniel_Rand_(Earth-616)#
    ## 105                http://marvel.wikia.com/Stephen_Strange_(Earth-616)#
    ## 106                           http://marvel.wikia.com/Ares_(Earth-616)#
    ## 107          http://marvel.wikia.com/James_Buchanan_Barnes_(Earth-616)#
    ## 108                   http://marvel.wikia.com/Jessica_Drew_(Earth-616)#
    ## 109                  http://marvel.wikia.com/Jessica_Jones_(Earth-616)#
    ## 110                    http://marvel.wikia.com/Amadeus_Cho_(Earth-616)#
    ## 111                     http://marvel.wikia.com/Maria_Hill_(Earth-616)#
    ## 112                 http://marvel.wikia.com/Robert_Baldwin_(Earth-616)#
    ## 113                  http://marvel.wikia.com/Sharon_Carter_(Earth-616)#
    ## 114                 http://marvel.wikia.com/Eric_O%27Grady_(Earth-616)#
    ## 115                     http://marvel.wikia.com/Brunnhilde_(Earth-616)#
    ## 116                  http://marvel.wikia.com/Richard_Rider_(Earth-616)#
    ## 117                 http://marvel.wikia.com/Brian_Braddock_(Earth-616)#
    ## 118                   http://marvel.wikia.com/Dennis_Sykes_(Earth-616)#
    ## 119                       http://marvel.wikia.com/Noh-Varr_(Earth-616)#
    ## 120                  http://marvel.wikia.com/Thaddeus_Ross_(Earth-616)#
    ## 121                      http://marvel.wikia.com/John_Aman_(Earth-616)#
    ## 122                      http://marvel.wikia.com/Shang-Chi_(Earth-616)#
    ## 123                  http://marvel.wikia.com/James_Santini_(Earth-616)#
    ## 124                   http://marvel.wikia.com/Emery_Schaub_(Earth-616)#
    ## 125                http://marvel.wikia.com/Jeanne_Foucault_(Earth-616)#
    ## 126                http://marvel.wikia.com/Fiona_(Inhuman)_(Earth-616)#
    ## 127                http://marvel.wikia.com/Jennifer_Takeda_(Earth-616)#
    ## 128                         http://marvel.wikia.com/Hollow_(Earth-616)#
    ## 129                    http://marvel.wikia.com/Julie_Power_(Earth-616)#
    ## 130                     http://marvel.wikia.com/Alani_Ryan_(Earth-616)#
    ## 131                       http://marvel.wikia.com/Ken_Mack_(Earth-616)#
    ## 132                 http://marvel.wikia.com/Humberto_Lopez_(Earth-616)#
    ## 133                http://marvel.wikia.com/Johnathon_Gallo_(Earth-616)#
    ## 134                          http://marvel.wikia.com/Lyra_(Earth-8009)#
    ## 135                   http://marvel.wikia.com/Anya_Corazon_(Earth-616)#
    ## 136                 http://marvel.wikia.com/Brandon_Sharpe_(Earth-616)#
    ## 137                http://marvel.wikia.com/Kevin_Masterson_(Earth-616)#
    ## 138                http://marvel.wikia.com/Michiko_Musashi_(Earth-616)#
    ## 139                http://marvel.wikia.com/Takashi_Matsuya_(Earth-616)#
    ## 140                  http://marvel.wikia.com/Daisy_Johnson_(Earth-616)#
    ## 141                   http://marvel.wikia.com/Ororo_Munroe_(Earth-616)#
    ## 142                http://marvel.wikia.com/Matthew_Murdock_(Earth-616)#
    ## 143                http://marvel.wikia.com/Eugene_Thompson_(Earth-616)#
    ## 144                  http://marvel.wikia.com/Otto_Octavius_(Earth-616)#
    ## 145              http://marvel.wikia.com/Alexander_Summers_(Earth-616)#
    ## 146                 http://marvel.wikia.com/Samuel_Guthrie_(Earth-616)#
    ## 147               http://marvel.wikia.com/Roberto_da_Costa_(Earth-616)#
    ## 148                      http://marvel.wikia.com/Eden_Fesi_(Earth-616)#
    ## 149               http://marvel.wikia.com/Captain_Universe_(Earth-616)#
    ## 150                    http://marvel.wikia.com/Isabel_Kane_(Earth-616)#
    ## 151                http://marvel.wikia.com/Marcus_Milton_(Earth-13034)#
    ## 152             http://marvel.wikia.com/Rogue_(Anna_Marie)_(Earth-616)#
    ## 153                  http://marvel.wikia.com/Shiro_Yoshida_(Earth-616)#
    ## 154                      http://marvel.wikia.com/Ex_Nihilo_(Earth-616)#
    ## 155          http://marvel.wikia.com/Abyss_(Ex_Nihilo%27s)_(Earth-616)#
    ## 156                      http://marvel.wikia.com/Nightmask_(Earth-616)#
    ## 157                   http://marvel.wikia.com/Kevin_Connor_(Earth-616)#
    ## 158                  http://marvel.wikia.com/Sam_Alexander_(Earth-616)#
    ## 159                 http://marvel.wikia.com/America_Chavez_(Earth-616)#
    ## 160          http://marvel.wikia.com/Loki_Laufeyson_(Ikol)_(Earth-616)#
    ## 161                  http://marvel.wikia.com/David_Alleyne_(Earth-616)#
    ## 162             http://marvel.wikia.com/Nicholas_Fury,_Jr._(Earth-616)#
    ## 163                http://marvel.wikia.com/Phillip_Coulson_(Earth-616)#
    ## 164                   http://marvel.wikia.com/Tony_Masters_(Earth-616)#
    ## 165                  http://marvel.wikia.com/Victor_Mancha_(Earth-616)#
    ## 166                   http://marvel.wikia.com/Monica_Chang_(Earth-616)#
    ## 167              http://marvel.wikia.com/Doombot_(Avenger)_(Earth-616)#
    ## 168                         http://marvel.wikia.com/Alexis_(Earth-616)#
    ## 169                    http://marvel.wikia.com/Eric_Brooks_(Earth-616)#
    ## 170                  http://marvel.wikia.com/Adam_Brashear_(Earth-616)#
    ## 171                 http://marvel.wikia.com/Victor_Alvarez_(Earth-616)#
    ## 172                      http://marvel.wikia.com/Ava_Ayala_(Earth-616)#
    ## 173                          http://marvel.wikia.com/Kaluu_(Earth-616)#
    ##                              Name.Alias Appearances Current. Gender
    ## 1             Henry Jonathan "Hank" Pym        1269      YES   MALE
    ## 2                        Janet van Dyne        1165      YES FEMALE
    ## 3           Anthony Edward "Tony" Stark        3068      YES   MALE
    ## 4                   Robert Bruce Banner        2089      YES   MALE
    ## 5                          Thor Odinson        2402      YES   MALE
    ## 6                Richard Milhouse Jones         612      YES   MALE
    ## 7                         Steven Rogers        3458      YES   MALE
    ## 8                Clinton Francis Barton        1456      YES   MALE
    ## 9                       Pietro Maximoff         769      YES   MALE
    ## 10                       Wanda Maximoff        1214      YES FEMALE
    ## 11                     Jacques Duquesne         115       NO   MALE
    ## 12                             Heracles         741      YES   MALE
    ## 13                             T'Challa         780       NO   MALE
    ## 14                 Victor Shade (alias)        1036      YES   MALE
    ## 15                         Dane Whitman         482       NO   MALE
    ## 16           Natalia Alianovna Romanova        1112      YES FEMALE
    ## 17                               Brandt         160       NO FEMALE
    ## 18                       Henry P. McCoy        1886       NO   MALE
    ## 19                      Heather Douglas         332       NO FEMALE
    ## 20                         Patsy Walker         557       NO FEMALE
    ## 21       Matthew Liebowitz (birth name)         197       NO   MALE
    ## 22                  Robert L. Frank Sr.         106       NO   MALE
    ## 23                       Simon Williams         692      YES   MALE
    ## 24                         Yondu Udonta         109       NO   MALE
    ## 25                      Martinex T'Naga         100       NO   MALE
    ## 26                           Charlie-27         132       NO   MALE
    ## 27                      Nicholette Gold         108       NO FEMALE
    ## 28                               Stakar         100       NO   MALE
    ## 29                       Vance Astrovik         156       NO   MALE
    ## 30                             Mar-Vell         254       NO   MALE
    ## 31             Carol Susan Jane Danvers         935      YES FEMALE
    ## 32                 Samuel Thomas Wilson         576      YES   MALE
    ## 33                              Jocasta         141      YES FEMALE
    ## 34                   Greer Grant Nelson         355      YES FEMALE
    ## 35                     Jennifer Walters         933      YES FEMALE
    ## 36                       Monica Rambeau         348      YES FEMALE
    ## 37                                 Eros         206       NO   MALE
    ## 38                      James R. Rhodes         533       NO   MALE
    ## 39           Barbara Barton (nee Morse)         374       NO FEMALE
    ## 40                        Moira Brandon           2       NO FEMALE
    ## 41                 Benjamin Jacob Grimm        2305       NO   MALE
    ## 42                        Bonita Juarez          83       NO FEMALE
    ## 43                         Marc Spector         402       NO   MALE
    ## 44                       John F. Walker         352       NO   MALE
    ## 45                  Jim Hammond (alias)         565       NO   MALE
    ## 46                        Miguel Santos         112       NO   MALE
    ## 47                      Julia Carpenter         218       NO FEMALE
    ## 48                                 X-51         149       NO   MALE
    ## 49                   Christopher Powell         168       NO   MALE
    ## 50                       Namor McKenzie        1561       NO   MALE
    ## 51             Scott Edward Harris Lang         217       NO   MALE
    ## 52                Anthony Ludgate Druid         158       NO   MALE
    ## 53                    Marrina Smallwood          86       NO FEMALE
    ## 54              Ravonna Lexus Renslayer          41       NO FEMALE
    ## 55                          Rita DeMara          68       NO FEMALE
    ## 56                        Dennis Dunphy          70       NO   MALE
    ## 57                                               61       NO   MALE
    ## 58                        Reed Richards        2125      YES   MALE
    ## 59           Susan Richards (nee Storm)        1761       NO   MALE
    ## 60                 Wendell Elvis Vaughn         293       NO   MALE
    ## 61                      Ashley Crawford          36      YES FEMALE
    ## 62                                               22       NO FEMALE
    ## 63                         DeMarr Davis          31      YES   MALE
    ## 64                          Val Ventura          34      YES   MALE
    ## 65                         Craig Hollis          33      YES   MALE
    ## 66                         Gene Lorrene           4       NO   MALE
    ## 67                        Dorreen Green          47       NO FEMALE
    ## 68                                                7       NO   MALE
    ## 69                         Doug Taggert           3       NO   MALE
    ## 70                                               16       NO FEMALE
    ## 71                          Wade Wilson         575       NO   MALE
    ## 72                          Greg Willis          58       NO   MALE
    ## 73                                Circe         237       NO FEMALE
    ## 74                Peter Benjamin Parker        4333      YES   MALE
    ## 75                        Walter Newell         126       NO   MALE
    ## 76                        Elvin Haliday         158       NO   MALE
    ## 77                        William Baker         355       NO   MALE
    ## 78           Crystal Amaquelin Maximoff         517       NO FEMALE
    ## 79                 Eric Kevin Masterson         202       NO   MALE
    ## 80                       Phillip Javert          31       NO   MALE
    ## 81                                               28       NO FEMALE
    ## 82                                               50       NO FEMALE
    ## 83                 Anthony Edward Stark          27       NO   MALE
    ## 84                  "Giulietta Nefaria"          18       NO FEMALE
    ## 85                       Vance Astrovik         302       NO   MALE
    ## 86                       Angelica Jones         330       NO FEMALE
    ## 87                   Delroy Garrett Jr.         101       NO   MALE
    ## 88          Maria de Guadalupe Santiago          43       NO FEMALE
    ## 89                        Jonathan Hart         126       NO   MALE
    ## 90                   Kelsey Leigh Shorr          24       NO FEMALE
    ## 91                           Carl Lucas         886      YES   MALE
    ## 92                              Veranke         159       NO FEMALE
    ## 93                James "Logan" Howlett        3130      YES   MALE
    ## 94                      Robert Reynolds         241       NO   MALE
    ## 95                           Maya Lopez          67       NO FEMALE
    ## 96                   Nathaniel Richards          23       NO   MALE
    ## 97                       Elijah Bradley         103       NO   MALE
    ## 98               William "Billy" Kaplan         123      YES   MALE
    ## 99  Dorrek VIII/Theodore "Teddy" Altman         110      YES   MALE
    ## 100                         Cassie Lang         160       NO FEMALE
    ## 101             Katherine "Kate" Bishop         132      YES FEMALE
    ## 102                        Alias: Jonas         121       NO   MALE
    ## 103             Thomas "Tommy" Shepherd          59      YES   MALE
    ## 104             Daniel Thomas Rand K'ai         629       NO   MALE
    ## 105      Doctor Stephen Vincent Strange        1324       NO   MALE
    ## 106                                Ares         236       NO   MALE
    ## 107               James Buchanan Barnes         663       NO   MALE
    ## 108                 Jessica Miriam Drew         525      YES FEMALE
    ## 109                       Jessica Jones         205      YES FEMALE
    ## 110                         Amadeus Cho         108       NO   MALE
    ## 111                          Maria Hill         359      YES FEMALE
    ## 112                      Robbie Baldwin         299       NO   MALE
    ## 113                       Sharon Carter         333       NO FEMALE
    ## 114                        Eric O'Grady          88       NO   MALE
    ## 115                          Brunnhilde         369       NO FEMALE
    ## 116                       Richard Rider         380       NO   MALE
    ## 117                      Brian Braddock         545       NO   MALE
    ## 118                        Dennis Sykes           6       NO   MALE
    ## 119                            Noh-Varr         126      YES   MALE
    ## 120                       Thaddeus Ross         417       NO   MALE
    ## 121                           John Aman          31      YES   MALE
    ## 122                           Shang-Chi         310      YES   MALE
    ## 123                       James Santini          40      YES   MALE
    ## 124                        Emery Schaub          26      YES   MALE
    ## 125                     Jeanne Foucault          63      YES FEMALE
    ## 126                               Fiona           2      YES FEMALE
    ## 127                     Jennifer Takeda          73      YES FEMALE
    ## 128                              Yvette          22      YES FEMALE
    ## 129                         Julie Power         153      YES FEMALE
    ## 130                          Alani Ryan          73      YES FEMALE
    ## 131                            Ken Mack          59       NO   MALE
    ## 132                      Humberto Lopez          66      YES   MALE
    ## 133                        Johnny Gallo          43      YES   MALE
    ## 134                                Lyra          55      YES FEMALE
    ## 135                        Anya Corazon         108      YES FEMALE
    ## 136                      Brandon Sharpe          64      YES   MALE
    ## 137                     Kevin Masterson          62      YES   MALE
    ## 138                     Michiko Musashi          94      YES FEMALE
    ## 139                        Taki Matsuya          18      YES   MALE
    ## 140                       Daisy Johnson          81       NO FEMALE
    ## 141                        Ororo Munroe        1598       NO FEMALE
    ## 142                        Matt Murdock        1375       NO   MALE
    ## 143                      Flash Thompson         746      YES   MALE
    ## 144                       Otto Octavius         561       NO   MALE
    ## 145                        Alex Summers         592      YES   MALE
    ## 146                      Samuel Guthrie         679      YES   MALE
    ## 147                    Roberto da Costa         491      YES   MALE
    ## 148                           Eden Fesi          65      YES   MALE
    ## 149                                              55      YES   MALE
    ## 150                           Izzy Kane          44      YES FEMALE
    ## 151                       Marcus Milton          65      YES   MALE
    ## 152                          Anna Marie         877      YES FEMALE
    ## 153                       Shiro Yoshida         176      YES   MALE
    ## 154                                              24      YES   MALE
    ## 155                                              25      YES FEMALE
    ## 156                                Adam          35      YES   MALE
    ## 157                   Kevin Kale Connor          44      YES   MALE
    ## 158                       Sam Alexander          78      YES   MALE
    ## 159                      America Chavez          22      YES FEMALE
    ## 160                      Loki Laufeyson          77       NO   MALE
    ## 161                       David Alleyne         115      YES   MALE
    ## 162  Nicholas Fury, Jr., Marcus Johnson          77      YES   MALE
    ## 163                     Phillip Coulson          69      YES   MALE
    ## 164                        Tony Masters         173       NO   MALE
    ## 165                       Victor Mancha          75      YES   MALE
    ## 166                        Monica Chang          12      YES FEMALE
    ## 167                                              14      YES   MALE
    ## 168                              Alexis          13      YES FEMALE
    ## 169                         Eric Brooks         198      YES   MALE
    ## 170                       Adam Brashear          29      YES   MALE
    ## 171                      Victor Alvarez          45      YES   MALE
    ## 172                           Ava Ayala          49      YES FEMALE
    ## 173                               Kaluu          35      YES   MALE
    ##     Probationary.Introl Full.Reserve.Avengers.Intro Year Years.since.joining
    ## 1                                            Sep-63 1963                  52
    ## 2                                            Sep-63 1963                  52
    ## 3                                            Sep-63 1963                  52
    ## 4                                            Sep-63 1963                  52
    ## 5                                            Sep-63 1963                  52
    ## 6                                            Sep-63 1963                  52
    ## 7                                            Mar-64 1964                  51
    ## 8                                            May-65 1965                  50
    ## 9                                            May-65 1965                  50
    ## 10                                           May-65 1965                  50
    ## 11                                           Sep-65 1965                  50
    ## 12                                           Oct-67 1967                  48
    ## 13                                           May-68 1968                  47
    ## 14                                           Nov-68 1968                  47
    ## 15                                           Dec-69 1969                  46
    ## 16                                           May-73 1973                  42
    ## 17                                           Aug-73 1973                  42
    ## 18               Jul-75                      Sep-76 1976                  39
    ## 19               Jul-75                      Sep-76 1976                  39
    ## 20               Nov-75                      Sep-76 1976                  39
    ## 21                                           Aug-75 1975                  40
    ## 22               Apr-77                      Jul-78 1978                  37
    ## 23               Apr-77                      Mar-79 1979                  36
    ## 24                                           Feb-78 1978                  37
    ## 25                                           Feb-78 1978                  37
    ## 26                                           Feb-78 1978                  37
    ## 27                                           Feb-78 1978                  37
    ## 28                                           Feb-78 1978                  37
    ## 29                                           Feb-78 1978                  37
    ## 30                                           Jul-78 1978                  37
    ## 31                                           Apr-79 1979                  36
    ## 32                                           Jun-79 1979                  36
    ## 33               Jul-80                      Nov-88 1988                  27
    ## 34                                           Sep-81 1981                  34
    ## 35                                           Jul-82 1982                  33
    ## 36               Jan-83                      May-83 1983                  32
    ## 37               Jun-83                      May-84 1984                  31
    ## 38               May-84                      Sep-84 1984                  31
    ## 39               Jun-84                      Sep-84 1984                  31
    ## 40                                           Nov-93 1993                  22
    ## 41                                           Jun-86 1986                  29
    ## 42                                           Sep-87 1987                  28
    ## 43               Sep-87                      Jun-88 1988                  27
    ## 44                                           May-89 1989                  26
    ## 45                                           Nov-89 1989                  26
    ## 46                                           Apr-91 1991                  24
    ## 47                                           Sep-92 1992                  23
    ## 48                                           Jun-92 1992                  23
    ## 49                                           May-93 1993                  22
    ## 50                                           Dec-85 1985                  30
    ## 51               Jan-87                       3-Feb 2003                  12
    ## 52                                           Apr-87 1987                  28
    ## 53                                           Dec-87 1987                  28
    ## 54                                           May-88 1988                  27
    ## 55                                           Nov-88 1988                  27
    ## 56                                           Jan-88 1988                  27
    ## 57                                           Feb-89 1989                  26
    ## 58                                           Feb-89 1989                  26
    ## 59                                           Feb-89 1989                  26
    ## 60                                           Oct-89 1989                  26
    ## 61                                           Jul-89 1989                  26
    ## 62                                           Jul-89 1989                  26
    ## 63                                           Jul-89 1989                  26
    ## 64                                           Jul-89 1989                  26
    ## 65                                           Jul-89 1989                  26
    ## 66                                            5-Apr 2005                  10
    ## 67                                            5-May 2005                  10
    ## 68                                            5-May 2005                  10
    ## 69                                            5-May 2005                  10
    ## 70                                            5-Jul 2005                  10
    ## 71                                            7-Sep 2007                   8
    ## 72                                            9-Aug 2009                   6
    ## 73                                           Feb-90 1990                  25
    ## 74                                           Apr-90 1990                  25
    ## 75                                           Jul-90 1990                  25
    ## 76               Feb-91                             1900                 115
    ## 77               Feb-91                             1900                 115
    ## 78               Aug-91                      Jan-92 1992                  23
    ## 79                                           Jan-92 1992                  23
    ## 80                                           Dec-92 1992                  23
    ## 81                                           Jun-93 1993                  22
    ## 82                                           Jul-93 1993                  22
    ## 83                                           Feb-96 1996                  19
    ## 84                                           Apr-96 1996                  19
    ## 85                                           May-98 1998                  17
    ## 86                                           May-98 1998                  17
    ## 87                                           Apr-00 2000                  15
    ## 88                                           Jul-00 2000                  15
    ## 89                                            1-Aug 2001                  14
    ## 90                                            4-Jun 2004                  11
    ## 91                                            5-Mar 2005                  10
    ## 92                                            5-Mar 2005                  10
    ## 93                                            5-Jun 2005                  10
    ## 94                                            5-Oct 2005                  10
    ## 95                                            5-Nov 2005                  10
    ## 96                                            5-Apr 2005                  10
    ## 97                                            5-Apr 2005                  10
    ## 98                                            5-Apr 2005                  10
    ## 99                                            5-Apr 2005                  10
    ## 100                                           5-Jun 2005                  10
    ## 101                                           5-Jun 2005                  10
    ## 102                                           6-Feb 2006                   9
    ## 103                                           6-Jun 2006                   9
    ## 104                                           7-Feb 2007                   8
    ## 105                                           7-Feb 2007                   8
    ## 106                                           7-Mar 2007                   8
    ## 107                                           8-Dec 2008                   7
    ## 108                                           8-Dec 2008                   7
    ## 109                                          10-Aug 2010                   5
    ## 110                                           9-Dec 2009                   6
    ## 111                                          10-May 2010                   5
    ## 112                                          10-Jun 2010                   5
    ## 113                                          10-May 2010                   5
    ## 114                                          10-May 2010                   5
    ## 115                                          10-May 2010                   5
    ## 116                                          10-May 2010                   5
    ## 117                                          10-May 2010                   5
    ## 118                                          10-Sep 2010                   5
    ## 119                                          10-Jun 2010                   5
    ## 120                                          11-Jun 2011                   4
    ## 121                                          10-Dec 2010                   5
    ## 122                                          11-Apr 2011                   4
    ## 123                                                 1900                 115
    ## 124                                                 1900                 115
    ## 125                                          10-Aug 2010                   5
    ## 126                                                 1900                 115
    ## 127                                          10-Aug 2010                   5
    ## 128                                                 1900                 115
    ## 129                                                 1900                 115
    ## 130                                                 1900                 115
    ## 131                                          10-Aug 2010                   5
    ## 132                                          10-Aug 2010                   5
    ## 133                                                 1900                 115
    ## 134                                                 1900                 115
    ## 135                                                 1900                 115
    ## 136                                          10-Aug 2010                   5
    ## 137                                                 1900                 115
    ## 138                                                 1900                 115
    ## 139                                                 1900                 115
    ## 140                                          12-Jan 2012                   3
    ## 141                                          12-Jan 2012                   3
    ## 142                                          11-Nov 2011                   4
    ## 143                                          12-Apr 2012                   3
    ## 144                                          13-Jan 2013                   2
    ## 145                                          12-Dec 2012                   3
    ## 146                                          13-Feb 2013                   2
    ## 147                                          13-Feb 2013                   2
    ## 148                                          13-Feb 2013                   2
    ## 149                                          13-Feb 2013                   2
    ## 150                                          13-Feb 2013                   2
    ## 151                                          13-Feb 2013                   2
    ## 152                                          13-Apr 2013                   2
    ## 153                                          13-May 2013                   2
    ## 154                                          13-Oct 2013                   2
    ## 155                                          13-Oct 2013                   2
    ## 156                                          13-Oct 2013                   2
    ## 157                                          13-Oct 2013                   2
    ## 158                                          15-Feb 2015                   0
    ## 159                                          13-Jul 2013                   2
    ## 160                                          13-Jul 2013                   2
    ## 161                                          13-Sep 2013                   2
    ## 162                                          13-Apr 2013                   2
    ## 163                                          13-Apr 2013                   2
    ## 164                                          13-May 2013                   2
    ## 165                                          13-Sep 2013                   2
    ## 166                                          13-Sep 2013                   2
    ## 167                                          13-Sep 2013                   2
    ## 168                                          13-Nov 2013                   2
    ## 169                                          13-Nov 2013                   2
    ## 170                                          14-Jan 2014                   1
    ## 171                                          14-Jan 2014                   1
    ## 172                                          14-Jan 2014                   1
    ## 173                                          15-Jan 2015                   0
    ##         Honorary Death1 Return1 Death2 Return2 Death3 Return3 Death4 Return4
    ## 1           Full    YES      NO                                             
    ## 2           Full    YES     YES                                             
    ## 3           Full    YES     YES                                             
    ## 4           Full    YES     YES                                             
    ## 5           Full    YES     YES    YES      NO                              
    ## 6       Honorary     NO                                                     
    ## 7           Full    YES     YES                                             
    ## 8           Full    YES     YES    YES     YES                              
    ## 9           Full    YES     YES                                             
    ## 10          Full    YES     YES                                             
    ## 11          Full    YES     YES                                             
    ## 12          Full     NO                                                     
    ## 13          Full     NO                                                     
    ## 14          Full    YES     YES                                             
    ## 15          Full     NO                                                     
    ## 16          Full    YES     YES                                             
    ## 17          Full    YES     YES                                             
    ## 18          Full     NO                                                     
    ## 19          Full    YES     YES    YES     YES                              
    ## 20          Full    YES     YES                                             
    ## 21          Full    YES     YES    YES      NO                              
    ## 22          Full    YES      NO                                             
    ## 23          Full    YES     YES                                             
    ## 24      Honorary     NO                                                     
    ## 25      Honorary     NO                                                     
    ## 26      Honorary     NO                                                     
    ## 27      Honorary     NO                                                     
    ## 28      Honorary     NO                                                     
    ## 29      Honorary     NO                                                     
    ## 30          Full    YES     YES    YES     YES    YES      NO               
    ## 31          Full     NO                                                     
    ## 32          Full     NO                                                     
    ## 33          Full    YES     YES    YES     YES    YES     YES    YES     YES
    ## 34          Full     NO                                                     
    ## 35          Full    YES     YES                                             
    ## 36          Full     NO                                                     
    ## 37          Full     NO                                                     
    ## 38          Full     NO                                                     
    ## 39          Full    YES     YES                                             
    ## 40      Honorary    YES      NO                                             
    ## 41          Full    YES     YES                                             
    ## 42          Full     NO                                                     
    ## 43          Full     NO                                                     
    ## 44          Full     NO                                                     
    ## 45          Full    YES     YES                                             
    ## 46          Full     NO                                                     
    ## 47          Full     NO                                                     
    ## 48          Full    YES     YES                                             
    ## 49          Full     NO                                                     
    ## 50          Full    YES     YES                                             
    ## 51          Full    YES     YES                                             
    ## 52      Honorary    YES     YES    YES     YES                              
    ## 53          Full    YES     YES    YES     YES                              
    ## 54          Full    YES     YES    YES      NO                              
    ## 55      Honorary    YES     YES                                             
    ## 56          Full    YES     YES    YES      NO                              
    ## 57          Full    YES     YES                                             
    ## 58          Full     NO                                                     
    ## 59          Full     NO                                                     
    ## 60          Full    YES     YES                                             
    ## 61          Full     NO                                                     
    ## 62          Full    YES      NO                                             
    ## 63          Full    YES     YES                                             
    ## 64          Full     NO                                                     
    ## 65          Full     NO                                                     
    ## 66          Full     NO                                                     
    ## 67          Full     NO                                                     
    ## 68          Full    YES      NO                                             
    ## 69          Full    YES      NO                                             
    ## 70          Full     NO                                                     
    ## 71          Full    YES      NO                                             
    ## 72          Full     NO                                                     
    ## 73          Full     NO                                                     
    ## 74          Full    YES     YES    YES     YES                              
    ## 75          Full     NO                                                     
    ## 76  Probationary     NO                                                     
    ## 77  Probationary    YES     YES     NO                                      
    ## 78          Full     NO                                                     
    ## 79          Full    YES     YES    YES      NO                              
    ## 80      Honorary     NO                                                     
    ## 81      Honorary     NO                                                     
    ## 82      Honorary    YES     YES    YES      NO                              
    ## 83          Full    YES      NO                                             
    ## 84      Honorary    YES      NO                                             
    ## 85          Full     NO                                                     
    ## 86          Full     NO                                                     
    ## 87          Full     NO                                                     
    ## 88          Full     NO                                                     
    ## 89          Full    YES     YES    YES     YES                              
    ## 90          Full     NO                                                     
    ## 91          Full     NO                                                     
    ## 92          Full    YES      NO                                             
    ## 93          Full    YES      NO                                             
    ## 94          Full    YES     YES                                             
    ## 95          Full    YES     YES    YES      NO                              
    ## 96          Full     NO                                                     
    ## 97          Full     NO                                                     
    ## 98          Full     NO                                                     
    ## 99          Full     NO                                                     
    ## 100         Full    YES     YES                                             
    ## 101         Full     NO                                                     
    ## 102         Full    YES      NO                                             
    ## 103         Full     NO                                                     
    ## 104         Full     NO                                                     
    ## 105         Full     NO                                                     
    ## 106         Full    YES     YES    YES      NO                              
    ## 107         Full     NO                                                     
    ## 108         Full     NO                                                     
    ## 109         Full     NO                                                     
    ## 110         Full     NO                                                     
    ## 111         Full     NO                                                     
    ## 112         Full     NO                                                     
    ## 113         Full    YES     YES                                             
    ## 114         Full    YES      NO                                             
    ## 115         Full     NO                                                     
    ## 116         Full    YES      NO                                             
    ## 117         Full     NO                                                     
    ## 118         Full    YES      NO                                             
    ## 119         Full     NO                                                     
    ## 120         Full    YES     YES                                             
    ## 121         Full     NO                                                     
    ## 122         Full     NO                                                     
    ## 123      Academy     NO                                                     
    ## 124      Academy     NO                                                     
    ## 125      Academy     NO                                                     
    ## 126      Academy     NO                                                     
    ## 127      Academy     NO                                                     
    ## 128      Academy     NO                                                     
    ## 129      Academy     NO                                                     
    ## 130      Academy     NO                                                     
    ## 131      Academy    YES      NO                                             
    ## 132      Academy    YES      NO                                             
    ## 133      Academy     NO                                                     
    ## 134      Academy     NO                                                     
    ## 135      Academy     NO                                                     
    ## 136      Academy     NO                                                     
    ## 137      Academy     NO                                                     
    ## 138      Academy     NO                                                     
    ## 139      Academy     NO                                                     
    ## 140         Full     NO                                                     
    ## 141         Full     NO                                                     
    ## 142         Full     NO                                                     
    ## 143     Honorary     NO                                                     
    ## 144         Full    YES      NO                                             
    ## 145         Full     NO                                                     
    ## 146         Full     NO                                                     
    ## 147         Full     NO                                                     
    ## 148         Full     NO                                                     
    ## 149         Full     NO                                                     
    ## 150         Full     NO                                                     
    ## 151         Full    YES      NO                                             
    ## 152         Full     NO                                                     
    ## 153         Full    YES     YES                                             
    ## 154         Full    YES      NO                                             
    ## 155         Full    YES      NO                                             
    ## 156         Full    YES      NO                                             
    ## 157         Full    YES      NO                                             
    ## 158     Honorary     NO                                                     
    ## 159         Full     NO                                                     
    ## 160         Full     NO                                                     
    ## 161         Full     NO                                                     
    ## 162         Full     NO                                                     
    ## 163         Full     NO                                                     
    ## 164         Full     NO                                                     
    ## 165         Full    YES     YES                                             
    ## 166         Full     NO                                                     
    ## 167         Full     NO                                                     
    ## 168         Full     NO                                                     
    ## 169         Full     NO                                                     
    ## 170         Full     NO                                                     
    ## 171         Full     NO                                                     
    ## 172         Full     NO                                                     
    ## 173         Full     NO                                                     
    ##     Death5 Return5
    ## 1                 
    ## 2                 
    ## 3                 
    ## 4                 
    ## 5                 
    ## 6                 
    ## 7                 
    ## 8                 
    ## 9                 
    ## 10                
    ## 11                
    ## 12                
    ## 13                
    ## 14                
    ## 15                
    ## 16                
    ## 17                
    ## 18                
    ## 19                
    ## 20                
    ## 21                
    ## 22                
    ## 23                
    ## 24                
    ## 25                
    ## 26                
    ## 27                
    ## 28                
    ## 29                
    ## 30                
    ## 31                
    ## 32                
    ## 33     YES     YES
    ## 34                
    ## 35                
    ## 36                
    ## 37                
    ## 38                
    ## 39                
    ## 40                
    ## 41                
    ## 42                
    ## 43                
    ## 44                
    ## 45                
    ## 46                
    ## 47                
    ## 48                
    ## 49                
    ## 50                
    ## 51                
    ## 52                
    ## 53                
    ## 54                
    ## 55                
    ## 56                
    ## 57                
    ## 58                
    ## 59                
    ## 60                
    ## 61                
    ## 62                
    ## 63                
    ## 64                
    ## 65                
    ## 66                
    ## 67                
    ## 68                
    ## 69                
    ## 70                
    ## 71                
    ## 72                
    ## 73                
    ## 74                
    ## 75                
    ## 76                
    ## 77                
    ## 78                
    ## 79                
    ## 80                
    ## 81                
    ## 82                
    ## 83                
    ## 84                
    ## 85                
    ## 86                
    ## 87                
    ## 88                
    ## 89                
    ## 90                
    ## 91                
    ## 92                
    ## 93                
    ## 94                
    ## 95                
    ## 96                
    ## 97                
    ## 98                
    ## 99                
    ## 100               
    ## 101               
    ## 102               
    ## 103               
    ## 104               
    ## 105               
    ## 106               
    ## 107               
    ## 108               
    ## 109               
    ## 110               
    ## 111               
    ## 112               
    ## 113               
    ## 114               
    ## 115               
    ## 116               
    ## 117               
    ## 118               
    ## 119               
    ## 120               
    ## 121               
    ## 122               
    ## 123               
    ## 124               
    ## 125               
    ## 126               
    ## 127               
    ## 128               
    ## 129               
    ## 130               
    ## 131               
    ## 132               
    ## 133               
    ## 134               
    ## 135               
    ## 136               
    ## 137               
    ## 138               
    ## 139               
    ## 140               
    ## 141               
    ## 142               
    ## 143               
    ## 144               
    ## 145               
    ## 146               
    ## 147               
    ## 148               
    ## 149               
    ## 150               
    ## 151               
    ## 152               
    ## 153               
    ## 154               
    ## 155               
    ## 156               
    ## 157               
    ## 158               
    ## 159               
    ## 160               
    ## 161               
    ## 162               
    ## 163               
    ## 164               
    ## 165               
    ## 166               
    ## 167               
    ## 168               
    ## 169               
    ## 170               
    ## 171               
    ## 172               
    ## 173               
    ##                                                                                                                                                                                                                                                                  Notes
    ## 1                                                                                                                                                                                                    Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                                                                                                      Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3                                                                                     Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                                                                                                                   Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                                                                                                          Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                                                                                                                 <NA>
    ## 7                                                                                                                                                                                                                     Dies at the end of Civil War. Later comes back. 
    ## 8                                                                                                           Dies in exploding Kree ship in Averngers Vol. 1  Issue 502. Brought back by Scarlet Witch. Dies again in House of M Vol 1 Issue 7. Is later brought back. 
    ## 9                                                                                                                                                                                                                   Dies in House of M Vol 1 Issue 7. Later comes back
    ## 10                                                                                                                                                                                                                 Dies in Uncanny_Avengers_Vol_1_14. Later comes back
    ## 11                                                                                                                                                                                                          Dies in Avengers_Vol_1_130. Brought back by the Chaos King
    ## 12                                                                                                                                                                                                                                                                <NA>
    ## 13                                                                                                                                                                                                                                                                <NA>
    ## 14                                                                                                                                                                                                                 Dies in Avengers_Vol_1_500. Is eventually rebuilt. 
    ## 15                                                                                                                                                                                                                                                                <NA>
    ## 16                                                                                                                                                                                                    Killed by The Hand. Later revived with The Stone of the Chaste. 
    ## 17                                                                                                                                                          Dies in Silver_Surfer_Vol_3_3. Actually "fragments of her essence were spread out all across the universe"
    ## 18                                                                                                                                                                                                                                                                <NA>
    ## 19                                                                                        Dies in Defenders_Vol_1_152. Later 'Obtained a new body.' Dies in Annihilation:_Conquest_Vol_1_3. Comes back as "As before" also was once named Madame MacEvil because LOL. 
    ## 20                                                                                                                                         Died during time on Defenders. Eventually ressurected by villian Grim Reaper along with Mockingbird to fight the Avengers. 
    ## 21                                                                                                        Died during Return_to_the_Old_West. Resurrected by She Hulk in negotiation with Time Variance Authority. As a time traveller he died in 1938. Did not return
    ## 22                                                                                                                                Died in Vision_and_the_Scarlet_Witch_Vol_1_2. Did not return however he was eventually cloned but the clone was killed immediately. 
    ## 23                                                                                                                                                                                                      Died in Avengers_Vol_1_9. Actually just in a death-like coma. 
    ## 24                                                                                                                                                                                                                                                                <NA>
    ## 25                                                                                                                                                                                                                                                                <NA>
    ## 26                                                                                                                                                                                                                                                                <NA>
    ## 27                                                                                                                                                                                                                                                                <NA>
    ## 28                                                                                                                                                                                                                                                                <NA>
    ## 29                                                                                                                                                                                                                                                                <NA>
    ## 30  The bad penny of the Marvel universe. First killed in Secret invasion. Revived during the Chaos war. Died again during aformentioned chaos war. Resurrected by the Kree Empire using the\xe6M'Kraan Crystal. Third death during Avengers vs. X-Men. Has not return
    ## 31                                                                                                                                                                                                                                                                <NA>
    ## 32                                                                                                                                                                                                                                                                <NA>
    ## 33     From her article: Death1: "Defeated Ultron and reversed the process leaving Jocasta a mindless husk." Return 1: "Ultron later revived Jocasta with a remote link activating the mental residue the Wasp left behind" Death 2: "Sacrificing herself to try to ki
    ## 34                                                                                                                                                                                                                                                                <NA>
    ## 35                                                                                                                                                                      Dies during Red Hulk Saga. Returns when Lyra infiltrates Intelligenica and finds her in statis
    ## 36                                                                                                                                                                                                                                                                <NA>
    ## 37                                                                                                                                                                                                                                                                <NA>
    ## 38                                                                                                                                                                                                                                                                <NA>
    ## 39                                                                                                                      Killed by Mephisto. After Secret Invasion it turns out she was actually being impersonated by a Skrull the whole time and was alive and well. 
    ## 40                                                                                                                                                                                 Died in her second appearance earns honorary Avengers status doing so. Stays dead. 
    ## 41                                                                                                                                               Once killed during a battle with Doctor Doom.' Brought back by the FF when they literally went to Heaven to get him. 
    ## 42                                                                                                                                                                                                                                                                <NA>
    ## 43                                                                                                                                                                                   NA but he he did die and return to get his powers prior to joining the Avengers. 
    ## 44                                                                                                                                                                                                                                                                <NA>
    ## 45                                                                                                    Dies during New Invaders. Was "was revived as a living weapon the Thinker intended to sell." Prior to joining the avengers he had died like 2 times in the 40s. 
    ## 46                                                                                                     NA: Has not died since joing the Avengers but prior to joining he fought the West Coast avengers and seemingly died. Later turned up under control of a villain
    ## 47                                                                                                                                                                                                                                                                <NA>
    ## 48                                                                                                                                                                                             Died in Deathlok Vol 2 #5. JK that was actually just a Life Model Decoy
    ## 49                                                                                                                                                                                                                                                                <NA>
    ## 50                                                                                                                                         Namor was presumed killed in the battle with Atlantean Barbarians. Had actually survived and turned up in the South Pacific
    ## 51                                                                                      Died in Avengers:_Disassembled. Was brought back in the Young Avengers: Children's Crusade arc because time travel but take my word for it it was actually kind of well done. 
    ## 52                                                                Shot with a Breathing gun and corpse dumped in trash. Revived by the Grim Reaper. Helped Avengers defeat the Reaper so his spirit could remain at peace. Later completely revived by the Chaos King.
    ## 53                                                                                Killed by Namor with the Ebony Blade. Returned when The Master put her in a statis tube. Death 2 was when Namor mercy killed her. Returned to life a second time by the Chaos King. 
    ## 54                                                                                                                                                                         Killed in Avengers_Vol_1_24. Revived by the Grandmaster. Died in Avengers:_Forever_Vol_1_3.
    ## 55                                                                                                                                                                                   Killed by Iron Man when he was controlled by Immortus. Revived during Chaos war. 
    ## 56                                                                                                          Died in Captain_America_348. Actually "Dunphy miraculously survived the crash and lived with an Inuit tribe." Second death when killed by Sharon Carter.  
    ## 57                                                                                                                                                                                               Killed by Neut who was working for Immortus. Reborn into a new body. 
    ## 58                                                                                                                                                                                                                                                                <NA>
    ## 59                                                                                                                                                                                                                                                                <NA>
    ## 60                                                                                                                                                                               Killed by the Cosmic Assassin. Actually turned into a being composed of pure energy. 
    ## 61                                                                                                                                                                                                                                                                <NA>
    ## 62                                                                                                                                                                                                              Died in Great_Lakes_Avengers_Vol_1_1. Has not returned
    ## 63                                                                                                                                                             Sacrificed self so that Mr. Immortal could stop the villain Maelstrom. Returned as the Angel of Death. 
    ## 64                                                                                                                                                                                                                                                                <NA>
    ## 65                                                                                    Christ; where to begin. His superpower is that he cannot die when he dies he immediately comes back. The Wikia inventories 24 deaths. These will be excluded from the analysis. 
    ## 66                                                                                                                                                                                                                                                                <NA>
    ## 67                                                                                                                                                                                                                                                                <NA>
    ## 68                                                                                                                                                                                                                                               Killed by Leather Boy
    ## 69                                                                                                                                                                                                                                          Accidently killed by Zaran
    ## 70                                                                                                                                                                                                                                                                <NA>
    ## 71                                                                                                            Died during incursion. It's not ever particulalry clear if he ever joined an Avengers team besides one issue so his inclusion here is specious at best. 
    ## 72                                                                                                                                                                                                         Died on Battleworld and came back prior to joining avengers
    ## 73                                                                                                                                                                                                                                                                <NA>
    ## 74                                                   Since joining the New Avengers: First death Killed by Morlun. Ressurected in a brand new body. Died in Amazing Spider-Man #700. Eventually mainfested in his body that Octavious stole and then took over again. 
    ## 75                                                                                                                                                                                                                                                                <NA>
    ## 76                                                                                                                                                                                                                                                                <NA>
    ## 77                                                                                                                                                                   Died in Identity_Disc_Vol_1_1. Later was revealed to be alive and working along with the Vulture.
    ## 78                                                                                                                                                                                                                                                                <NA>
    ## 79                                                                                   Became posessed died to purge self of corruption. Revived as undead Avenger minion of the Grim Reaper. Returned to the spirit world by the Scarlet Witch. Has since stayed dead. 
    ## 80                                                                                                                                                                                                                                                                <NA>
    ## 81                                                                                                                                                                                                                                                                <NA>
    ## 82                                                                                                                                Reduced her to a pile of bones and organsby Vargas. Brought back by Chaos King. Died second time in Chaos_War:_Dead_Avengers_Vol_1_3
    ## 83                                                                                                                                                                                                                                          Merged with 616 Tony Stark
    ## 84                                                                                                                                                                                                     Whitney killed was a clone possibly this incarnation of Masque'
    ## 85                                                                                                                                                                                                                                                                <NA>
    ## 86                                                                                                                                                                                                                                                                <NA>
    ## 87                                                                                                                                                                                                                                                                <NA>
    ## 88                                                                                                                                                                                                                                                                <NA>
    ## 89                                                                    First died in a zero energy explosion in space. Body was apparently reanimated by Scarlet Witch. Abruptly exploded killing Scott Lang as well. Second return in Marvel_Zombies_Supreme_Vol_1_2. 
    ## 90                                                                                                                                                                                                                                                                <NA>
    ## 91                                                                                                                                                                                                                                                                <NA>
    ## 92                                                                                                                                                                                                                       Killed by Norman Osbourne. Has not returned. 
    ## 93                                                                                                                                                                                                            Died in Death_of_Wolverine_Vol_1_4. Has not yet returned
    ## 94                                                                                                                                                 Died during Seige. Brought back to life by the Apocalypse twins as a horseman of death in Uncanny_Avengers_Vol_1_9.
    ## 95                                                                                      First death 'had been murdered in a fight with Elektra.' "Revived by Elektra and the Hand with their dark magic." Died in most recent run of Moon Knight has not yet returned.
    ## 96                                                                                                                                                                                                                    NA is actually Kang but let's just not go there.
    ## 97                                                                                                                                                                                                                                                                <NA>
    ## 98                                                                                                                                                                                                                                                                <NA>
    ## 99                                                                                                                                                                                                                                                                <NA>
    ## 100                                                                                                                                                          Dies in Young Avengers: The Childrens Crusade when she is killed by Doom. Eventually reborn years later. 
    ## 101                                                                                                                                                                                                                                                               <NA>
    ## 102                                                                                                                                                                                                                      Dies in Young Avengers The Children's Crusade
    ## 103                                                                                                                                                                                                                                                               <NA>
    ## 104                                                                                                                                                                                                                                                               <NA>
    ## 105                                                                                                                                                                                                                                                               <NA>
    ## 106                                                                                                                                                                   Ripped in half by the Sentry during Seige. Revived during Chaos King. Then killedby Chaos King. 
    ## 107                                                                                                                                                                                                                                                               <NA>
    ## 108                                                                                                                                                                                                                                                               <NA>
    ## 109                                                                                                                                                                                                                                                               <NA>
    ## 110                                                                                                                                                                                                                                                               <NA>
    ## 111                                                                                                                                                                                                                                                               <NA>
    ## 112                                                                                                                                                                                                                                                               <NA>
    ## 113                                                                                                                                                                       Died in Captain_America_Vol_7_10. It's not clear how she survived but yeah she totally did. 
    ## 114                                                                                                                                                                                                                                 Died in Secret_Avengers_Vol_1_22. 
    ## 115                                                                                                                                                                                                 NA but died/came back during Ragnarok prior to becoming an Avenger
    ## 116                                                                                                                                                                                                                          Died in Guardians_of_the_Galaxy_Vol_3_20.
    ## 117                                                                                                                                                                                                                                                               <NA>
    ## 118                                                                                                                                                                                                        Died in Heroic_Age:_One_Month_to_Live_Vol_1_5 stayed dead. 
    ## 119                                                                                                                                                                                                                                                               <NA>
    ## 120                                                                                                                                                                                              Died in Circle of Four arc. Made a deal with Mephisto and came back. 
    ## 121                                                                                                                                                                                                                                                               <NA>
    ## 122                                                                                                                                                                                                                                                               <NA>
    ## 123                                                                                                                                                                                                                                                               <NA>
    ## 124                                                                                                                                                                                                                                                               <NA>
    ## 125                                                                                                                                                                                                                                                               <NA>
    ## 126                                                                                                                                                                                                                                                               <NA>
    ## 127                                                                                                                                                                                                                                                               <NA>
    ## 128                                                                                                                                                                                                                                                               <NA>
    ## 129                                                                                                                                                                                                                                                               <NA>
    ## 130                                                                                                                                                                                                                                                               <NA>
    ## 131                                                                                                                                                                                                                          Died in Murderworld during Avengers Arena
    ## 132                                                                                                                                                                                                                          Died in Murderworld during Avengers Arena
    ## 133                                                                                                                                                                                                                                                               <NA>
    ## 134                                                                                                                                                                                                                                                               <NA>
    ## 135                                                                                                                                                                                                                                                               <NA>
    ## 136                                                                                                                                                                                                                                                               <NA>
    ## 137                                                                                                                                                                                                                                                               <NA>
    ## 138                                                                                                                                                                                                                                                               <NA>
    ## 139                                                                                                                                                                                                                                                               <NA>
    ## 140                                                                                                                                                                                                                                                               <NA>
    ## 141                                                                                                                                                                                                                                                               <NA>
    ## 142                                                                                                                                                                                                                                                               <NA>
    ## 143                                                                                                                                                                                                                                                               <NA>
    ## 144                                                                                                                                                                                                                             Died in Superior_Spider-Man_Vol_1_30. 
    ## 145                                                                                                                                                                                                                                                               <NA>
    ## 146                                                                                                                                                                                                                                                               <NA>
    ## 147                                                                                                                                                                                                                                                               <NA>
    ## 148                                                                                                                                                                                                                                                               <NA>
    ## 149                                                                                                                                                                                                                                                               <NA>
    ## 150                                                                                                                                                                                                                                                               <NA>
    ## 151                                                                                                                                                                                                                                    Died in New_Avengers_Vol_3_32. 
    ## 152                                                                                                                                                                                                                                                               <NA>
    ## 153                                                                                                                                 Died in Uncanny_Avengers_Vol_1_22. Returned when 'he was able to use his power to transform his body into a state of pure energy '
    ## 154                                                                                                                                                                                                                                      Died in New_Avengers_Vol_3_32
    ## 155                                                                                                                                                                                                                                      Died in New_Avengers_Vol_3_32
    ## 156                                                                                                                                                                                                                                      Died in New_Avengers_Vol_3_32
    ## 157                                                                                                                                                                                                                                      Died in New_Avengers_Vol_3_32
    ## 158                                                                                                                                                                                                                                                               <NA>
    ## 159                                                                                                                                                                                                                                                               <NA>
    ## 160                                                                                                                                                                                                                                                               <NA>
    ## 161                                                                                                                                                                                                                                                               <NA>
    ## 162                                                                                                                                                                                                                                                               <NA>
    ## 163                                                                                                                                                                                                                                                               <NA>
    ## 164                                                                                                                                                                                                                                                               <NA>
    ## 165                                                                                                                                                                                                 Died in Avengers_A.I._Vol_1_4. Returned in Avengers_A.I._Vol_1_9. 
    ## 166                                                                                                                                                                                                                                                               <NA>
    ## 167                                                                                                                                                                                                                                                               <NA>
    ## 168                                                                                                                                                                                                                                                               <NA>
    ## 169                                                                                                                                                                                                                                                               <NA>
    ## 170                                                                                                                                                                                                                                                               <NA>
    ## 171                                                                                                                                                                                                                                                               <NA>
    ## 172                                                                                                                                                                                                                                                               <NA>
    ## 173                                                                                                                                                                                                                                                               <NA>
    ##     times
    ## 1       1
    ## 2       1
    ## 3       1
    ## 4       1
    ## 5       2
    ## 6       0
    ## 7       1
    ## 8       2
    ## 9       1
    ## 10      1
    ## 11      1
    ## 12      0
    ## 13      0
    ## 14      1
    ## 15      0
    ## 16      1
    ## 17      1
    ## 18      0
    ## 19      2
    ## 20      1
    ## 21      2
    ## 22      1
    ## 23      1
    ## 24      0
    ## 25      0
    ## 26      0
    ## 27      0
    ## 28      0
    ## 29      0
    ## 30      3
    ## 31      0
    ## 32      0
    ## 33      5
    ## 34      0
    ## 35      1
    ## 36      0
    ## 37      0
    ## 38      0
    ## 39      1
    ## 40      1
    ## 41      1
    ## 42      0
    ## 43      0
    ## 44      0
    ## 45      1
    ## 46      0
    ## 47      0
    ## 48      1
    ## 49      0
    ## 50      1
    ## 51      1
    ## 52      2
    ## 53      2
    ## 54      2
    ## 55      1
    ## 56      2
    ## 57      1
    ## 58      0
    ## 59      0
    ## 60      1
    ## 61      0
    ## 62      1
    ## 63      1
    ## 64      0
    ## 65      0
    ## 66      0
    ## 67      0
    ## 68      1
    ## 69      1
    ## 70      0
    ## 71      1
    ## 72      0
    ## 73      0
    ## 74      2
    ## 75      0
    ## 76      0
    ## 77      1
    ## 78      0
    ## 79      2
    ## 80      0
    ## 81      0
    ## 82      2
    ## 83      1
    ## 84      1
    ## 85      0
    ## 86      0
    ## 87      0
    ## 88      0
    ## 89      2
    ## 90      0
    ## 91      0
    ## 92      1
    ## 93      1
    ## 94      1
    ## 95      2
    ## 96      0
    ## 97      0
    ## 98      0
    ## 99      0
    ## 100     1
    ## 101     0
    ## 102     1
    ## 103     0
    ## 104     0
    ## 105     0
    ## 106     2
    ## 107     0
    ## 108     0
    ## 109     0
    ## 110     0
    ## 111     0
    ## 112     0
    ## 113     1
    ## 114     1
    ## 115     0
    ## 116     1
    ## 117     0
    ## 118     1
    ## 119     0
    ## 120     1
    ## 121     0
    ## 122     0
    ## 123     0
    ## 124     0
    ## 125     0
    ## 126     0
    ## 127     0
    ## 128     0
    ## 129     0
    ## 130     0
    ## 131     1
    ## 132     1
    ## 133     0
    ## 134     0
    ## 135     0
    ## 136     0
    ## 137     0
    ## 138     0
    ## 139     0
    ## 140     0
    ## 141     0
    ## 142     0
    ## 143     0
    ## 144     1
    ## 145     0
    ## 146     0
    ## 147     0
    ## 148     0
    ## 149     0
    ## 150     0
    ## 151     1
    ## 152     0
    ## 153     1
    ## 154     1
    ## 155     1
    ## 156     1
    ## 157     1
    ## 158     0
    ## 159     0
    ## 160     0
    ## 161     0
    ## 162     0
    ## 163     0
    ## 164     0
    ## 165     1
    ## 166     0
    ## 167     0
    ## 168     0
    ## 169     0
    ## 170     0
    ## 171     0
    ## 172     0
    ## 173     0

``` r
deaths <- av |> pivot_longer(cols = starts_with("Death"), values_to = "Death") ## |> mutate(Time = parse_number(Time))



?pivot_longer
```

    ## starting httpd help server ... done

``` r
deaths <- deaths %>% 
  distinct(Name.Alias, .keep_all = TRUE)

deaths <- deaths %>%
  select(-c(name, Return1, Return2, Return3, Return4, Return5))

print(deaths)
```

    ## # A tibble: 163 × 13
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  3 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Richard …         612 YES      MALE   ""                 
    ##  7 http://marvel.wik… "Steven R…        3458 YES      MALE   ""                 
    ##  8 http://marvel.wik… "Clinton …        1456 YES      MALE   ""                 
    ##  9 http://marvel.wik… "Pietro M…         769 YES      MALE   ""                 
    ## 10 http://marvel.wik… "Wanda Ma…        1214 YES      FEMALE ""                 
    ## # ℹ 153 more rows
    ## # ℹ 7 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Notes <chr>, times <dbl>,
    ## #   Death <chr>

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

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
