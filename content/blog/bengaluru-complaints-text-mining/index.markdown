---
title: "Bengaluru Complaints Supervised Classification Model"
excerpt: "Categorizing over 100,000 complaints from the *Silicon Valley* of India "
author: Nathan States
date: '2022-08-22'
slug: []
categories: []
editor_options:
  chunk_output_type: console
---

<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/echarts-en.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/ecStat.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/dataTool.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r-binding/echarts4r.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts-theme-states.json/echarts-theme-states.json.js"></script>
<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/echarts-en.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/ecStat.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/dataTool.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r-binding/echarts4r.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts-theme-states.json/echarts-theme-states.json.js"></script>
<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/echarts-en.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/ecStat.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/dataTool.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r-binding/echarts4r.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts-theme-states.json/echarts-theme-states.json.js"></script>
<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<link href="{{< blogdown/postref >}}index_files/wordcloud2/wordcloud.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/wordcloud2/wordcloud2-all.js"></script>
<script src="{{< blogdown/postref >}}index_files/wordcloud2/hover.js"></script>
<script src="{{< blogdown/postref >}}index_files/wordcloud2-binding/wordcloud2.js"></script>
<style type="text/css">
.img {
  height: auto;
  max-width: 100%;
  background-color: #fcfcfc;
}

.html {
  height: auto;
  max-width: 100%;
}

.full-width {
  left: 50%;
  margin-left: auto;
  margin-right: auto;
  max-width: 70vw;
  position: relative;
  right: 50%;
  width: 70vw;
}

.most-width {
  left: 50%;
  margin-left: -40vw;
  margin-right: -40vw;
  max-width: 80vw;
  position: relative;
  right: 50%;
  width: 80vw;
}

.box-width{
  left: 50%;
  margin-left: -35vw;
  margin-right: -35vw;
  max-width: 70vw;
  position: relative;
  right: 50%;
  width: 70vw;
}
</style>

<div class="box-width">

<img src="preview.jpg" width="1280" />

</div>

## Results

-   Over 80% of complaints filed were related to street lights not working, road maintenance issues, or garbage collection issues.
-   Of the four classification models, the linear support vector classifier performed the best, recording **88.773%** accuracy. The top three models all performed similarly as well, though, with all falling within roughly three percentage points.
-   Improvements were made by increasing the number of stop words, as well as combining smaller categories into larger ones. Using the LinearSVC, these changes led to a *3.214%* increase, ultimately recording an accuracy of **91.987%**.
-   Further improvements could be had by adding to the stop word list, changing the contents of the “Others” category, and adjusting the downsampling of the model.

# Table of Contents

