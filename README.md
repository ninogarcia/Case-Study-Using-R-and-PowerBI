# Google Certificate Case Study

## Introduction
This exploratory analysis case study is towards Capstome project requirement for Google Data Analytics Professional Certificate. The case study involves a Dataset of Employee Attrition for Healthcare.

The analysis will follow the 6 phases of the Data Analysis process: Ask, Prepare, Process, Analyze, and Act (APPAA).

## ASK

Task: Analyze employee attrition rates by department, educational field, age group, and gender to identify patterns and develop retention strategies.

Description: To address the issue of employee attrition, we need to analyze the attrition rates of each department, educational field, age group, and gender to identify patterns and develop targeted retention strategies.

These are the questions that needs to be answered.

* What is the attrition rate of each department?
* What is the attrition rate based on educational field?
* What is the number of employees by age group?
* What is the attrition rate by gender for different age groups?

## PREPARE

### Data Source

ABOUT THE DATASET:

This dataset contains employee and company data useful for the analysis. Attrition - whether an employee left or not - is included and can be used as the target variable. The data is synthetic and based on the IBM Watson dataset for attrition. Employee roles and departments were changed to reflect the healthcare domain. 

The Dataset is from [Employee Attrition for Healthcare](https://www.kaggle.com/datasets/jpmiller/employee-attrition-for-healthcare). The data has been made available by [John Miller](https://www.kaggle.com/jpmiller) under CC0: Public Domain license.

### Documentation, Cleaning and Preparation of data for analysis

R Libraries that I am going to use in this project
```r
install.packages("tidyverse")
install.packages("skimr")
install.packages("dplyr")
library(tidyverse)
library(skimr)
library(dplyr)
```
