# London Bikeshare Portfolio Project
 
---
title: "Portfolio Project 2"
subtitle: "London Bike Sharing 2015"
author: "Arav Pradhan"
date: "2023-05-19"
output: 
  html_document: 
    toc: yes
---
[Kaggle dataset](https://www.kaggle.com/datasets/hmavrodiev/london-bike-sharing-dataset?resource=download) sourced from:

* [https://cycling.data.tfl.gov.uk/](https://cycling.data.tfl.gov.uk/) 'Contains OS data © Crown copyright and database rights 2016' and Geomni UK Map data © and database rights [2019] 'Powered by TfL Open Data'

* freemeteo.com - weather data

* https://www.gov.uk/bank-holidays From 1/1/2015 to 31/12/2016

## Introduction
Bike sharing data in London. This project aims to investigate new insights pertaining to the future of bike sharing and visualize them in an easy and aesthetic way to showcase to the audience. The project is to be completed in R and Tableau, with R being used for data cleaning and wrangling, and Tableau for visualization which will be linked into this Markdown document.

## Data cleaning and wrangling

To start, the directory is set.

```{r}
setwd("C:/Users/aravp/Documents/Portfolio Project 2")
getwd()
my_dir <- getwd()
```

Run required packages.
```{r}
Packages <- c("tidyverse", "ggplot2", "dplyr", "writexl")
Packages %in% loadedNamespaces() # should return all as FALSE

```
```{r}
pacman::p_load(Packages, character.only = TRUE)
Packages %in% loadedNamespaces() #should return all as TRUE
```


Preliminary exploration of data
```{r}
bikeshare_london <- read.csv("C:/Users/aravp/Documents/Portfolio Project 2/raw data/unzipped/london_merged.csv")
head(bikeshare_london)
row_and_col <- c(nrow(bikeshare_london), ncol((bikeshare_london)))
row_and_col
```
There are 17414 rows and 10 columns in the data frame. A dplyr function must be used to see all the unique values in each column
```{r}
sapply(bikeshare_london, function(x) n_distinct(x))
table(bikeshare_london$weather_code)
```
`table(bikeshare_london$weather_code)` was used to identify the counts of each unique value in the column `weather_code` and displayed to the corresponding unique value since the `sapply` function did not display the unique values. 

```{r}
table(bikeshare_london$season)
```
The column names are renamed to names which make sense in the context
```{r}
bikeshare_london <- bikeshare_london %>%
  select(1:10) %>%
  set_names(c("time", "count", "real_temp_C", "feels_like_C", "humidity","wind_speed_kmph", "weather", "is_holiday", "is_weekend", "season"))
head(bikeshare_london)
```
The numeric values of certain columns such as weather and season can be better represented by strings describing the weather and season.
```{r}
unique(bikeshare_london$weather)
bikeshare_london$weather[bikeshare_london$weather == 1] <- "Clear"
bikeshare_london$weather[bikeshare_london$weather == 2] <- "Scattered Clouds"
bikeshare_london$weather[bikeshare_london$weather == 3] <- "Broken Clouds"
bikeshare_london$weather[bikeshare_london$weather == 4] <- "Cloudy"
bikeshare_london$weather[bikeshare_london$weather == 7] <- "Rain"
bikeshare_london$weather[bikeshare_london$weather == 10] <- "Rain and thunderstorms"
bikeshare_london$weather[bikeshare_london$weather == 26] <- "Snowfall"

unique(bikeshare_london$weather)
```
The same assessments are applied to the season column.
```{r}
unique(bikeshare_london$season)
bikeshare_london$season[bikeshare_london$season == 0] <- "Spring"
bikeshare_london$season[bikeshare_london$season == 1] <- "Summer"
bikeshare_london$season[bikeshare_london$season == 2] <- "Autumn"
bikeshare_london$season[bikeshare_london$season == 3] <- "Winter"
unique(bikeshare_london$season)

```
## Export final data
It is now time to export the data into an excel file for visualization in Tableau. `writexl` is the package used to complete this function.
```{r}
write_xlsx(bikeshare_london, "C:/Users/aravp/Documents/Portfolio Project 2/bikeshare_london_final.xlsx")
```





