# Variation in BMI Distribution Across Different Ethnic Groups by Age

This analysis is based on the National Health and Nutrition Examination Survey (NHANES) dataset for the years 2013-2014. NHANES is a program of studies designed to assess the health and nutritional status of adults and children in the United States. 

The primary focus of this analysis is to understand the distribution of Body Mass Index (BMI) across different age groups and ethnicities.

## Libraries and Dataset

We make use of the `RNHANES` package to fetch the NHANES data and `tidyverse` for data manipulation and visualization.

```R
library(RNHANES)
library(tidyverse)
```

## Data Preparation

The datasets "DEMO_H" (Demographics) and "BMX_H" (Body Measures) for the years 2013-2014 are loaded and merged based on the sequence number (SEQN). We filter out individuals below 18 years of age and remove missing BMI values. Additionally, we rename and recode certain variables for clarity.

```R
nhanes_data = nhanes_load_data("DEMO_H", "2013-2014") %>%
  select(SEQN, cycle, RIDAGEYR, RIDRETH1, INDFMIN2) %>%
  transmute(SEQN=SEQN, wave=cycle, Age=RIDAGEYR, RIDRETH1, INDFMIN2) %>%
  left_join(nhanes_load_data("BMX_H", "2013-2014"), by="SEQN") %>%
  select(SEQN, wave, Age, RIDRETH1, INDFMIN2, BMXBMI)

dat = nhanes_data %>% 
  filter(Age > 18, !is.na(BMXBMI)) %>% 
  rename(BMI = BMXBMI) %>% 
  mutate(Race = recode_factor(RIDRETH1,
                              `1` = "Mexian American",
                              `2` = "Hispanic",
                              `3` = "Non-Hispanic, White",
                              `4` = "Non-Hispanic, Black",
                              `5` = "Others"))
```

## Data Structure
Here's a glimpse of the structure of the dataset:

```R
str(dat)
```
![str_nhanes](https://github.com/paoyingheng/nhanes/assets/44899774/4f31701e-d9fd-4105-9cc0-6f88c62bb83b)

## Data Visualization
### Heatmap: BMI Distribution by Age and Ethnicity

```R
ggplot(dat, aes(Age, Race)) +
  geom_raster(aes(fill = BMI)) + 
  labs(title="Variation in BMI Distribution Across Different Ethnic Groups by Age", x="Age (years)",
          y = "Ethnicity") +
  scale_fill_gradientn(colours=c("white", "blue"))
```
![heatmap](https://github.com/paoyingheng/nhanes/assets/44899774/71f7e0ff-fb7a-479e-ae0d-76f8c4039872)

## Key Insights
- The heatmap represents the distribution of BMI (Body Mass Index) across different age groups for various ethnicities.
- Ethnicities represented include: "Others", "Non-Hispanic, Black", "Non-Hispanic, White", "Hispanic", and "Mexican American".
- The color gradient indicates the BMI values, with darker shades representing higher BMI.
- A majority of individuals, irrespective of ethnic group, appear to have a BMI ranging between 20 and 40, which is within the typical and overweight range.
- Some data points, especially within the "Non-Hispanic, Black" group, show a darker shade, indicating a higher BMI value, potentially within the obese range.
- The data does not show a consistent trend of BMI variation with age across the ethnic groups; however, certain age brackets in some ethnicities do display elevated BMI values.

## Conclusions
In this analysis of the NHANES 2013-2014 dataset, we observed the variation of Body Mass Index (BMI) across different age groups and ethnicities. The visualizations highlighted the differences and similarities in BMI trends among various ethnic groups. It was evident that BMI values differed within certain age brackets across ethnicities. However, a consistent trend across all groups was not observed. This preliminary exploration provides insights into the diverse health profiles of the U.S. population and underscores the importance of tailored health interventions for different ethnic groups. Further in-depth studies can help in understanding the underlying factors affecting these trends, offering actionable insights for healthcare professionals and policymakers.

For more details about the dataset, visit [NHANES](https://www.cdc.gov/nchs/nhanes/).
