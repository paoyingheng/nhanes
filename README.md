# Variation in BMI Distribution Across Different Ethnic Groups by Age

This analysis is based on the NHANES (National Health and Nutrition Examination Survey) dataset for the years 2013-2014. The primary focus is to understand the distribution of Body Mass Index (BMI) across different age groups and ethnicities.

## Libraries and Dataset

We make use of the `RNHANES` package to fetch the NHANES data and the `tidyverse` suite for data manipulation and visualization.

```R
library(RNHANES)
library(tidyverse)
Data Preparation
The datasets "DEMO_H" (Demographics) and "BMX_H" (Body Measures) for the years 2013-2014 are loaded and merged based on the sequence number (SEQN). We filter out individuals below 18 years of age and remove missing BMI values. Additionally, we rename and recode certain variables for clarity.

R
Copy code
# ...[Include the data loading and manipulation code here]
Data Structure
Here's a glimpse of the structure of the dataset:

R
Copy code
str(dat)
Data Visualization
Line Plot: Variation of BMI by Age and Ethnicity
R
Copy code
ggplot(dat, aes(x = Age, y = BMI)) + 
  geom_line(aes(color = Race))
Line plot of BMI by Age and Ethnicity

Line Plot with Facets: Variation of BMI by Age for Each Ethnicity
R
Copy code
ggplot(dat, aes(x = Age, y = BMI)) + 
  geom_line(aes(color = Race)) +
  facet_wrap(~Race) 
Line plot with facets

Heatmap: BMI Distribution by Age and Ethnicity
R
Copy code
ggplot(dat, aes(Age, Race)) +
  geom_raster(aes(fill = BMI)) + 
  labs(title="Variation in BMI Distribution Across Different Ethnic Groups by Age", x="Age (years)",
          y = "Ethnicity") +
  scale_fill_gradientn(colours=c("white", "red"))
Heatmap of BMI by Age and Ethnicity

Key Insights
The heatmap represents the distribution of BMI (Body Mass Index) across different age groups for various ethnicities.
Ethnicities represented include: "Others", "Non-Hispanic, Black", "Non-Hispanic, White", "Hispanic", and "Mexican American".
The color gradient indicates the BMI values, with darker shades representing higher BMI.
A majority of individuals, irrespective of ethnic group, appear to have a BMI ranging between 20 and 40, which is within the typical and overweight range.
Some data points, especially within the "Non-Hispanic, Black" group, show a darker shade, indicating a higher BMI value, potentially within the obese range.
The data does not show a consistent trend of BMI variation with age across the ethnic groups; however, certain age brackets in some ethnicities do display elevated BMI values.
