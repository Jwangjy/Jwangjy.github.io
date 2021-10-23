---
title: "Interactive Maps in R - An analysis of historical minimum wage values for US states"
categories:
  - Projects
tags:
  - R
  - Visualization
excerpt: "Small project on data visualization through interactive presentations in R" 
header:
  overlay_image: /assets/images/posts/interactive maps/interactive-map-bg.jpg
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Pexels**](https://www.pexels.com/)"
---

## Table of Contents
1. [Introduction](#introduction)
2. [Summary](#summary)
3. [Data Cleaning](#data-cleaning)
4. [Analysis](#analysis)
5. [Challenges](#challenges)
6. [Conclusion](#conclusion)

# Introduction

Data visualization is key in any kind of data presentation. I wanted to do a series of small projects to practice different visualization methods and tools. This specific project was inspired by [DatasliceYT](https://www.youtube.com/watch?v=RrtqBYLf404) who created a similar visualization.

# Summary

The topic of choice for this visualisation is the minimum wage in each US state. The recent pandemic has really highlighted a discrepancy between what we classify as essential work versus well compensated work, and has brought discussions on the minimum wage to the forefront of many people's minds. I will be looking to present each state's minimum wage from 1968 to 2020 in an interactive map. I will also be conducting further analysis on each state's inflation adjusted cost of living versus minimum wage growth.

# Data cleaning

The data was taken from the Department of Labor's [site](https://www.dol.gov/agencies/whd/state/minimum-wage/history) and [Kaggle](https://www.kaggle.com/lislejoem/us-minimum-wage-by-state-from-1968-to-2017). Since plotly only recognizes US states based on their two letter State code, a secondary data set was downloaded to transform the State names to their State code via an inner join between the two data sets.

# Analysis

For the analysis, I will only be presenting certain segments of the code used. The full code is available on the github page for the [project](https://github.com/Jwangjy/Interactive-Map-Min-Wage).

## Nominal Wage Changes from 1968 - 2020

Libraries used:

```r
library(plotly)
library(dplyr)
library(readr)
library(tidyverse)
library(htmlwidgets)
```

<iframe src="/assets/images/posts/interactive maps/nominal_minwage.html" height="500px" width="100%"></iframe>

As shown from the graph, nominal minimum wages have increased in almost all US states from 1968 to 2020. However, the real buying power of current minimal wage in comparison to 1968 minimum wage may not have increased given inflation through the years.

## Real Wage Percent Changes from 1968 - 2020

While our data set includes information on real wages, I recalculated it separately in order to practice data transformation in R. From the Department of Labor's data, we can calculate the 1968 value of 2020 minimum wage:

```r
#2020 CPI: 258.66
#1968 CPI: 34.80
#CPI Factor: 258.66/34.80 = 7.43
#2020 Federal Min Wage: 7.25
#2020 Federal Min Wage in 1968 Dollars: 7.25/7.43 = 0.975
#1968 Federal Min Wage: 1.15
```

Calculation code to transform our data and mutate hover labels for the interactive graphs. For all states, if they do not have a state defined minimum wage for either 1968 or 2020, the federal minimum wage was used instead.

```r
allwage_df = read.csv("minimum wage data.csv") %>%
  rename(high_value = Department.Of.Labor.Cleaned.High.Value)
relwage2020_df = subset(allwage_df, Year== 2020) %>%
  mutate(State=toupper(State)) %>%
  inner_join(states, by.x = State, by.y = State) %>%
  select(Code, State, Wage = high_value, FedWage = Federal.Minimum.Wage)

relwage2020_df$EffWage2020 <- apply(relwage2020_df[,3:4],1,max)
relwage2020_df$EffWage2020Real <- relwage2020_df$EffWage2020 / CPIFactor

relwage1968_df = subset(allwage_df, Year== 1968) %>%
  mutate(State=toupper(State)) %>%
  inner_join(states, by.x = State, by.y = State) %>%
  select(Code, State, Wage = high_value, FedWage = Federal.Minimum.Wage)

relwage1968_df$EffWage1968Real <- apply(relwage1968_df[,3:4],1,max)

wagecomp_df <- merge(relwage2020_df,relwage1968_df,by=c("State","Code")) %>%
  select(Code, State, EffWage1968Real, EffWage2020Real)

wagecomp_df$Wage.Pct.Change = (wagecomp_df$EffWage2020Real - wagecomp_df$EffWage1968Real) / wagecomp_df$EffWage1968Real * 100

wagecomp_df$wagechangeround <- lapply(wagecomp_df$Wage.Pct.Change, round, 2)
wagecomp_df$hover2 = paste0(wagecomp_df$State, "\n", wagecomp_df$wagechangeround, "%")  
```

<iframe src="/assets/images/posts/interactive maps/relchange_minwage.html" height="500px" width="100%"></iframe>

We can see from the data that relatively few US states have seen a real increase in minimum wage from 1968 to 2020. Since a number of the states do not have state defined minimum wages and calculations were done for them using Federal minimum wage numbers, we can also adjust for the percent change in real Federal minimum wage as a baseline. We know from earlier calculations that the percent change in real Federal minimum wage is -15.2%.

<iframe src="/assets/images/posts/interactive maps/relchangefed_minwage.html" height="500px" width="100%"></iframe>

As shown, roughly half of the US states have failed to outperform the Federal government in increasing their minimum wage levels. Even the Federal minimum wage has failed to adjust fully for the inflation seen during this timeframe. 

# Challenges

It was a very interesting learning experience working with plotly and creating these interactive graphs. I enjoyed the process of data transformation as well in calculating the real wage numbers. The process highlighted to me the importance of having the technical know-how to get the data that I needed, as my original attempts to scrape the data from the Department of Labor's website directly did not go well. I was lucky to find cleaned data on Kaggle that worked for this project. Data scrapping will definitely be a skill I look to improve on in future projects. 

The project also only covered a small portion of the capabilities of plotly. I am definitely interested in exploring other visualization techniques in plotly in the future.

# Conclusion

From this project, we can see that the minimum wage level has fallen far below where it used to be in real terms for most US states and at the Federal level. In order for the 2020 Federal minimum wage to match the 1968 Federal minimum wage in buying power, it would need to be $8.55. 

We also see that the minimum wage has lagged behind US economic growth to a large extent. From the [World Bank](https://data.worldbank.org/indicator/NY.GDP.MKTP.CD?locations=US), US nominal GDP in 1968 was $0.9425Tn, and 2019 nominal GDP was $21.433Tn, an increase of over 2000%. In order for the minimum wage change to match this economic growth across, the Federal minimum wage would need to be $29.19 in 2019. 

This highlights the urgent need to revisit minimum wage levels, which has been a topic of contention in recent months. One potential solution that has been brought up could be a universal basic income. This could be calibrated such that minimum wage workers have a combined monthly income that at the very least guarantees them a buying power equal to their 1968 peers. However, more work would need to be done to analyze the economic impact and resources needed to implement this. One such experiment is being conducted in [New Jersey](https://www.nbcnewyork.com/news/local/njs-largest-city-launches-6000-guaranteed-income-pilot-program/3046337/) and could potentially be the start of universal income experiements across more US states.

