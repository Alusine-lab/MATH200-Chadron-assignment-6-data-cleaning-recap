# MATH200-Chadron-assignment-6-data-cleaning-recap
# Assignment 6 - Data Cleaning Recap

This repository contains my work for Assignment 6 in MATH200 at Chadron State College.  
The goal of this assignment is to apply data-cleaning techniques using R.  

## **What This Project Covers**
- Importing and cleaning the dataset.
- Performing transformations and handling missing values.
- Exploring different approaches to complete the "Your Turn" task.
- Possibly creating visualizations for better data insights.
- Documenting findings and pushing changes to GitHub.

All code is stored in `assignment-6.Rmd`.

---
title: "1870 Census Occupation"
author: "Alusine Samura"
date: "2025-02-27"
output: html_document
---




## Task Description

In this task, we will be working with the 1870 Census Data on people's occupation, which contains state-level aggregates of occupation by gender. The goal is to:

- Use `tidyr` to reshape the data into a long format.
- Separate the `occupation.gender` variable into two variables: `occupation` and `gender`.
- Create scatter plots to compare the values for men against women, faceted by occupation.



```{r
# Load necessary libraries
library(tidyr)
library(dplyr)
library(ggplot2)

# Load the dataset (adjust the path if needed)
data <- read.csv("occupation-1870.csv", stringsAsFactors = FALSE)

# Check the first few rows of the data
head(data)

# Check column names to ensure correct data loading
colnames(data)

# Dynamically select columns corresponding to male and female counts
cols <- grep("Male|Female", colnames(data), value = TRUE)

# Gather the data into a long format
data_long <- data %>%
  gather(key = "occupation.gender", value = "count", cols)

# Separate the "occupation.gender" column into occupation and gender
data_long <- data_long %>%
  separate(occupation.gender, into = c("occupation", "gender"), sep = "\\.")  # Split at the period

# View the first few rows after separation
head(data_long)

# Spread the data such that gender is a separate column
data_wide <- data_long %>%
  spread(key = gender, value = count)

# View the wide-format data to ensure it looks correct
head(data_wide)

# Now, create the scatter plot comparing men's and women's counts, facetted by occupation
ggplot(data_wide, aes(x = Male, y = Female, color = occupation)) +
  geom_point() +
  facet_wrap(~ occupation) +
  labs(title = "Occupation by Gender in the 1870 Census",
       x = "Men's Count", y = "Women's Count") +
  theme_minimal() +
  theme(legend.position = "none")

```


