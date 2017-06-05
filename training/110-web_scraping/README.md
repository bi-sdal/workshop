-   [Ways of getting information from the web](#ways-of-getting-information-from-the-web)
-   [Getting Tables from websites](#getting-tables-from-websites)
-   [Scraping Websites](#scraping-websites)
-   [Scraping](#scraping)

Ways of getting information from the web
========================================

-   Download data manually
-   Use an API (Application Programming Interface)
-   Scrape it
    -   Check the Terms and Conditoins (TOC)

Getting Tables from websites
============================

Useful for getting tables from wikipedia

Getting US States Abbreviations

<https://en.wikipedia.org/wiki/List_of_U.S._state_abbreviations>

``` r
library(RCurl)
```

    ## Loading required package: bitops

``` r
library(XML)
library(testthat)
library(stringr)
# url is a name of a function
wiki_url <- RCurl::getURL("https://en.wikipedia.org/wiki/List_of_U.S._state_abbreviations")
```

``` r
tables <- XML::readHTMLTable(wiki_url)
class(tables)
```

    ## [1] "list"

``` r
length(tables)
```

    ## [1] 5

``` r
abbrevs <- tables[[1]]
head(abbrevs)
```

    ##               V1
    ## 1         Codes:
    ## 2            ISO
    ## 3           ANSI
    ## 4           USPS
    ## 5           USCG
    ## 6 Abbreviations:
    ##                                                                                                        V2
    ## 1                                                                                                    <NA>
    ## 2 ISO 3166 codes (2-letter, 3-letter and 3-digit codes from ISO 3166-1; 2+2-letter codes from ISO 3166-2)
    ## 3                                        2-letter and 2-digit codes from the ANSI standard INCITS 38:2009
    ## 4                                                 2-letter codes used by the United States Postal Service
    ## 5 2-letter codes used by the United States Coast Guard (red text shows differences between ANSI and USPS)
    ## 6                                                                                                    <NA>
    ##     V3   V4   V5   V6   V7   V8   V9  V10
    ## 1 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 2 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 3 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 4 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 5 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>
    ## 6 <NA> <NA> <NA> <NA> <NA> <NA> <NA> <NA>

``` r
us <- abbrevs[11:nrow(abbrevs), ]
head(us)
```

    ##                           V1            V2           V3 V4 V5 V6 V7     V8
    ## 11  United States of America Federal state US\nUSA\n840 US 00         U.S.
    ## 12                   Alabama         State        US-AL AL 01 AL AL   Ala.
    ## 13                    Alaska         State        US-AK AK 02 AK AK Alaska
    ## 14                   Arizona         State        US-AZ AZ 04 AZ AZ  Ariz.
    ## 15                  Arkansas         State        US-AR AR 05 AR AR   Ark.
    ## 16                California         State        US-CA CA 06 CA CF Calif.
    ##        V9       V10
    ## 11   U.S.    U.S.A.
    ## 12   Ala.          
    ## 13 Alaska     Alas.
    ## 14  Ariz.       Az.
    ## 15   Ark.          
    ## 16 Calif. Ca., Cal.

``` r
first_value <- stringr::str_trim((as.character(us[1, 1])))
testthat::expect_equal(object = first_value, expected = 'United States of America')
```

``` r
# testthat::expect_equal(object = stringr::str_trim((as.character(us[1, 1]))), expected = 'does not match')
```

Scraping Websites
=================

<https://stat4701.github.io/edav/2015/04/02/rvest_tutorial/>

``` r
library(rvest)
```

    ## Loading required package: xml2

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:XML':
    ## 
    ##     xml

``` r
if (interactive()) {
  data_location <- 'data/working'
} else {
  data_location <- '../../data/working'
}
```

Scraping
========

IMDB Top Rated Movies:

<http://www.imdb.com/chart/top?ref_=nv_mv_250_6>

<http://selectorgadget.com/>

CSS class and id

``` r
lego_movie <- read_html("http://www.imdb.com/title/tt1490017/")
```

``` r
# Rating
lego_movie %>% 
  html_node("strong span") %>%
  html_text() %>%
  as.numeric()
```

    ## [1] 7.8

``` r
# First page of actors
lego_movie %>%
  html_nodes(".itemprop .itemprop") %>%
  html_text()
```

    ##  [1] "Will Arnett"     "Elizabeth Banks" "Craig Berry"    
    ##  [4] "Alison Brie"     "David Burrows"   "Anthony Daniels"
    ##  [7] "Charlie Day"     "Amanda Farinos"  "Keith Ferguson" 
    ## [10] "Will Ferrell"    "Will Forte"      "Dave Franco"    
    ## [13] "Morgan Freeman"  "Todd Hansen"     "Jonah Hill"

``` r
lego_movie %>%
  html_nodes("table") %>%
  .[[1]] %>%
  html_table()
```

    ##                                   X1                                X2
    ## 1  Cast overview, first billed only: Cast overview, first billed only:
    ## 2                                                          Will Arnett
    ## 3                                                      Elizabeth Banks
    ## 4                                                          Craig Berry
    ## 5                                                          Alison Brie
    ## 6                                                        David Burrows
    ## 7                                                      Anthony Daniels
    ## 8                                                          Charlie Day
    ## 9                                                       Amanda Farinos
    ## 10                                                      Keith Ferguson
    ## 11                                                        Will Ferrell
    ## 12                                                          Will Forte
    ## 13                                                         Dave Franco
    ## 14                                                      Morgan Freeman
    ## 15                                                         Todd Hansen
    ## 16                                                          Jonah Hill
    ##                                   X3
    ## 1  Cast overview, first billed only:
    ## 2                                ...
    ## 3                                ...
    ## 4                                ...
    ## 5                                ...
    ## 6                                ...
    ## 7                                ...
    ## 8                                ...
    ## 9                                ...
    ## 10                               ...
    ## 11                               ...
    ## 12                               ...
    ## 13                               ...
    ## 14                               ...
    ## 15                               ...
    ## 16                               ...
    ##                                                                                                     X4
    ## 1                                                                    Cast overview, first billed only:
    ## 2                                              Batman /  \n            Bruce Wayne \n  \n  \n  (voice)
    ## 3                                                  Wyldstyle /  \n            Lucy \n  \n  \n  (voice)
    ## 4                                         Blake /  \n            Additional Voices \n  \n  \n  (voice)
    ## 5                                                                         Unikitty \n  \n  \n  (voice)
    ## 6                                   Octan Robot /  \n            Additional Voices \n  \n  \n  (voice)
    ## 7                                                                            C-3PO \n  \n  \n  (voice)
    ## 8                                                                            Benny \n  \n  \n  (voice)
    ## 9                                                                              Mom \n  \n  \n  (voice)
    ## 10                                                                        Han Solo \n  \n  \n  (voice)
    ## 11 Lord Business (voice) /  \n            President Business (voice) /  \n            The Man Upstairs
    ## 12                                              Abraham Lincoln \n  \n  \n  (voice) (as Orville Forte)
    ## 13                                                                           Wally \n  \n  \n  (voice)
    ## 14                                                                       Vitruvius \n  \n  \n  (voice)
    ## 15                                      Gandalf /  \n            Additional Voices \n  \n  \n  (voice)
    ## 16                                                                   Green Lantern \n  \n  \n  (voice)

``` r
lego_movie %>%
  html_nodes(".primary_photo , .ellipsis, .character, #titleCast .itemprop, #titleCast .loadlate")
```

    ## {xml_nodeset (87)}
    ##  [1] <td class="primary_photo">\n<a href="/name/nm0004715/?ref_=tt_cl_i1 ...
    ##  [2] <img height="44" width="32" alt="Will Arnett" title="Will Arnett" s ...
    ##  [3] <td class="itemprop" itemprop="actor" itemscope itemtype="http://sc ...
    ##  [4] <span class="itemprop" itemprop="name">Will Arnett</span>
    ##  [5] <td class="ellipsis">\n              ...\n          </td>
    ##  [6] <td class="character">\n              <div>\n            <a href="/ ...
    ##  [7] <td class="primary_photo">\n<a href="/name/nm0006969/?ref_=tt_cl_i2 ...
    ##  [8] <img height="44" width="32" alt="Elizabeth Banks" title="Elizabeth  ...
    ##  [9] <td class="itemprop" itemprop="actor" itemscope itemtype="http://sc ...
    ## [10] <span class="itemprop" itemprop="name">Elizabeth Banks</span>
    ## [11] <td class="ellipsis">\n              ...\n          </td>
    ## [12] <td class="character">\n              <div>\n            <a href="/ ...
    ## [13] <td class="primary_photo">\n<a href="/name/nm1911947/?ref_=tt_cl_i3 ...
    ## [14] <td class="itemprop" itemprop="actor" itemscope itemtype="http://sc ...
    ## [15] <span class="itemprop" itemprop="name">Craig Berry</span>
    ## [16] <td class="ellipsis">\n              ...\n          </td>
    ## [17] <td class="character">\n              <div>\n            <a href="/ ...
    ## [18] <td class="primary_photo">\n<a href="/name/nm1555340/?ref_=tt_cl_i4 ...
    ## [19] <img height="44" width="32" alt="Alison Brie" title="Alison Brie" s ...
    ## [20] <td class="itemprop" itemprop="actor" itemscope itemtype="http://sc ...
    ## ...

``` r
# more manual way
lego_movie %>%
  html_nodes("table") %>%
  .[[1]] %>%
  html_nodes("tr") %>%
  html_nodes("span") %>%
  html_text()
```

    ##  [1] "Will Arnett"     "Elizabeth Banks" "Craig Berry"    
    ##  [4] "Alison Brie"     "David Burrows"   "Anthony Daniels"
    ##  [7] "Charlie Day"     "Amanda Farinos"  "Keith Ferguson" 
    ## [10] "Will Ferrell"    "Will Forte"      "Dave Franco"    
    ## [13] "Morgan Freeman"  "Todd Hansen"     "Jonah Hill"
