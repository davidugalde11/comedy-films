Capstone - Case Study 3
================
David Ugalde
2023-08-23

- [1 Ask](#1-ask)
  - [1.1 Summary](#11-summary)
  - [1.2 Business Task](#12-business-task)
  - [1.3 Stakeholders](#13-stakeholders)
- [2 Prepare](#2-prepare)
  - [2.1 Dataset](#21-dataset)
  - [2.2 License](#22-license)
  - [2.3 Information](#23-information)
  - [2.4 Organization](#24-organization)
  - [2.5 Credibility](#25-credibility)
- [3 Process](#3-process)
  - [3.1 Load libraries](#31-load-libraries)
  - [3.2 Import Datasets](#32-import-datasets)
  - [3.3 Preview](#33-preview)
  - [3.4 Extract genres](#34-extract-genres)
  - [3.5 Obtain ‘pure’ comedy films](#35-obtain-pure-comedy-films)
  - [3.6 Merge pure comedy data frame to get their
    values](#36-merge-pure-comedy-data-frame-to-get-their-values)
  - [3.7 Explore information
    available](#37-explore-information-available)
  - [3.8 Filter comedy film to fit Marvel’s
    inception](#38-filter-comedy-film-to-fit-marvels-inception)
  - [3.9 Remove duplicate rows](#39-remove-duplicate-rows)
- [4 Analysis](#4-analysis)
  - [4.1 Are less comedy movies being
    made?](#41-are-less-comedy-movies-being-made)
  - [4.2 Descriptive Statistics](#42-descriptive-statistics)
  - [4.3 Top ten highest grossing comedy
    films](#43-top-ten-highest-grossing-comedy-films)
  - [4.4 Filter data for financial
    analysis](#44-filter-data-for-financial-analysis)
  - [4.5 Are comedy films making less
    revenue?](#45-are-comedy-films-making-less-revenue)
  - [4.6 Does popularity affect
    revenue](#46-does-popularity-affect-revenue)
  - [4.7 Is my movie too long?](#47-is-my-movie-too-long)
  - [4.8 Filter budget to exclude null
    values](#48-filter-budget-to-exclude-null-values)
  - [4.9 Should I aks for a bigger
    budget?](#49-should-i-aks-for-a-bigger-budget)
  - [4.10 When should I release my
    movie?](#410-when-should-i-release-my-movie)
- [5 Act](#5-act)

# 1 Ask

## 1.1 Summary

American actor, comedian, singer, and screenwriter Adam DeVine has
recently come out to state that the MCU has killed comedy films. In an
interview on This Past Weekend with Theo Von, Devine asserted “I feel
like superhero movies kind of ruined comedies. Because people go to the
theater and you expect to watch something that costs \$200 million to
make and comedy movies aren’t that.”
[Source](https://deadline.com/2023/08/adam-devine-marvel-theory-superhero-movies-ruined-comedies-1235457090/)

In this case study, this analysis seeks to examine if there are any
trends to collaborate with DeVine’s statement. Analyzing if the data
shows a decrease in the quantity and/or box office of comedy films since
the inception of the Marvel Cinematic Universe (MCU) in 2008, marked by
the release of the first ‘Iron Man’ film.

Furthermore, in a more thorough examination of comedy films, we will
analyze what factors contribute to their box office success. The
findings of this analysis may serve to inform Hollywood executives and
producers, as to what they should be on the lookout for a potentially,
successful comedy film in today’s movie-going landscape.

## 1.2 Business Task

Analyze the impact the MCU has had on the success and production of
comedy films. In addition, identify trends that could guide stakeholders
on the production of comedy films.

## 1.3 Stakeholders

- Hollywood Executives
- Comedy Produces

# 2 Prepare

## 2.1 Dataset

We analyzed **The Movies Dataset** this dataset is an ensemble of data
collected from TMDB and GroupLens. The dataset is hosted in Kaggle by
the user *Rounak Banik*.

## 2.2 License

**CC0: Public Domain**: You can copy, modify, distribute, and perform
the work, even for commercial purposes, all without asking permission.

## 2.3 Information

These files contain metadata for all 45,000 movies listed in the Full
MovieLens Dataset. The dataset consists of movies released on or before
July 2017. Data points include cast, crew, plot keywords, budget,
revenue, posters, release dates, languages, production companies,
countries, TMDB vote counts, and vote averages. This dataset also has
files containing 26 million ratings from 270,000 users for all 45,000
movies. Ratings are on a scale of 1-5 and have been obtained from the
official GroupLens website.

## 2.4 Organization

There are eight CSV documents. The films had their individual ID with
the movies_metadata.csv file containing the bulk of the information for
this analysis.

## 2.5 Credibility

The database contains metadata about films no later than July 2017,
therefore the data is missing six years of information and with Devine’s
comments being made in 2023 our study is not as current as it should be.
In addition, there’s a substantial amount of empty cells or 0 values
(i.e budget) which could be due to lack of information. This means our
data is incomplete and not as reliable as it should be.

# 3 Process

We chose to conduct this analysis using R because the dataset’s size
exceeded the capabilities of spreadsheet software, and we also aimed to
create visualizations.

## 3.1 Load libraries

``` r
library(tidyverse)
library(janitor)
library(skimr)
library(jsonlite)
```

## 3.2 Import Datasets

We will import the movies_metadata and ratings csv files being that they
hold the information to gather insights for our business task. Keywords,
credits, and links are not necessary for this analysis.

## 3.3 Preview

Preview showcases the genre column holding a json file that contains the
many genres per film.

``` r
glimpse(movies)
```

    ## Rows: 45,466
    ## Columns: 24
    ## $ adult                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    ## $ belongs_to_collection <chr> "{'id': 10194, 'name': 'Toy Story Collection', '…
    ## $ budget                <dbl> 30000000, 65000000, 0, 16000000, 0, 60000000, 58…
    ## $ genres                <chr> "[{'id': 16, 'name': 'Animation'}, {'id': 35, 'n…
    ## $ homepage              <chr> "http://toystory.disney.com/toy-story", NA, NA, …
    ## $ id                    <dbl> 862, 8844, 15602, 31357, 11862, 949, 11860, 4532…
    ## $ imdb_id               <chr> "tt0114709", "tt0113497", "tt0113228", "tt011488…
    ## $ original_language     <chr> "en", "en", "en", "en", "en", "en", "en", "en", …
    ## $ original_title        <chr> "Toy Story", "Jumanji", "Grumpier Old Men", "Wai…
    ## $ overview              <chr> "Led by Woody, Andy's toys live happily in his r…
    ## $ popularity            <dbl> 21.946943, 17.015539, 11.712900, 3.859495, 8.387…
    ## $ poster_path           <chr> "/rhIRbceoE9lR4veEXuwCC2wARtG.jpg", "/vzmL6fP7aP…
    ## $ production_companies  <chr> "[{'name': 'Pixar Animation Studios', 'id': 3}]"…
    ## $ production_countries  <chr> "[{'iso_3166_1': 'US', 'name': 'United States of…
    ## $ release_date          <date> 1995-10-30, 1995-12-15, 1995-12-22, 1995-12-22,…
    ## $ revenue               <dbl> 373554033, 262797249, 0, 81452156, 76578911, 187…
    ## $ runtime               <dbl> 81, 104, 101, 127, 106, 170, 127, 97, 106, 130, …
    ## $ spoken_languages      <chr> "[{'iso_639_1': 'en', 'name': 'English'}]", "[{'…
    ## $ status                <chr> "Released", "Released", "Released", "Released", …
    ## $ tagline               <chr> NA, "Roll the dice and unleash the excitement!",…
    ## $ title                 <chr> "Toy Story", "Jumanji", "Grumpier Old Men", "Wai…
    ## $ video                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    ## $ vote_average          <dbl> 7.7, 6.9, 6.5, 6.1, 5.7, 7.7, 6.2, 5.4, 5.5, 6.6…
    ## $ vote_count            <dbl> 5415, 2413, 92, 34, 173, 1886, 141, 45, 174, 119…

## 3.4 Extract genres

In order to get the genres we must parse the movies df utilizing
fromJSON but beforehand switching from single to double quotes in order
to adopt appropriate format.

``` r
head(genres)
```

    ## # A tibble: 6 × 3
    ##      id title     genres   
    ##   <dbl> <fct>     <fct>    
    ## 1   862 Toy Story Animation
    ## 2   862 Toy Story Comedy   
    ## 3   862 Toy Story Family   
    ## 4  8844 Jumanji   Adventure
    ## 5  8844 Jumanji   Fantasy  
    ## 6  8844 Jumanji   Family

## 3.5 Obtain ‘pure’ comedy films

A lot of the films contain multiple genres. However, in Adam Devine’s
interview he mentions that more often than not comedy films have to be
pitched as action and other genres to be made. Therefore, we obtained
the films that only contain comedy.

``` r
pure_comedy_films <- genres %>% 
  group_by(id, title) %>% 
  filter(all(genres == "Comedy"))

head(pure_comedy_films)
```

    ## # A tibble: 6 × 3
    ## # Groups:   id, title [6]
    ##      id title                                                             genres
    ##   <dbl> <fct>                                                             <fct> 
    ## 1 11862 Father of the Bride Part II                                       Comedy
    ## 2 10607 Don't Be a Menace to South Central While Drinking Your Juice in … Comedy
    ## 3  9536 Bio-Dome                                                          Comedy
    ## 4 10634 Friday                                                            Comedy
    ## 5 13997 Black Sheep                                                       Comedy
    ## 6 40154 A Midwinter's Tale                                                Comedy

## 3.6 Merge pure comedy data frame to get their values

We know merge the pure comedy films data frames with a left join that
will return everything in the pure comedy data frame and the
corresponding values in the movies data frame.

``` r
head(comedy_movies_merged)
```

    ## # A tibble: 6 × 25
    ## # Groups:   id, title [6]
    ##      id title      genres.x adult belongs_to_collection budget genres.y homepage
    ##   <dbl> <chr>      <fct>    <lgl> <chr>                  <dbl> <chr>    <chr>   
    ## 1 11862 Father of… Comedy   FALSE {'id': 96871, 'name'…  0     [{'id':… <NA>    
    ## 2 10607 Don't Be … Comedy   FALSE <NA>                   0     [{'id':… <NA>    
    ## 3  9536 Bio-Dome   Comedy   FALSE <NA>                   1.5e7 [{'id':… <NA>    
    ## 4 10634 Friday     Comedy   FALSE {'id': 43563, 'name'…  3.5e6 [{'id':… http://…
    ## 5 13997 Black She… Comedy   FALSE <NA>                   0     [{'id':… <NA>    
    ## 6 40154 A Midwint… Comedy   FALSE <NA>                   0     [{'id':… <NA>    
    ## # ℹ 17 more variables: imdb_id <chr>, original_language <chr>,
    ## #   original_title <chr>, overview <chr>, popularity <dbl>, poster_path <chr>,
    ## #   production_companies <chr>, production_countries <chr>,
    ## #   release_date <date>, revenue <dbl>, runtime <dbl>, spoken_languages <chr>,
    ## #   status <chr>, tagline <chr>, video <lgl>, vote_average <dbl>,
    ## #   vote_count <dbl>

## 3.7 Explore information available

We get the column names in order to see what information is available to
us.

``` r
colnames(comedy_movies_merged)
```

    ##  [1] "id"                    "title"                 "genres.x"             
    ##  [4] "adult"                 "belongs_to_collection" "budget"               
    ##  [7] "genres.y"              "homepage"              "imdb_id"              
    ## [10] "original_language"     "original_title"        "overview"             
    ## [13] "popularity"            "poster_path"           "production_companies" 
    ## [16] "production_countries"  "release_date"          "revenue"              
    ## [19] "runtime"               "spoken_languages"      "status"               
    ## [22] "tagline"               "video"                 "vote_average"         
    ## [25] "vote_count"

## 3.8 Filter comedy film to fit Marvel’s inception

The first Iron Man film came out May 2nd, 2005 so we will get movies
from this point forward. Note that we limit the filter to July 30th,
2017 because the movies csv seems to have been updated at some values,
but this is done sparingly. Therefore, the original parameters described
at the Kaggle notebook were used in order to not skew the analysis.

``` r
comedy_films_filtered <- comedy_movies_merged %>% 
  filter(release_date >= as.Date("2008-05-02") & release_date <= as.Date("2017-07-30")) %>% 
  select(title, release_date, runtime, popularity, budget, revenue)
  
head(comedy_films_filtered)
```

    ## # A tibble: 6 × 7
    ## # Groups:   id, title [6]
    ##       id title           release_date runtime popularity   budget   revenue
    ##    <dbl> <chr>           <date>         <dbl>      <dbl>    <dbl>     <dbl>
    ## 1 136558 Kingdom Come    2011-01-01        88      0.986        0         0
    ## 2  14530 American Crude  2008-06-03        94      5.68         0         0
    ## 3  13172 The Promotion   2008-06-06        86      5.92         0         0
    ## 4  12133 Step Brothers   2008-07-25        98      8.58  65000000 128107642
    ## 5  13991 College         2008-08-28        94     10.4    7000000   6230276
    ## 6  14771 The Onion Movie 2008-05-31        80      5.69         0         0

## 3.9 Remove duplicate rows

The left join created multiple combination of row that aren’t necessary
and lower the efficiency of program, thus they are also filtered out for
this study.

``` r
comedy_films_filtered <- comedy_films_filtered %>% 
  distinct(id, .keep_all = TRUE) 

head(comedy_films_filtered)
```

    ## # A tibble: 6 × 7
    ## # Groups:   id, title [6]
    ##       id title           release_date runtime popularity   budget   revenue
    ##    <dbl> <chr>           <date>         <dbl>      <dbl>    <dbl>     <dbl>
    ## 1 136558 Kingdom Come    2011-01-01        88      0.986        0         0
    ## 2  14530 American Crude  2008-06-03        94      5.68         0         0
    ## 3  13172 The Promotion   2008-06-06        86      5.92         0         0
    ## 4  12133 Step Brothers   2008-07-25        98      8.58  65000000 128107642
    ## 5  13991 College         2008-08-28        94     10.4    7000000   6230276
    ## 6  14771 The Onion Movie 2008-05-31        80      5.69         0         0

# 4 Analysis

## 4.1 Are less comedy movies being made?

First we create a histogram to show the frequency of comedy films
distributed amongs the creation of the MCU to 2017.

``` r
ggplot(data = comedy_films_filtered) +
  geom_histogram(mapping = aes(x = release_date)) +
  labs(title = "Histogram of Comedy Movies Post-MCU",
       x = "Release Date",
       y = "Number of Films Released")
```

![](Capstone-Case-Study3_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->
<br/ >Our distribution does not collaborate the assessment that less
comedy films have been made due to the success of MCU films. However,
it’s important to note that six years worth of data are missing and
these could potentially show a decrease in comedy films creating a
right-tail distribution due to the MCU continued existence.

## 4.2 Descriptive Statistics

Note that we exclude n/a values and values that are zero(missing values)
from our analysis. This significantly reduces the credibility of our
analysis.

``` r
comedy_films_filtered_all <- comedy_films_filtered %>% 
  filter_all(all_vars(!is.na(.) & . != 0))

summary(comedy_films_filtered_all)
```

    ##        id            title            release_date           runtime     
    ##  Min.   :  4258   Length:136         Min.   :2008-07-25   Min.   : 62.0  
    ##  1st Qu.: 48845   Class :character   1st Qu.:2011-01-12   1st Qu.: 92.0  
    ##  Median : 87930   Mode  :character   Median :2012-12-15   Median :100.0  
    ##  Mean   :135509                      Mean   :2012-10-21   Mean   :101.5  
    ##  3rd Qu.:226654                      3rd Qu.:2014-07-05   3rd Qu.:108.0  
    ##  Max.   :430263                      Max.   :2017-02-16   Max.   :207.0  
    ##    popularity          budget             revenue         
    ##  Min.   : 0.2301   Min.   :        1   Min.   :     9069  
    ##  1st Qu.: 6.0383   1st Qu.:  6375000   1st Qu.: 10744014  
    ##  Median : 8.3223   Median : 20000000   Median : 45437598  
    ##  Mean   : 8.9767   Mean   : 25634200   Mean   : 73964790  
    ##  3rd Qu.:11.3385   3rd Qu.: 36500000   3rd Qu.:109521754  
    ##  Max.   :42.0615   Max.   :112000000   Max.   :459270619

Next we create a descriptive statistical analysis of our comedy films in
order to observe some interesting patterns.

- Most pure comedy films were released in late-2012 a little over four
  years after the MCU begun.
- The most successful comedy film in the time period made \$459270619
  which was “The Hangover” (2009).

## 4.3 Top ten highest grossing comedy films

Lets get a list of the top 10 comedy films that generated the most
revenue.

``` r
top_ten_revenue <- comedy_films_filtered_all %>% 
  select(title, revenue) %>% 
  arrange(desc(revenue)) %>% 
  head(10)

print(top_ten_revenue)
```

    ## # A tibble: 10 × 3
    ## # Groups:   id, title [10]
    ##        id title                   revenue
    ##     <dbl> <chr>                     <dbl>
    ##  1  18785 The Hangover          459270619
    ##  2 109439 The Hangover Part III 362000072
    ##  3  38365 Grown Ups             271430189
    ##  4 195589 Neighbors             268157400
    ##  5  45243 The Hangover Part II  254455986
    ##  6 109418 Grown Ups 2           246984278
    ##  7 274167 Daddy's Home          242786137
    ##  8  38745 Gulliver's Travels    237382724
    ##  9  71552 American Reunion      234989584
    ## 10  10201 Yes Man               225990978

From this data we can see that five spots of the top ten comedy films
are franchises. This could provide insight as to the lucrative power of
franchises in the comedy film space. One could argue that these films go
sequels due to the success o the first film.

## 4.4 Filter data for financial analysis

Due to the vast amount of missing values in our data set this time only
revenue’s null/0 values will be filtered out as to not miss any data
that has release date and revenue/budget.

``` r
comedy_films_filtered <- comedy_films_filtered %>% 
  filter(!is.na(revenue) & revenue != 0)
```

## 4.5 Are comedy films making less revenue?

``` r
ggplot(data = comedy_films_filtered) +
  geom_point(mapping = aes(x = release_date, y = revenue, color = revenue)) +
  geom_smooth(mapping = aes(x = release_date, y = revenue)) +
  labs(title = "Relationship Between Release Date and Revenue for Comedy Films",
       x = "Release Date",
       y = "Revenue")
```

![](Capstone-Case-Study3_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->
<br/ >Once again the amount of revenue comedy movies have generated
remain stable post-MCU. With an outlier (box office hit) occurring every
once in a while. This once again expresses that there isn’t a
significant correlation between the MCU inception and the amount of
revenue comedy films generate.

## 4.6 Does popularity affect revenue

``` r
ggplot(data = comedy_films_filtered) +
  geom_point(mapping = aes(x = popularity, y = revenue)) +
  geom_smooth(mapping = aes(x = popularity, y = revenue)) +
  labs(title = "Relationship between Popularity and Revenue for Comedy Films",
       x = "Popularity",
       y = "Revenue")
```

![](Capstone-Case-Study3_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->
<br/ >As expected the more popular a film is the more revenue it will
generate. However, as seen by our correlation coefficient this is a
moderate correlation at best. Indicating some interesting information.
Are most of the audiences willing to go to the movie theater for a
comedy film go once it reaches a moderate popularity? Could this help us
determine the marketing budget for comedy films? Could streaming
services be at play due to movie goers preferring to wait for their
films to hit the insulary market?

``` r
print(cor(comedy_films_filtered$popularity, comedy_films_filtered$revenue))
```

    ## [1] 0.5854383

## 4.7 Is my movie too long?

``` r
ggplot(data = comedy_films_filtered) +
  geom_point(mapping = aes(x = runtime, y = revenue)) +
  geom_smooth(mapping = aes(x = runtime, y = revenue)) +
  labs(title = "Relationship Between Run Time and Revenue for Comedy Films",
       x = "Runtime",
       y = "Revenue")
```

![](Capstone-Case-Study3_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->
<br/ >The linear relationship between run time and revenue is far weaker
than the relationship between popularity and revenue. This could be
useful when considering financing a film as a longer movie could end up
being more expensive. Which in turn doesn’t have a correlation to a
higher revenue. Knowing this could inform comedy film produces at
allocating their budget else where.

## 4.8 Filter budget to exclude null values

``` r
comedy_films_filtered <- comedy_films_filtered %>% 
  filter(!is.na(budget) & budget != 0)
```

## 4.9 Should I aks for a bigger budget?

``` r
ggplot(data = comedy_films_filtered) +
  geom_point(mapping = aes(x = budget, y = revenue)) +
  geom_smooth(mapping = aes(x = budget, y = revenue)) +
  labs(title = "Relationship Between Budget and Revenue for Comedy Films",
       x = "Budget",
       y = "Revenue")
```

![](Capstone-Case-Study3_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->
<br/ >There is a moderate positive correlation between budget and
revenue which makes sense especially when considering the top 10 highest
grossing comedy films of this analysis. Star-studded, blockbuster comedy
films should be studied further on, however we should also asses the
risk to reward ratio of trying to recreate events like “The Hangover” or
“Grownups.” However, this correlation isn’t very strong thus new
analysis should be made about creating a release strategy plan to
release low to mid tier budget comedy films to generate consistent, safe
profit. Next, we examine the months comedy films do better at the box
office which will collaborate the need to develop a new release
strategy.

``` r
print(cor(comedy_films_filtered$revenue, comedy_films_filtered$budget))
```

    ## [1] 0.6622439

## 4.10 When should I release my movie?

### 4.10.1 Create month data frame with average revenue

We will need to get the average revenue per month and we do this by
obtaining our months row by taking this from date column. Then we group
by month and we get the average revenue by month.

``` r
monthly_average <- comedy_films_filtered %>% 
  mutate(month = format(release_date, "%m")) %>% 
  group_by(month) %>% 
  summarise(avg_revenue = mean(revenue, na.rm = TRUE))
```

### 4.10.2 Create a bar graph to represent revenue and month relationship

``` r
bar_month <- ggplot(data = monthly_average) +
  geom_bar(mapping = aes(x = month, y = avg_revenue), stat = "identity", fill = "blue") +
  labs(title = "Average Revenue for Comedy Films Grouped by Month",
       x = "Months",
       y = "Average Revenue")
```

### 4.10.3 Create month data frame with film count

``` r
monthly_film_sum <- comedy_films_filtered %>% 
  mutate(month = format(release_date, "%m")) %>% 
  group_by(month) %>% 
  summarise(film_count = n())
```

### 4.10.4 Create a line graph to represent amount of films and month relationship

``` r
line_month <- ggplot(data = monthly_film_sum) +
  geom_line(mapping = aes(x = month, y = film_count, group = 1), color = "red") +
  geom_point(mapping = aes(x = month, y = film_count), color = "red", size = 3) +
  labs(title = "Number of Comedy Films Released Each Month",
       x = "Month",
       y = "Film Count")
```

``` r
library(gridExtra)
grid.arrange(bar_month, line_month, ncol = 1)
```

![](Capstone-Case-Study3_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->
<br>This visualization provides some interesting trends that could
inform comedy movie producers about the best time to release their
comedy films. The months that have the highest average revenue are May
through July and this demonstrates that unlike popular belief comedy
films hold weight in the Summer box office arena. What’s interesting is
that the number of films released at this time is actually *lower* than
the months prior to the Summer months. This could be because movies
executives want to release their films before to avoid competition with
big movies. This provides an opportunity to exploit a new business model
and abandon ineffective movie release practices.

# 5 Act

Our findings have found out that **the MCU has isn’t correlated to the
release nor the revenue of comedy films.** However, it’s important to
reiterate the limitations of this study due to the lack of currency and
reliability of our dataset. We propose that this topic is addressed once
more with a more thorough primary data. In addition, it should be noted
that the age of the MCU is also the age of Netflix. **Thus, we should
explore how both the superhero genre and the streaming service age has
impacted comedy films. **

We have laid out the following recommendations for Hollywood Executives
and Comedy Producers when it comes to their plans of making new comedy
films.

<u>Recommendations.</u>

1.  **We should save money on marketing and run time to lower the budget
    of our comedy films.** Creating a comedy blockbuster shouldn’t be a
    priority due to it’s spontaneity and their high budget required.
    **Instead we propose focusing in comedy movies with modest to
    intermediate budgets.** However, we should be on the lookout for
    unexpected box office hits from which we should accelerate
    development of sequel/ trilogy.

2.  **Comedy movies should serve as counter programming for MCU-esque
    blockbuster juggernauts** It’s apparent that comedy films are
    released at higher frequencies the months prior to the Summer season
    which is known to be a competitive market place. However, this is a
    *mistake* because our budget-friendly movies could serve as an
    alternative to superhero films. Although, we won’t make a profit
    anywhere near a big MCU film. This will still be a consistent, safe
    release strategy that will serve to accrue content that we can
    deploy to the digital market.
