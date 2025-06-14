# 💼 Project : Carbon Emission Analysis

## 1. Introduction

### What is the project about?

> ![Photo by Swiss](https://lms.swisscoding.edu.vn/pluginfile.php/22102/mod_label/intro/cover.jpg)

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

---
### 🎯 Objectives

The key objectives of this project are:

- To identify the **products** and **industries** with the highest carbon emissions.
- To determine the **countries** and **companies** that contribute most significantly to global emissions.
- To observe **emission trends over time** and detect which **industries have improved** or worsened.
- To uncover **insights and patterns** that could guide sustainability practices or regulatory recommendations.

---
### 📊 Data Source: Where Our Data Comes From

Our dataset is compiled from publicly available data from [nature.com](https://www.nature.com/) and encompasses the Product Carbon Footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO₂ (carbon dioxide equivalent).

---

### 📁 Data Structure
# 💼 Project : Carbon Emission Analysis

## 1. Introduction

### What is the project about?

> ![Photo by Swiss](https://lms.swisscoding.edu.vn/pluginfile.php/22102/mod_label/intro/cover.jpg)

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

---
### 🎯 Objectives

The key objectives of this project are:

- To identify the **products** and **industries** with the highest carbon emissions.
- To determine the **countries** and **companies** that contribute most significantly to global emissions.
- To observe **emission trends over time** and detect which **industries have improved** or worsened.
- To uncover **insights and patterns** that could guide sustainability practices or regulatory recommendations.

---
### 📊 Data Source: Where Our Data Comes From

Our dataset is compiled from publicly available data from [nature.com](https://www.nature.com/) and encompasses the Product Carbon Footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO₂ (carbon dioxide equivalent).

---

### 📁 Data Structure

The dataset consists of **4 tables** containing information regarding carbon emissions generated during the production of goods.

---

## 📋 Tables and Their Description

### Table: `product_emissions`

| Column Name                    | Description                                                                 |
|-------------------------------|-----------------------------------------------------------------------------|
| `id`                          | Identifier for each product emission record                                |
| `company_id`                  | Identifier for the company associated with the product                     |
| `country_id`                  | Identifier for the country where the product is being produced             |
| `industry_group_id`           | Identifier for the industry group to which the product belongs             |
| `year`                        | The year in which the emissions data was recorded                          |
| `product_name`                | The name of the product                                                    |
| `weight_kg`                   | The weight of the product in kilograms                                     |
| `carbon_footprint_pcf`       | Carbon footprint of the product (CO₂ equivalent)                           |
| `upstream_percent_total_pcf` | % of footprint from upstream activities                                    |
| `operations_percent_total_pcf` | % of footprint from operational activities                                |
| `downstream_percent_total_pcf` | % of footprint from downstream activities                                 |

---

### Table: `industry_groups`

| Column Name        | Description                                   |
|--------------------|-----------------------------------------------|
| `id`               | Unique identifier for each industry group     |
| `industry_group`   | Name of the industry group                    |

---

### Table: `companies`

| Column Name     | Description                         |
|-----------------|-------------------------------------|
| `id`            | Unique identifier for each company  |
| `company_name`  | Name of the company                 |

---

### Table: `countries`

| Column Name     | Description                        |
|-----------------|------------------------------------|
| `id`            | Unique identifier for each country |
| `country_name`  | Name of the country_

## 2. Project Brief

This project focuses on analyzing carbon emissions data in order to identify:

- The products and industries with the highest carbon footprints
- The countries and companies contributing most to global emissions
- Carbon footprint trends over time
- Industry groups that have shown significant improvement or decline

The final report includes SQL queries, query results, explanations, and key insights based on database responses.

---

### 🧪 Initial Data Exploration

To begin the analysis, we preview each table by selecting the first 5 rows.

---

#### 🔍 Table: `product_emissions`

```sql
SELECT * 
FROM product_emissions 
LIMIT 5;
```
#####Ouput:
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 
---
## 3. Preprocessing

Before conducting any analysis, we need to ensure the dataset is clean by checking for duplicates and missing values (NULLs).

### 🔁 3.1 Checking for Duplicate Records

To check for duplicates in the `product_emissions` table:

```sql
SELECT 
	id,
	company_id,
	country_id,
	industry_group_id,
	year,
COUNT(*) AS douplicate_count
FROM 
    product_emissions
GROUP BY 
   	id,
 	company_id,
	country_id,
	industry_group_id,
	year
HAVING 
    COUNT(*) > 1
Limit 5;
```
###Ouput:
| id           | company_id | country_id | industry_group_id | year | douplicate_count | 
| -----------: | ---------: | ---------: | ----------------: | ---: | ---------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | 2                | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | 2                | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | 2                | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | 2                | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | 2                | 
---
##I found duplicate values but I can not delete this data, so the results of this article are only relative.
---
## 📊 4. Data Analysis & Insights

### 4.1 Which products contribute the most to carbon emissions?

#### 🧾 SQL Query:

```sql
SELECT 
    product_name,
    ROUND(carbon_footprint_pcf, 2) AS carbon_footprint_pcf
FROM 
    product_emissions
ORDER BY 
    carbon_footprint_pcf DESC
LIMIT 10;
```
