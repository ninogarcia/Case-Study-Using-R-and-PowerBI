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

Install and load R libraries
```r
install.packages("tidyverse")
install.packages("skimr")
install.packages("dplyr")
library(tidyverse)
library(skimr)
library(dplyr)
```
&nbsp;
&nbsp;
&nbsp;

Load Dataset
```r
attrition_data <- read_csv("...\\watson_healthcare_modified.csv")
```
&nbsp;
&nbsp;
&nbsp;

Inspect the Data Frame
```r
head(attrition_data)
glimpse(attrition_data)
View(attrition_data)
```
## PROCESS

### Data Cleaning before conducting analysis

Delete duplicate rows
```r
attrition_data_unique <- distinct(attrition_data)
```
&nbsp;
&nbsp;
&nbsp;

Check for missing values
```r
colSums(is.na(attrition_data))
```

### Adding new columns that can be used for aggregate functions

Create attrition count column
```r
attrition_data$Attrition_Count <- as.integer(ifelse(attrition_data$Attrition == "Yes", 1, 0))
```
&nbsp;
&nbsp;
&nbsp;

Calculate the sum of Attrition_Count by department
```r
dept_counts <- aggregate(Attrition_Count ~ Department, data = attrition_data, FUN = sum)
```
&nbsp;
&nbsp;
&nbsp;

Calculate the percentage of Attrition_Count by department
```r
dept_counts$Percent <- dept_counts$Attrition_Count / sum(dept_counts$Attrition_Count) * 100
```
&nbsp;
&nbsp;
&nbsp;

Create a new column 'Age_Band'
```r
attrition_data$Age_Band <- ifelse(attrition_data$Age >= 55, 'Over 55',
                                  ifelse(attrition_data$Age >= 45, '45-54',
                                         ifelse(attrition_data$Age >= 35, '35-44',
                                                ifelse(attrition_data$Age >= 25, '25-34',
                                                       'Under 25'))))
```


## ANALYZE

Let's check the attrition by Age Band
```r
summary_data1 <- attrition_data %>%
  group_by(Age_Band) %>%
  summarise(
    Attrition_Count = sum(Attrition_Count),
    EmployeeCount = n(),
  )
print(summary_data1)
```
&nbsp;
&nbsp;
&nbsp;

Let's check the attrition by Department
```r
summary_data2 <- attrition_data %>%
  group_by(Department) %>%
  summarise(
    Attrition_Count = sum(Attrition_Count),
    EmployeeCount = n(),
  )
print(summary_data2)
```
&nbsp;
&nbsp;
&nbsp;

Let's check the attrition by Education Field
```r
summary_data3 <- attrition_data %>%
  group_by(EducationField) %>%
  summarise(
    Attrition_Count = sum(Attrition_Count),
    EmployeeCount = n(),
  )
print(summary_data3)
```
&nbsp;
&nbsp;
&nbsp;

### Visualizations

Attrition by Department
```r
ggplot(dept_counts, aes(x = "", y = Attrition_Count, fill = Department)) +
  geom_bar(width = 1, stat = "identity") +
  coord_polar(theta = "y") +
  geom_text(aes(label = paste0(Attrition_Count, "\n", paste0("(", round(Percent, 2), "%", ")"))),
            position = position_stack(vjust = 0.5)) +
  labs(fill = "Department", title = "Attrition Counts by Department (Pie Chart)") +
  theme_void() +
  theme(legend.position = "right",
        axis.title = element_blank(),
        axis.text = element_blank(),
        axis.ticks = element_blank(),
        panel.grid = element_blank(),
        plot.title = element_text(hjust = 0.5, size = 18),
        legend.title = element_text(size = 12),
        legend.text = element_text(size = 10))
 ```
![alt text](https://github.com/ninogarcia/Google-Certificate-Case-Study-Using-R-and-PowerBI/raw/main/pie%20chart.png)
&nbsp;
&nbsp;
&nbsp;

* Findings: The highest attrition count is in the Maternity department with 98 employees leaving the organization, followed by Cardiology with 74 employees leaving, and then Neurology with 27 employees leaving.

&nbsp;
&nbsp;
&nbsp;

Attrition by Education Field 

```r
EducationField_sorted <- attrition_data %>%
  mutate(EducationField = factor(EducationField, 
                                 levels = c("Human Resources", "Other", "Technical Degree", "Marketing", "Medical", "Life Sciences")))
```
```r
ggplot(EducationField_sorted, aes(x = Attrition_Count, y = EducationField)) +
  geom_bar(stat = "identity") +
  labs(title = "Attrition Count by Education Field", x = "Attrition Count", y = "") +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5))
```

![alt text](https://github.com/ninogarcia/Google-Certificate-Case-Study-Using-R-and-PowerBI/raw/main/Attrition%20by%20Education%20Field.png)

* Findings: The Education Field with the highest attrition count is Life Sciences with 84 employees leaving the organization, followed by Medical with 51 employees leaving, and then Marketing with 28 employees leaving. Among the Education Fields, Human Resources has the lowest attrition count with only 6 employees leaving, followed by Other with 8 employees leaving, and then Technical Degree with 22 employees leaving.