-   [Background](#background)
-   [Data Wrangling](#data-wrangling)
-   [Exploratory Data Analysis](#eda)
    -   [Number of Grievances by Day](#number-per-day)
    -   [Grievances by Category](#number-category)
    -   [Grievances by Subcategory](#number-subcategory)
    -   [Most Common Complaint Words](#number-words)
-   [MLE Models](#mle-models)
    -   [Preparing the Data](#preparing-data)
    -   [Text Preprocessing](#text-preprocessing)
    -   [Building the Models](#building-models)
    -   [Results](#results)
-   [Improvements](#improvements)
    -   [Modifications](#modifications)
    -   [Redo Text Preprocessing](#redo-text-preprocessing)
    -   [New Results](#new-results)
-   [Conclusions](#conclusions)
    -   [Further Improvements](#further-improvements)

## Background <a id="background"></a>

Text data can be one of the more difficult data types to use in analytics, but raw text is invaluable in many different ways. Using simple Python libraries, modern machine learning models can parse thousands of rows in seconds, quickly understanding blocks of text in numerous, useful ways. One of the most common tasks when dealing with text data is to group rows into different categories, which can provide a quick summary of a data.

The *Bruhat Bengaluru Mahangara Palike* (BBMP) - the administrative body that oversees city development in Bengaluru, commonly referred to as the *Silicon Valley* of India - created a web application that allows citizens to file grievances with the city. From February 8th, 2020 to February 21st, 2021, a total of **105,956** complaints were filed with the city, translating to roughly 280 grievances a day. Exploring this data not only provides insight into the most common problems facing the city of Bengaluru (or at least the complaints most likely to filed), but also presents an opportunity to quantify and categorize them.

In this data, grievances have been manually categorized by the administrators who oversee the app at BBMP, but this is extremely inefficient. Usefully, though, the developers have already created categories that they felt best sorted the data, which means (assuming complaints don’t change substantially in the future) we can train a machine learning model that performs this task automatically. Because the categories are already defined, this will be a **supervised** classification model.

## Data Wrangling <a id="data-wrangling"></a>

Data wrangling and the EDA will be done using `R`.

First, we import in the data and the necessary libraries.

``` r
# Load Libraries
library(tidyverse) 
library(tidymodels)
library(tidytext)
library(echarts4r)
library(here)
library(lubridate)
library(wordcloud2)
library(reticulate)

# Set Directory 
here::set_here()

# Import Data
grievances <- readr::read_csv("bengaluru-grievances.csv")
```

The admins at BBMP keep their data neat and tidy, so there’s not many problems to fix. There are a couple things to consider, however.

The first issue is that some descriptions are extremely short, making classifying them accurately near impossible. We can fix this by limiting the number of characters in a description, though the appropriate number of rows to remove is debatable. Ideally, we don’t want to exclude too much of the data while still removing descriptions too short to be properly categorized.

Using `str_length` from the `stringr` package, we see that 5,392 complaints contained fewer than 12 characters, meaning removing them would still preserve 95% of the original data. Using `filter` from `dplyr`, we keep all complaints containing more than 12 characters.

``` r
# Counting number of rows 
grievances %>%
  filter(str_length(description) < 12) %>%
  count() 
```

    ## # A tibble: 1 x 1
    ##       n
    ##   <int>
    ## 1  5392

``` r
# Removing those rows 
grievances <- grievances %>%
  filter(str_length(description) > 12)
```

The other consideration is whether the existing categories accurately reflect the data or not. It’s possible certain similarities between different categories would better be condensed into one, and likewise, single columns into multiple ones. These changes may not only better describe the data, but improve overall accuracy in the long run.

Doing so would require a major overhaul to the data, requiring reclassifying all the complaints into new categories. For now, we will leave the original categories intact and proceed, but future models may benefit from reclassification.

## Exploratory Data Analysis <a id="eda"></a>

Interactive charts created using `echarts4r`.

### Number of Grievances By Day <a id="number-per-day"></a>

<div class="most-width">

``` r
# Set Theme 
e_common(
  theme = "echarts-theme-states.json",
  font_family = "Playfair Display"
)

# Chart 
grievances %>%
  group_by(created_at = as.Date(created_at)) %>%
  summarise(Total = n()) %>%
  e_charts(created_at) %>%
  e_line(Total, symbol = "none") %>%
  e_x_axis(axisLabel = list(interval = 0, rotate = 45)) %>%
  e_title(
    text = "Total number of grievances by day",
    subtext = "Data: BBMP | nathanstates.com"
  ) %>%
  e_color(
    "#3414b9", 
    background = "rgb(0,0,0,0)"
  ) %>%
  e_legend(show = FALSE) %>%
  e_tooltip(
    backgroundColor = "rgba(20, 20, 20, 0.5)",
    borderColor = "rgba(0, 0, 0, 0)",
    textStyle = list(
      color = "#ffffff"
    ),
    trigger = "axis"
  )
```

<div id="htmlwidget-1" style="width:100%;height:500px;" class="echarts4r html-widget"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"theme":"echarts-theme-states.json","tl":false,"draw":true,"renderer":"canvas","events":[],"buttons":[],"opts":{"yAxis":[{"show":true}],"xAxis":[{"data":["2020-02-08","2020-02-09","2020-02-10","2020-02-11","2020-02-12","2020-02-13","2020-02-14","2020-02-15","2020-02-16","2020-02-17","2020-02-18","2020-02-19","2020-02-20","2020-02-21","2020-02-22","2020-02-23","2020-02-24","2020-02-25","2020-02-26","2020-02-27","2020-02-28","2020-02-29","2020-03-01","2020-03-02","2020-03-03","2020-03-04","2020-03-05","2020-03-06","2020-03-07","2020-03-08","2020-03-09","2020-03-10","2020-03-11","2020-03-12","2020-03-13","2020-03-14","2020-03-15","2020-03-16","2020-03-17","2020-03-18","2020-03-19","2020-03-20","2020-03-21","2020-03-22","2020-03-23","2020-03-24","2020-03-25","2020-03-26","2020-03-27","2020-03-28","2020-03-29","2020-03-30","2020-03-31","2020-04-01","2020-04-02","2020-04-03","2020-04-04","2020-04-05","2020-04-06","2020-04-07","2020-04-08","2020-04-09","2020-04-10","2020-04-11","2020-04-12","2020-04-13","2020-04-14","2020-04-15","2020-04-16","2020-04-17","2020-04-18","2020-04-19","2020-04-20","2020-04-21","2020-04-22","2020-04-23","2020-04-24","2020-04-25","2020-04-26","2020-04-27","2020-04-28","2020-04-29","2020-04-30","2020-05-01","2020-05-02","2020-05-03","2020-05-04","2020-05-05","2020-05-06","2020-05-07","2020-05-08","2020-05-09","2020-05-10","2020-05-11","2020-05-12","2020-05-13","2020-05-14","2020-05-15","2020-05-16","2020-05-17","2020-05-18","2020-05-19","2020-05-20","2020-05-21","2020-05-22","2020-05-23","2020-05-24","2020-05-25","2020-05-26","2020-05-27","2020-05-28","2020-05-29","2020-05-30","2020-05-31","2020-06-01","2020-06-02","2020-06-03","2020-06-04","2020-06-05","2020-06-06","2020-06-07","2020-06-08","2020-06-09","2020-06-10","2020-06-11","2020-06-12","2020-06-13","2020-06-14","2020-06-15","2020-06-16","2020-06-17","2020-06-18","2020-06-19","2020-06-20","2020-06-21","2020-06-22","2020-06-23","2020-06-24","2020-06-25","2020-06-26","2020-06-27","2020-06-28","2020-06-29","2020-06-30","2020-07-01","2020-07-02","2020-07-03","2020-07-04","2020-07-05","2020-07-06","2020-07-07","2020-07-08","2020-07-09","2020-07-10","2020-07-11","2020-07-12","2020-07-13","2020-07-14","2020-07-15","2020-07-16","2020-07-17","2020-07-18","2020-07-19","2020-07-20","2020-07-21","2020-07-22","2020-07-23","2020-07-24","2020-07-25","2020-07-26","2020-07-27","2020-07-28","2020-07-29","2020-07-30","2020-07-31","2020-08-01","2020-08-02","2020-08-03","2020-08-04","2020-08-05","2020-08-06","2020-08-07","2020-08-08","2020-08-09","2020-08-10","2020-08-11","2020-08-12","2020-08-13","2020-08-14","2020-08-15","2020-08-16","2020-08-17","2020-08-18","2020-08-19","2020-08-20","2020-08-21","2020-08-22","2020-08-23","2020-08-24","2020-08-25","2020-08-26","2020-08-27","2020-08-28","2020-08-29","2020-08-30","2020-08-31","2020-09-01","2020-09-02","2020-09-03","2020-09-04","2020-09-05","2020-09-06","2020-09-07","2020-09-08","2020-09-09","2020-09-10","2020-09-11","2020-09-12","2020-09-13","2020-09-14","2020-09-15","2020-09-16","2020-09-17","2020-09-18","2020-09-19","2020-09-20","2020-09-21","2020-09-22","2020-09-23","2020-09-24","2020-09-25","2020-09-26","2020-09-27","2020-09-28","2020-09-29","2020-09-30","2020-10-01","2020-10-02","2020-10-03","2020-10-04","2020-10-05","2020-10-06","2020-10-07","2020-10-08","2020-10-09","2020-10-10","2020-10-11","2020-10-12","2020-10-13","2020-10-14","2020-10-15","2020-10-16","2020-10-17","2020-10-18","2020-10-19","2020-10-20","2020-10-21","2020-10-22","2020-10-23","2020-10-24","2020-10-25","2020-10-26","2020-10-27","2020-10-28","2020-10-29","2020-10-30","2020-10-31","2020-11-01","2020-11-02","2020-11-03","2020-11-04","2020-11-05","2020-11-06","2020-11-07","2020-11-08","2020-11-09","2020-11-10","2020-11-11","2020-11-12","2020-11-13","2020-11-14","2020-11-15","2020-11-16","2020-11-17","2020-11-18","2020-11-19","2020-11-20","2020-11-21","2020-11-22","2020-11-23","2020-11-24","2020-11-25","2020-11-26","2020-11-27","2020-11-28","2020-11-29","2020-11-30","2020-12-01","2020-12-02","2020-12-03","2020-12-04","2020-12-05","2020-12-06","2020-12-07","2020-12-08","2020-12-09","2020-12-10","2020-12-11","2020-12-12","2020-12-13","2020-12-14","2020-12-15","2020-12-16","2020-12-17","2020-12-18","2020-12-19","2020-12-20","2020-12-21","2020-12-22","2020-12-23","2020-12-24","2020-12-25","2020-12-26","2020-12-27","2020-12-28","2020-12-29","2020-12-30","2020-12-31","2021-01-01","2021-01-02","2021-01-03","2021-01-04","2021-01-05","2021-01-06","2021-01-07","2021-01-08","2021-01-09","2021-01-10","2021-01-11","2021-01-12","2021-01-13","2021-01-14","2021-01-15","2021-01-16","2021-01-17","2021-01-18","2021-01-19","2021-01-20","2021-01-21","2021-01-22","2021-01-23","2021-01-24","2021-01-25","2021-01-26","2021-01-27","2021-01-28","2021-01-29","2021-01-30","2021-01-31","2021-02-01","2021-02-02","2021-02-03","2021-02-04","2021-02-05","2021-02-06","2021-02-07","2021-02-08","2021-02-09","2021-02-10","2021-02-11","2021-02-12","2021-02-13","2021-02-14","2021-02-15","2021-02-16","2021-02-17","2021-02-18","2021-02-19","2021-02-20","2021-02-21"],"type":"time","boundaryGap":true,"axisLabel":{"interval":0,"rotate":45}}],"legend":{"data":["Total"],"show":false,"type":"plain"},"series":[{"data":[{"value":["2020-02-08","228"]},{"value":["2020-02-09","339"]},{"value":["2020-02-10","244"]},{"value":["2020-02-11","325"]},{"value":["2020-02-12","280"]},{"value":["2020-02-13","222"]},{"value":["2020-02-14","307"]},{"value":["2020-02-15","319"]},{"value":["2020-02-16","209"]},{"value":["2020-02-17","234"]},{"value":["2020-02-18","239"]},{"value":["2020-02-19","320"]},{"value":["2020-02-20","235"]},{"value":["2020-02-21","139"]},{"value":["2020-02-22","225"]},{"value":["2020-02-23","190"]},{"value":["2020-02-24","268"]},{"value":["2020-02-25","340"]},{"value":["2020-02-26","294"]},{"value":["2020-02-27","295"]},{"value":["2020-02-28","312"]},{"value":["2020-02-29","742"]},{"value":["2020-03-01","357"]},{"value":["2020-03-02","371"]},{"value":["2020-03-03","378"]},{"value":["2020-03-04","366"]},{"value":["2020-03-05","350"]},{"value":["2020-03-06","358"]},{"value":["2020-03-07","388"]},{"value":["2020-03-08","306"]},{"value":["2020-03-09","363"]},{"value":["2020-03-10","389"]},{"value":["2020-03-11","354"]},{"value":["2020-03-12","332"]},{"value":["2020-03-13","313"]},{"value":["2020-03-14","294"]},{"value":["2020-03-15","230"]},{"value":["2020-03-16","390"]},{"value":["2020-03-17","348"]},{"value":["2020-03-18","396"]},{"value":["2020-03-19","302"]},{"value":["2020-03-20","311"]},{"value":["2020-03-21","362"]},{"value":["2020-03-22","148"]},{"value":["2020-03-23","343"]},{"value":["2020-03-24","151"]},{"value":["2020-03-25","176"]},{"value":["2020-03-26","179"]},{"value":["2020-03-27","125"]},{"value":["2020-03-28","136"]},{"value":["2020-03-29","176"]},{"value":["2020-03-30","251"]},{"value":["2020-03-31","199"]},{"value":["2020-04-01","164"]},{"value":["2020-04-02","152"]},{"value":["2020-04-03","125"]},{"value":["2020-04-04","164"]},{"value":["2020-04-05","110"]},{"value":["2020-04-06","137"]},{"value":["2020-04-07","188"]},{"value":["2020-04-08","149"]},{"value":["2020-04-09","177"]},{"value":["2020-04-10","183"]},{"value":["2020-04-11","178"]},{"value":["2020-04-12","126"]},{"value":["2020-04-13","132"]},{"value":["2020-04-14","154"]},{"value":["2020-04-15","165"]},{"value":["2020-04-16","134"]},{"value":["2020-04-17","128"]},{"value":["2020-04-18","139"]},{"value":["2020-04-19","117"]},{"value":["2020-04-20","160"]},{"value":["2020-04-21","137"]},{"value":["2020-04-22","136"]},{"value":["2020-04-23","161"]},{"value":["2020-04-24","180"]},{"value":["2020-04-25","151"]},{"value":["2020-04-26","126"]},{"value":["2020-04-27","176"]},{"value":["2020-04-28","148"]},{"value":["2020-04-29","256"]},{"value":["2020-04-30","193"]},{"value":["2020-05-01","144"]},{"value":["2020-05-02","167"]},{"value":["2020-05-03","144"]},{"value":["2020-05-04","229"]},{"value":["2020-05-05","188"]},{"value":["2020-05-06","182"]},{"value":["2020-05-07","225"]},{"value":["2020-05-08","154"]},{"value":["2020-05-09","225"]},{"value":["2020-05-10","162"]},{"value":["2020-05-11","180"]},{"value":["2020-05-12","212"]},{"value":["2020-05-13","234"]},{"value":["2020-05-14","180"]},{"value":["2020-05-15","215"]},{"value":["2020-05-16","215"]},{"value":["2020-05-17","184"]},{"value":["2020-05-18","162"]},{"value":["2020-05-19","188"]},{"value":["2020-05-20","199"]},{"value":["2020-05-21","227"]},{"value":["2020-05-22","174"]},{"value":["2020-05-23","224"]},{"value":["2020-05-24","206"]},{"value":["2020-05-25","338"]},{"value":["2020-05-26","312"]},{"value":["2020-05-27","342"]},{"value":["2020-05-28","280"]},{"value":["2020-05-29","245"]},{"value":["2020-05-30","331"]},{"value":["2020-05-31","305"]},{"value":["2020-06-01","329"]},{"value":["2020-06-02","390"]},{"value":["2020-06-03","320"]},{"value":["2020-06-04","302"]},{"value":["2020-06-05","311"]},{"value":["2020-06-06","297"]},{"value":["2020-06-07","221"]},{"value":["2020-06-08","349"]},{"value":["2020-06-09","381"]},{"value":["2020-06-10","315"]},{"value":["2020-06-11","278"]},{"value":["2020-06-12","279"]},{"value":["2020-06-13","258"]},{"value":["2020-06-14","210"]},{"value":["2020-06-15","281"]},{"value":["2020-06-16","297"]},{"value":["2020-06-17","288"]},{"value":["2020-06-18","306"]},{"value":["2020-06-19","260"]},{"value":["2020-06-20","293"]},{"value":["2020-06-21","163"]},{"value":["2020-06-22","287"]},{"value":["2020-06-23","265"]},{"value":["2020-06-24","222"]},{"value":["2020-06-25","316"]},{"value":["2020-06-26","250"]},{"value":["2020-06-27","237"]},{"value":["2020-06-28","192"]},{"value":["2020-06-29","294"]},{"value":["2020-06-30","268"]},{"value":["2020-07-01","263"]},{"value":["2020-07-02","249"]},{"value":["2020-07-03","249"]},{"value":["2020-07-04","205"]},{"value":["2020-07-05","192"]},{"value":["2020-07-06","278"]},{"value":["2020-07-07","306"]},{"value":["2020-07-08","340"]},{"value":["2020-07-09","277"]},{"value":["2020-07-10","301"]},{"value":["2020-07-11","269"]},{"value":["2020-07-12","271"]},{"value":["2020-07-13","266"]},{"value":["2020-07-14","126"]},{"value":["2020-07-15","134"]},{"value":["2020-07-16","157"]},{"value":["2020-07-17","194"]},{"value":["2020-07-18","189"]},{"value":["2020-07-19","156"]},{"value":["2020-07-20","252"]},{"value":["2020-07-21","248"]},{"value":["2020-07-22","260"]},{"value":["2020-07-23","251"]},{"value":["2020-07-24","268"]},{"value":["2020-07-25","268"]},{"value":["2020-07-26","184"]},{"value":["2020-07-27","279"]},{"value":["2020-07-28","185"]},{"value":["2020-07-29","284"]},{"value":["2020-07-30","252"]},{"value":["2020-07-31","154"]},{"value":["2020-08-01","225"]},{"value":["2020-08-02","151"]},{"value":["2020-08-03","276"]},{"value":["2020-08-04","237"]},{"value":["2020-08-05","237"]},{"value":["2020-08-06","249"]},{"value":["2020-08-07","232"]},{"value":["2020-08-08","208"]},{"value":["2020-08-09","144"]},{"value":["2020-08-10","278"]},{"value":["2020-08-11","263"]},{"value":["2020-08-12","278"]},{"value":["2020-08-13","293"]},{"value":["2020-08-14","254"]},{"value":["2020-08-15","165"]},{"value":["2020-08-16","194"]},{"value":["2020-08-17","256"]},{"value":["2020-08-18","298"]},{"value":["2020-08-19","275"]},{"value":["2020-08-20","289"]},{"value":["2020-08-21","216"]},{"value":["2020-08-22","142"]},{"value":["2020-08-23","209"]},{"value":["2020-08-24","258"]},{"value":["2020-08-25","279"]},{"value":["2020-08-26","221"]},{"value":["2020-08-27","219"]},{"value":["2020-08-28","255"]},{"value":["2020-08-29","247"]},{"value":["2020-08-30","183"]},{"value":["2020-08-31","302"]},{"value":["2020-09-01","280"]},{"value":["2020-09-02","265"]},{"value":["2020-09-03","314"]},{"value":["2020-09-04","281"]},{"value":["2020-09-05","289"]},{"value":["2020-09-06","211"]},{"value":["2020-09-07","387"]},{"value":["2020-09-08","407"]},{"value":["2020-09-09","409"]},{"value":["2020-09-10","359"]},{"value":["2020-09-11","340"]},{"value":["2020-09-12","311"]},{"value":["2020-09-13","238"]},{"value":["2020-09-14","308"]},{"value":["2020-09-15","283"]},{"value":["2020-09-16","306"]},{"value":["2020-09-17","275"]},{"value":["2020-09-18","269"]},{"value":["2020-09-19","289"]},{"value":["2020-09-20","182"]},{"value":["2020-09-21","268"]},{"value":["2020-09-22","283"]},{"value":["2020-09-23","327"]},{"value":["2020-09-24","338"]},{"value":["2020-09-25","265"]},{"value":["2020-09-26","234"]},{"value":["2020-09-27","211"]},{"value":["2020-09-28","357"]},{"value":["2020-09-29","351"]},{"value":["2020-09-30","369"]},{"value":["2020-10-01","345"]},{"value":["2020-10-02","329"]},{"value":["2020-10-03","391"]},{"value":["2020-10-04","244"]},{"value":["2020-10-05","325"]},{"value":["2020-10-06","368"]},{"value":["2020-10-07","286"]},{"value":["2020-10-08","313"]},{"value":["2020-10-09","315"]},{"value":["2020-10-10","275"]},{"value":["2020-10-11","322"]},{"value":["2020-10-12","405"]},{"value":["2020-10-13","325"]},{"value":["2020-10-14","339"]},{"value":["2020-10-15","275"]},{"value":["2020-10-16","282"]},{"value":["2020-10-17","262"]},{"value":["2020-10-18","256"]},{"value":["2020-10-19","375"]},{"value":["2020-10-20","400"]},{"value":["2020-10-21","459"]},{"value":["2020-10-22","388"]},{"value":["2020-10-23","426"]},{"value":["2020-10-24","363"]},{"value":["2020-10-25","221"]},{"value":["2020-10-26","349"]},{"value":["2020-10-27","446"]},{"value":["2020-10-28","466"]},{"value":["2020-10-29","374"]},{"value":["2020-10-30","311"]},{"value":["2020-10-31","320"]},{"value":["2020-11-01","214"]},{"value":["2020-11-02","283"]},{"value":["2020-11-03","311"]},{"value":["2020-11-04","349"]},{"value":["2020-11-05","341"]},{"value":["2020-11-06","331"]},{"value":["2020-11-07","313"]},{"value":["2020-11-08","254"]},{"value":["2020-11-09","465"]},{"value":["2020-11-10","300"]},{"value":["2020-11-11","290"]},{"value":["2020-11-12","294"]},{"value":["2020-11-13","242"]},{"value":["2020-11-14","206"]},{"value":["2020-11-15","180"]},{"value":["2020-11-16","239"]},{"value":["2020-11-17","289"]},{"value":["2020-11-18","277"]},{"value":["2020-11-19","281"]},{"value":["2020-11-20","251"]},{"value":["2020-11-21","245"]},{"value":["2020-11-22","243"]},{"value":["2020-11-23","252"]},{"value":["2020-11-24","288"]},{"value":["2020-11-25","316"]},{"value":["2020-11-26","172"]},{"value":["2020-11-27","319"]},{"value":["2020-11-28","361"]},{"value":["2020-11-29","219"]},{"value":["2020-11-30","340"]},{"value":["2020-12-01","289"]},{"value":["2020-12-02","297"]},{"value":["2020-12-03","248"]},{"value":["2020-12-04","212"]},{"value":["2020-12-05","274"]},{"value":["2020-12-06","208"]},{"value":["2020-12-07","285"]},{"value":["2020-12-08","248"]},{"value":["2020-12-09","276"]},{"value":["2020-12-10","251"]},{"value":["2020-12-11","200"]},{"value":["2020-12-12","281"]},{"value":["2020-12-13","168"]},{"value":["2020-12-14","303"]},{"value":["2020-12-15","246"]},{"value":["2020-12-16","270"]},{"value":["2020-12-17","265"]},{"value":["2020-12-18","272"]},{"value":["2020-12-19","205"]},{"value":["2020-12-20","207"]},{"value":["2020-12-21","283"]},{"value":["2020-12-22","292"]},{"value":["2020-12-23","261"]},{"value":["2020-12-24","236"]},{"value":["2020-12-25","227"]},{"value":["2020-12-26","243"]},{"value":["2020-12-27","181"]},{"value":["2020-12-28","256"]},{"value":["2020-12-29","226"]},{"value":["2020-12-30","262"]},{"value":["2020-12-31","218"]},{"value":["2021-01-01","158"]},{"value":["2021-01-02","192"]},{"value":["2021-01-03","148"]},{"value":["2021-01-04","277"]},{"value":["2021-01-05","263"]},{"value":["2021-01-06","204"]},{"value":["2021-01-07","218"]},{"value":["2021-01-08","225"]},{"value":["2021-01-09","285"]},{"value":["2021-01-10","232"]},{"value":["2021-01-11","300"]},{"value":["2021-01-12","373"]},{"value":["2021-01-13","261"]},{"value":["2021-01-14","215"]},{"value":["2021-01-15","226"]},{"value":["2021-01-16","289"]},{"value":["2021-01-17","210"]},{"value":["2021-01-18","268"]},{"value":["2021-01-19","273"]},{"value":["2021-01-20","224"]},{"value":["2021-01-21","269"]},{"value":["2021-01-22","294"]},{"value":["2021-01-23","206"]},{"value":["2021-01-24","171"]},{"value":["2021-01-25","261"]},{"value":["2021-01-26","257"]},{"value":["2021-01-27","308"]},{"value":["2021-01-28","272"]},{"value":["2021-01-29","285"]},{"value":["2021-01-30","264"]},{"value":["2021-01-31","246"]},{"value":["2021-02-01","339"]},{"value":["2021-02-02","236"]},{"value":["2021-02-03","302"]},{"value":["2021-02-04","259"]},{"value":["2021-02-05","237"]},{"value":["2021-02-06","238"]},{"value":["2021-02-07","251"]},{"value":["2021-02-08","328"]},{"value":["2021-02-09","278"]},{"value":["2021-02-10","268"]},{"value":["2021-02-11","268"]},{"value":["2021-02-12","242"]},{"value":["2021-02-13","242"]},{"value":["2021-02-14","171"]},{"value":["2021-02-15","297"]},{"value":["2021-02-16","305"]},{"value":["2021-02-17","287"]},{"value":["2021-02-18","274"]},{"value":["2021-02-19","293"]},{"value":["2021-02-20","258"]},{"value":["2021-02-21","213"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"Total","type":"line","coordinateSystem":"cartesian2d","symbol":"none"}],"title":[{"text":"Total number of grievances by day","subtext":"Data: BBMP | nathanstates.com"}],"color":["#3414b9"],"backgroundColor":"rgb(0,0,0,0)","tooltip":{"trigger":"axis","backgroundColor":"rgba(20, 20, 20, 0.5)","borderColor":"rgba(0, 0, 0, 0)","textStyle":{"color":"#ffffff"}},"textStyle":{"fontFamily":"Playfair Display"}},"dispose":true},"evals":[],"jsHooks":[]}</script>

</div>

On most days, grievances would vary between 200 to 400 a day, with some spikes in September onwards, including a massive single-day one in March of 742. On average, 280 complaints were filed each day.

### Grievances by Category <a id="number-category"></a>

<div class="most-width">

``` r
grievances %>%
  group_by(category) %>%
  summarise(Total = n()) %>%
  arrange(desc(Total)) %>%
  filter(Total > 1000) %>%
  slice(1:10) %>%
  e_charts(category) %>%
  e_bar(Total) %>%
  e_x_axis(axisLabel = list(interval = 0, rotate = 45)) %>%
  e_title(
    text = "Most common grievances by category", 
    subtext = "Only categories above 1,000 visible"
  ) %>%
  e_legend(show = FALSE) %>%
  e_labels(show = FALSE) %>%
  e_color("#0fa68c", background = "rgb(0,0,0,0)") %>%
  e_tooltip(
    backgroundColor = "rgba(20, 20, 20, 0.5)",
    borderColor = "rgba(0, 0, 0, 0)",
    textStyle = list(
      color = "#ffffff"
    )
  )
```

<div id="htmlwidget-2" style="width:100%;height:500px;" class="echarts4r html-widget"></div>
<script type="application/json" data-for="htmlwidget-2">{"x":{"theme":"echarts-theme-states.json","tl":false,"draw":true,"renderer":"canvas","events":[],"buttons":[],"opts":{"yAxis":[{"show":true}],"xAxis":[{"data":["Electrical","Solid Waste (Garbage) Related","Road Maintenance(Engg)","Forest","Animal control","Health Dept","Revenue Department","CORONA COVID19","Others"],"type":"category","boundaryGap":true,"axisLabel":{"interval":0,"rotate":45}}],"legend":{"data":["Total"],"show":false,"type":"plain"},"series":[{"data":[{"value":["Electrical","38619"]},{"value":["Solid Waste (Garbage) Related","25177"]},{"value":["Road Maintenance(Engg)","17259"]},{"value":["Forest"," 4397"]},{"value":["Animal control"," 4103"]},{"value":["Health Dept"," 3247"]},{"value":["Revenue Department"," 1323"]},{"value":["CORONA COVID19"," 1115"]},{"value":["Others"," 1069"]}],"name":"Total","type":"bar","yAxisIndex":0,"xAxisIndex":0,"coordinateSystem":"cartesian2d","label":{"show":false,"position":"top"}}],"title":[{"text":"Most common grievances by category","subtext":"Only categories above 1,000 visible"}],"color":["#0fa68c"],"backgroundColor":"rgb(0,0,0,0)","tooltip":{"trigger":"item","backgroundColor":"rgba(20, 20, 20, 0.5)","borderColor":"rgba(0, 0, 0, 0)","textStyle":{"color":"#ffffff"}},"textStyle":{"fontFamily":"Playfair Display"}},"dispose":true},"evals":[],"jsHooks":[]}</script>

</div>

81.89% of total grievances were categorized as “Electrical,” “Solid Waste (Garbage) Related,” or “Road Maintenance (Engg).” The next three largest categories make up an additional 11.09%, meaning the remaining categories have less than 1,350 occurrences combined.

Note that this is *not* due to a lack of categories; in fact, there are a total of 20 categories in the data.

``` r
length(unique(grievances$category))
```

    ## [1] 20

Large imbalances like this are important to consider when building machine learning models in general. Algorithms are specifically programmed to achieve the highest accuracy regardless of original purposes, and they tend to overestimate the presence of larger categories. If a model discovers it can restrict itself to three options while still recording 80%+ accuracy, it will almost always do so.

This means, though, machine learning models will tend to ignore minor categories, because - using our current data as an example - predicting a category outside the top three has an inherent 81.89% *fail* rate, so this will need to be addressed when creating models.

There is question to whether certain smaller categories should exist at **all**, though.

``` r
grievances %>%
  group_by(category) %>%
  summarise(total = n()) %>%
  filter(total < 100)
```

    ## # A tibble: 5 x 2
    ##   category                   total
    ##   <chr>                      <int>
    ## 1 Education                     20
    ## 2 Estate                        75
    ## 3 Markets                       38
    ## 4 Optical Fiber Cables (OFC)    62
    ## 5 Welfare Schemes               28

Five categories have less than 100 complaints total, including two which have less than thirty. This is far too few complaints to reliably build a model with.

### Grievances by Subcategory <a id="number-subcategory"></a>

<div class="most-width">

``` r
grievances %>%
  group_by(subcategory) %>%
  summarise(Total = n()) %>%
  arrange(Total) %>%
  filter(Total > 1000) %>%
  e_charts(subcategory) %>%
  e_bar(Total) %>%
  e_legend(show = FALSE) %>%
  e_title(
    text = "Most common grievances by subcategory", 
    subtext = "Only categories above 1,000 visible"
    ) %>%
  e_color("#0fa68c", background = "rgb(0,0,0,0)") %>%
  e_flip_coords() %>%
  e_tooltip(
    backgroundColor = "rgba(20, 20, 20, 0.5)",
    borderColor = "rgba(0, 0, 0, 0)",
    textStyle = list(
      color = "#ffffff"
    )
  )
```

<div id="htmlwidget-3" style="width:100%;height:500px;" class="echarts4r html-widget"></div>
<script type="application/json" data-for="htmlwidget-3">{"x":{"theme":"echarts-theme-states.json","tl":false,"draw":true,"renderer":"canvas","events":[],"buttons":[],"opts":{"xAxis":[{"show":true}],"yAxis":[{"data":["water stagnation","Footpath","Requirement For New Street Lights","Burning of Garbage in Open Space","Dead animal(s)","Road cutting","Garbage dumping in vacant sites","footpath encroachment","Others","obstructions Branches / Trees.","Removal of dead/fallen trees","Debris Removal / Construction Material","Sweeping not done","Animal birth control/neutering of stray dogs","Road side drains","Potholes","Garbage dump","Garbage vehicle not arrived","Street Light Not Working"],"type":"category","boundaryGap":true}],"legend":{"data":["Total"],"show":false,"type":"plain"},"series":[{"data":[{"value":[" 1053","water stagnation"]},{"value":[" 1085","Footpath"]},{"value":[" 1101","Requirement For New Street Lights"]},{"value":[" 1159","Burning of Garbage in Open Space"]},{"value":[" 1271","Dead animal(s)"]},{"value":[" 1439","Road cutting"]},{"value":[" 1466","Garbage dumping in vacant sites"]},{"value":[" 1581","footpath encroachment"]},{"value":[" 1658","Others"]},{"value":[" 1968","obstructions Branches / Trees."]},{"value":[" 2208","Removal of dead/fallen trees"]},{"value":[" 2263","Debris Removal / Construction Material"]},{"value":[" 2715","Sweeping not done"]},{"value":[" 2983","Animal birth control/neutering of stray dogs"]},{"value":[" 3778","Road side drains"]},{"value":[" 5642","Potholes"]},{"value":[" 6719","Garbage dump"]},{"value":[" 9857","Garbage vehicle not arrived"]},{"value":["36397","Street Light Not Working"]}],"name":"Total","type":"bar","yAxisIndex":0,"xAxisIndex":0,"coordinateSystem":"cartesian2d"}],"title":[{"text":"Most common grievances by subcategory","subtext":"Only categories above 1,000 visible"}],"color":["#0fa68c"],"backgroundColor":"rgb(0,0,0,0)","tooltip":{"trigger":"item","backgroundColor":"rgba(20, 20, 20, 0.5)","borderColor":"rgba(0, 0, 0, 0)","textStyle":{"color":"#ffffff"}},"textStyle":{"fontFamily":"Playfair Display"}},"dispose":true},"evals":[],"jsHooks":[]}</script>

</div>

While the model being built is only to predict main categories, by viewing subcategories, we see over 35% of total complaints were related specifically to street lights not working, comprising almost all of the “Electrical” category.

“Solid Waste (Garbage) Related” has been divided into two subcategories; “Garbage vehicle not arrived” and “Garbage dump,” while Road Maintenance is divided into three subcategories, being “potholes”, “road side drains”, and “debris removal”.

### Most Common Complaint Words <a id="number-words"></a>

``` r
tidy <- grievances %>%
  unnest_tokens(word, description) %>%
  anti_join(get_stopwords()) %>%
  count(word) %>%
  arrange(desc(n)) %>%
  slice(1:50)
```

    ## Joining, by = "word"

``` r
wordcloud2(
  tidy,
  color = rep_len(c("#8c5ac8", "#0b0d21", "#0a32d2"), nrow(tidy)),
  backgroundColor = "#fcfcfc"
)
```

<div id="htmlwidget-4" style="width:8.5in;height:576px;" class="wordcloud2 html-widget"></div>
<script type="application/json" data-for="htmlwidget-4">{"x":{"word":["street","light","working","road","garbage","please","days","main","vehicle","cross","water","kindly","house","bbmp","layout","near","lights","also","since","2","action","area","people","request","tree","past","waste","1","side","take","sir","complaint","last","3","one","issue","due","done","dogs","week","arrived","lot","collection","many","coming","problem","front","time","dumped","help"],"freq":[41325,34848,34697,27237,24211,11857,9258,7383,7201,6750,5914,5788,5696,5451,5344,5042,5039,4865,4818,4792,4759,4712,4683,4391,4388,4339,4238,4206,4202,4201,3952,3853,3834,3801,3769,3746,3560,3551,3530,3518,3301,3296,3115,3067,3030,2940,2850,2844,2835,2832],"fontFamily":"Segoe UI","fontWeight":"bold","color":["#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21","#0a32d2","#8c5ac8","#0b0d21"],"minSize":0,"weightFactor":0.0043557168784029,"backgroundColor":"#fcfcfc","gridSize":0,"minRotation":-0.785398163397448,"maxRotation":0.785398163397448,"shuffle":true,"rotateRatio":0.4,"shape":"circle","ellipticity":0.65,"figBase64":null,"hover":null},"evals":[],"jsHooks":{"render":[{"code":"function(el,x){\n                        console.log(123);\n                        if(!iii){\n                          window.location.reload();\n                          iii = False;\n\n                        }\n  }","data":null}]}}</script>

Seeing as over 35% of the data was subcategorized as “Street Lights Not Working,” we see that they are the most common words across all the complaitns.

Missing from that is the word “*not*,” but this is because we removed all **stop words** from the data. Put simply, stop words are words that don’t add anything to the process of categorizing text. They usually include words such as “he,” “she,” “there,” “they,” “I,” and so on, which is why they don’t appear in the word cloud.

The stop words included in the `tidytext` package (and the ones used in the function above) are meant to apply universally, but if you look closely at the word cloud, there are several words that almost certainly would not factor when classifying complaints. These include terms like “please,” “kindly”, “last request,” “sir,” and individual numbers like “1,” “2,” and “3.” While there might exist some incidental correlations between some of these words and their respective categories (perhaps citizens filing animal control complaints are nicer on average, so the term “please” could be used to identify those complaints more accurately, for example), it’s likely this will just throw off our model’s accuracy in the long run.

------------------------------------------------------------------------

From the EDA, the primary factors that should be considered when building the models are:

1.  Account for imbalances in number of occurrences per category.
2.  Reduce the number of categories by combining them into existing ones.
3.  Add additional stop words to the existing list.

## MLE Models <a id="mle-models"></a>

### Preparing the Data <a id="preparing-data"></a>

We’ll opt to use Python for creating the MLE models, as Python libraries are generally more efficient and developed than their R counterparts. Because creating models is computer intensive, the code here has been evaluated locally and presented here for demonstration.

To start, we’ll load import the necessary libraries and load the data using `pandas`. The models will be built using the `sklearn` package.

``` python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns  
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.feature_selection import chi2
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import MultinomialNB
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import LinearSVC
from sklearn.model_selection import cross_val_score
from sklearn import metrics

df = pd.read_csv("bengaluru-grievances.csv")
```

One of the packages imported here is `TfidfVectorizer`, which will be the algorithm used to create the models. I’ll explain why I specifically chose this package later on.

Here, I quickly apply the earlier data wrangling techniques by removing complaints less than 12 characters, this time using Python syntax.

``` python
# Reduce Character Limit  
character_limit = (df["description"].str.len() > 12)
df = df.loc[character_limit]
```

A model based on over 100,000 observations is **extremely** hardware intensive, and it causes my laptop to overheat a lot. For practical purposes, we’ll cut down on our data by choosing 15,000 rows at random.

``` python
df = df.sample(15000, random_state = 1).copy()
```

If we wanted to get another random 15,000 rows, we could change the `random_state = 1` argument to any other number, like 42, 671, or 7.

With no other additional changes to be made, we can begin creating the models.

### Text Preprocessing <a id="text-preprocessing"></a>

There are several methods to building models, but the simplest method is to create a new column in our data - we’ll call it “category_id” - that is a factor variable of all existing categories in our data. This essentially amounts to assigning each category a number (Electrical = 0, Road Engineering = 1, etc), which is necessary for getting our model to run properly, as `sklearn` will not understand strings as factors.

``` python
df['category_id'] = df["category"].factorize()[0]
category_id_df = df[["category", "category_id"]].drop_duplicates()
```

Next, the description column (which stores the text for grievances) needs to be converted to vectors using a chosen algorithm. The algorithm chosen here is **Term Frequency - Inverse Document Frequency** (TF-IDF), which is the product of `\(TF\)` and `\(IDF\)` scores. This is the `TfidfVectorizer` function that we imported earlier.

It’s useful to present these terms mathematically.

<br>

**Term Frequency**: $$ TF = \frac{Number \hspace{0.15cm} of \hspace{0.15cm} times \hspace{0.15cm} a \hspace{0.15cm} term \hspace{0.15cm} appears \hspace{0.15cm} in \hspace{0.15cm} the \hspace{0.15cm} description}{Total \hspace{0.15cm} number \hspace{0.15cm} of \hspace{0.15cm} words \hspace{0.15cm} in \hspace{0.15cm} the \hspace{0.15cm} description} $$

**Inverse Document Frequency**: $$ IDF = log(\frac{Number \hspace{0.15cm} of \hspace{0.15cm} rows \hspace{0.15cm} in \hspace{0.15cm} a \hspace{0.15cm} data}{Number \hspace{0.15cm} of \hspace{0.15cm} rows \hspace{0.15cm} a \hspace{0.15cm} term \hspace{0.15cm} appears \hspace{0.15cm} in \hspace{0.15cm} a \hspace{0.15cm} data}) $$

**TF-IDF**: $$ TF-IDF = TF * IDF $$

<br>

The reason for choosing this algorithm was for the `\(IDF\)` component, which **downsamples** words that appear frequently across all complaints, while adding extra weight to terms that appear less often.

Analyzing it mathematically: as the denominator of the `\(IDF\)` variable increases, the closer it rapidly (more precisely, *exponentially*) approaches zero. Let `\(N\)` represent the total number of rows in the data, and let `\(t\)` represent a chosen term. If a certain word were to appear in **every** single complaint, then we would have `\(IDF(N, t) = log(\frac{N}{t}) = log(\frac{N}{N}) = log(1) = 0\)`, which would mean that when calculating `\(TF-IDF\)`, that specific word would have absolutely no weight attached to it when classifying complaints.

Now; why do this? As discussed previously during the EDA section, almost 82% of all grievances fall into exactly three categories. Classification models will tend to stick to only a few categories, struggling to identify minor categories. While this may record higher accuracy scores on average, doing so means minor categories will rarely be classified at all, and in some instances, could lower overall accuracy if skew is significant enough. By ranking terms on an exponentially decreasing scale, we hope to reduce this issue.

We first setup our `TfidfVectorizer` and assign it to a variable, `tfidf`.

``` python
tfidf = TfidfVectorizer(
  sublinear_tf = True, # Set term frequency to logarithmic scale
  min_df = 5, # Remove terms that appear less than 'min_df' times
  ngram_range = (1, 2), # Unigrams and bigrams are considered 
  stop_words = 'english' # Use common English stop words
)
```

From here, we can begin building our models, but before doing so, let’s see what the most common terms were for each category.

To do so, we use `tfidf.get_feature_names_out()` on each category and assign that to a variable that we’ll call “feature_names.” This contains all of the most common words associated with each category, which we then split into two separate lists for unigrams and bigrams (fancy words for “one word” and “two words”). From there, we print to console the `\(N = 3\)` most common terms from each list. We wrap all this in a `for` loop, automatically progressing through each category.

``` python
# Defaults 
features = tfidf.fit_transform(df.description).toarray()
labels = df.category_id
N = 3

# For Loop
for category, category_id in sorted(category_to_id.items()):
  features_chi2 = chi2(features, labels == category_id)
  indices = np.argsort(features_chi2[0])
  feature_names = np.array(tfidf.get_feature_names_out())[indices]
  unigrams = [v for v in feature_names if len(v.split(' ')) == 1]
  bigrams = [v for v in feature_names if len(v.split(' ')) == 2]
  print("\n==> %s:" %(Product))
  print("  * Most Correlated Unigrams are: %s" %(', '.join(unigrams[-N:])))
  print("  * Most Correlated Bigrams are: %s" %(', '.join(bigrams[-N:])))
```

As this part was performed offline, here is the output in screenshots.

![Image one](term-correlation-1.png)

![Image two](term-correlation-2.png)

![Image three](term-correlation-3.png)

The results are largely what we would expect / hope for, though there are some things to note.

Some common phrases that appear for certain categories seemingly have nothing to do with them, such as the terms “77”, “kindly”, and “plz look”, which is one of the most common bigrams for both “Education” and “Welfare Schemes.” Remember, these were the categories that had less than 100 observations **total**. When we split the data to grab 15,000 random rows, these categories were split further, reducing those categories even further, which is why these nonsense phrases appear.

### Building the Models <a id="building-models"></a>

To begin, we first split the data into a 75:25 training and test split. The model will “learn” how to classify grievances based on the training data, and then it will “test” its accuracy on the remaining 25% (or about 3,750 rows).

``` python
# We define them here as independent variables 
X = df["description"]
y = df["category"]

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25, random_state = 0)
```

There are several different models to choose from, but it’s difficult to determine which will perform best before actually building them. Therefore, we’ll test several out simultaneously by storing them in a list and looping through each model.

``` python
models = [
    RandomForestClassifier(n_estimators = 100, max_depth = 5, random_state = 0),
    LinearSVC(),
    MultinomialNB(),
    LogisticRegression(random_state = 0)    
]
```

Here, we stored in a list the following:

-   Random Forest Model
-   Linear Support Vector Classifier Model
-   Multinomial Naive Bayes Model
-   Logistic Regression Model

After, we apply each model to the training data and record the results. The accuracy of each model is inherently random, as model performance is somewhat due to chance, so we’ll use a five-fold cross-validation and take the mean average of each iteration to get a more balanced result. We store the results in a `pandas` dataframe for analysis.

``` python
# Copy and pasted from before 
features = tfidf.fit_transform(df.description).toarray()
labels = df.category_id

CV = 5 # Number of cross-validations
cv_df = pd.DataFrame(index = range(CV * len(models))) # CV dataframe
entries = [] # Array for storing model results 

for model in models:
  model_name = model.__class__.__name__
  accuracies = cross_val_score(model, features, labels, scoring='accuracy', cv=CV)
  for fold_idx, accuracy in enumerate(accuracies):
    entries.append((model_name, fold_idx, accuracy))
    
cv_df = pd.DataFrame(entries, columns=['model_name', 'fold_idx', 'accuracy'])
```

### Results <a id="results"></a>

Because we used a five-fold cross-validation, we have a total of 20 accuracy results - five for each model. We grab the mean accuracy and standard deviation for each model, storing them into a list.

``` python
mean_accuracy = cv_df.groupby('model_name').accuracy.mean()
std_accuracy = cv_df.groupby('model_name').accuracy.std()

accuracy = pd.concat([mean_accuracy, std_accuracy], axis= 1, ignore_index=True)
accuracy.columns = ['Mean Accuracy', 'Standard Deviation']

accuracy
```

| Model               | Mean Accuracy | Standard Deviation |
|---------------------|---------------|--------------------|
| Linear SVC          | 88.773%       | 0.368%             |
| Logistic Regression | 87.767%       | 0.433%             |
| Multinomial NB      | 85.720%       | 0.117%             |
| Random Forest       | 66.213%       | 1.411%             |

The top three models all performed similarly as well, all falling within 3.1% percentage points. The Linear Support Vector Classifier performed the best among the three, while the Random Forest performed atrociously.+ Standard deviation among the top three remained fairly low.

## Improvements <a id="improvements"></a>

We will focus model improvement on the Linear SVC because it performed the best of the four.

As a reminder, these were the three main considerations before going in.

1.  Account for imbalances in number of occurrences per category.
2.  Consider reducing number of categories.
3.  Consider adding additional stopwords.

To get a better idea of how our model performed, we will plot a **confusion matrix**, which displays the total number of attempts our classification model made, including accuracy.

``` python
# Recreating LinearSVC Model 
X_train, X_test, y_train, y_test,indices_train,indices_test = train_test_split(features, 
                                                               labels, 
                                                               df.index, test_size=0.25, 
                                                               random_state=1)
model = LinearSVC()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

# Confusion Matrix Plot
conf_mat = confusion_matrix(y_test, y_pred)
fig, ax = plt.subplots(figsize = (8, 8))
sns.heatmap(conf_mat, annot = True, cmap = "Greens", fmt = 'd',
            xticklabels = category_id_df.category.values, 
            yticklabels = category_id_df.category.values)
plt.xlabel("Predicted")
plt.ylabel("Actual")
plt.title("Confusion Matrix for LinearSVC \n", size = 20)
```

<center>

![Heatmap](heatmap.png)

</center>

On the diagonal are the number of rows that the model correctly predicted for each category. The horizontals and verticals represent the number of incorrect guesses, with the vertical representing incorrect guesses for that specific category. For example, the model correctly classified 1,484 complaints as “Electrical,” incorrectly classified 2 as “Electrical” when they should of been classified as “COVID-19,” and classified 10 as “Solid Waste” when they should of been classified as “Electrical.”

### Modifications <a id="modifications"></a>

Looking at the chart, the top three categories dominate the total number of occurrences, comprising 2,969 rows out of 3,750 in our test data. Note that most of the incorrect predictions appear in the vertical of each of these three columns, meaning the model was incorrectly classifying complaints as them. Even though we chose an algorithm to specifically downsample those categories, our model still has a tendency to over-predict them.

A few categories have 12 or fewer observations: those being Markets, Estate, OFC, Welfare Schemes, Advertisement, Education, Town Planning, Lakes, and Parks and Playgrounds. Converting these will likely improve accuracy considering how poorly our model did at predicting them, but there isn’t clear category to merge them with. Many of these categories seem to have been falsely labeled as “Road Maintenance.” While converting these columns over to this might lead to higher accuracy, it doesn’t really make any sense in this case, and likely would hurt performance in the future.

We could reassign these variables to “Others,” but that category performed *abysmally*, only correctly predicting 3 out of 43 complaints. On one hand, moving them in there probably won’t hurt, but it likely won’t improve it either.

Lakes and Advertisements, which the model was able to predict quite a few correctly, will be left untouched. For the remaining categories under 12 test observations, they will be merged in with Others.

``` python
# Read in Data | Copy and Paste from Above
df = pd.read_csv("bengaluru-grievances.csv")
pd.DataFrame(df.category.unique()).values 
character_limit = (df["description"].str.len() > 12)
df = df.loc[character_limit]
df = df.sample(15000, random_state = 2).copy() # Select New Rows 

# Convert Columns 
df["category"] = df["category"].replace({'Markets': 'Others'})
df["category"] = df["category"].replace({'Estate': 'Others'})
df["category"] = df["category"].replace({'Welfare Schemes': 'Others'})
df["category"] = df["category"].replace({'Education': 'Others'})
df["category"] = df["category"].replace({'Town Planning': 'Others'})
```

“Optical Fiber Cables” and “Storm Water Drains,” though, are directly related to “Road Maintenance,” and if we look at the chart, that’s what the model ended up guessing for them. For these categories, it makes more sense to convert them over to “Road Maintenance” as opposed to “Others.”

``` python
df["category"] = df["category"].replace({'Optical Fiber Cables (OFC)': 'Road Maintenance(Engg)'})
df["category"] = df["category"].replace({'Storm  Water Drain(SWD)': 'Road Maintenance(Engg)'})
```

While we’ve converted the total number of categories down from 20 to 13, we’ve only changed a total of 45 test rows. Even if the improved model were able to now correctly predict all these observations, we would only see an improvement of 1.2%, certainly not insignificant, but hardly substantial.

Improving our stop word list, on the other hand, will hypothetically improve the accuracy of the model overall.

Recall earlier when we found the most common unigrams and bigrams for each category. Several terms that appeared most often had little or nothing to do with their respective grievances, and should be able to be removed while maintaining original accuracy.

The original stop word list comes from another function in `sklearn`, and already contains over 300 words. We want to keep those words while adding to it, so we will union them together in a new list and use it in `TfidfVectorizer`.

Before doing that, the last choice to decide is whether to adjust the downsampling of the model or not. This doesn’t appear to be needed, as there is a good amount of variance for both “Solid Waste (Garbage) Related” and “Road Maintenance(Engg).” Increasing downsampling will likely cause these categories to perform worse, so for this iteration, we leave it untouched.

``` python
# Import Function 
from sklearn.feature_extraction import text

# Add Stop Words 
stop_words = text.ENGLISH_STOP_WORDS.union(["please", "plz", "look", "help", "causing", "walk", "pedestrians", "kindly", "refused", "senior", "help", "one", "two", "three", "77", "1", "2", "3"])

# TfidfVectorizer 
tfidf = TfidfVectorizer(
  sublinear_tf = True, # Set term frequency to logarithmic scale
  min_df = 5, # Remove terms that appear less than 'min_df' times
  ngram_range = (1, 2), # Keep unigrams and bigrams
  stop_words = stop_words # Use custom stop words 
)
```

### Redo Text Preprocessing <a id="redo-text-preprocessing"></a>

We have to redo the text preprocessing from earlier, so this is all copy-and-paste from above.

``` python
# Copy and Paste
df['category_id'] = df["category"].factorize()[0]
category_id_df = df[["category", "category_id"]].drop_duplicates()
category_to_id = dict(category_id_df.values)
id_to_category = dict(category_id_df[["category_id", "category"]].values)

features = tfidf.fit_transform(df.description).toarray()
labels = df.category_id
X = df["description"]
y = df["category"]

X_train, X_test, y_train, y_test,indices_train,indices_test = train_test_split(
  features, 
  labels, 
  df.index, test_size = 0.25, 
  random_state = 2)
model = LinearSVC()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
```

### New Results <a id="new-results"></a>

We don’t need the complicated `for` loop from before because we only have one model this time. Therefore, we simply use `cross_val_score` as we did before and print the results to console.

``` python
accuracy_svc = cross_val_score(model, features, labels, scoring='accuracy', cv=CV)

cv_mean_accuracy_svc = accuracy_svc.mean()
cv_mean_std_svc = accuracy_svc.std()

print(cv_mean_accuracy_svc * 100)
print(cv_mean_std_svc * 100)
```

<center>

![Results](results.png)

</center>

Our new Linear SVC model was able to achieve **91.987%** accuracy with an average standard deviation of *0.282%* across five iterations. That’s an improvement of **3.214%** while also reducing variance within model performance by *0.086%*.

Another way to look at these refinements; out of a possible **3,750** complaints, our model was able to correctly predict an additional *120* complaints, going from **3,328** correct predictions to **3,449**.

<center>

![Heatmap 2](heatmap-2.png)

</center>

## Conclusions <a id="conclusions"></a>

Once again, the top three categories performed similarly as well as before. Road Maintenance was able to correctly predict an additional *63* complaints on this iteration. These three categories also continue to make up most of the incorrect predictions.

The “Others” categories once again performed dreadfully, only recording an additional two correct predictions despite even more chances.

Minor categories saw little or no improvement. The “Health Dept” was the only category that performed worse, dropping from 69.7% to 54.6% accuracy. The model incorrectly chose “Solid Waste” and “Road Maintenance” more often for “Health Dept” complaints than the previous model did, though it’s unclear as to why this is.

Increasing the stop word list seems to have improved accuracy across the board.

### Further Improvements <a id="further-improvements"></a>

-   The “Others” category needs to be transformed completely. Considering most complaints seem to be issues related to voting, creating a separate category for those specific complaints will likely improve accuracy.
-   Smaller categories continued to struggle; condensing categories even further may be appropriate.
-   This model performed much worse classifying “Health Dept” than the previous models did. Whether this is due to unintended consequences or simply bad luck isn’t clear, but more analysis is needed for this category.
-   Additional phrases can still be added to the stop word list.
