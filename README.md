
---

### 📁 Data Structure
# 💼 Project : Carbon Emission Analysis

## 1. Introduction

### What is the project about?

> ![Cover Image](https://github.com/ctamdz/Carbon-Emission-Analysis/blob/main/cover.jpg?raw=true)


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
##### Ouput:
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
### Ouput:
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
####> ⚠️ Note: The dataset contains duplicate product entries (e.g., "TCDE").  
> To ensure accurate ranking, we applied a `GROUP BY` clause and selected the **maximum** carbon footprint for each product.


```sql
SELECT 
    product_name,
    ROUND(MAX(carbon_footprint_pcf), 2) AS carbon_footprint_pcf
FROM 
    product_emissions
GROUP BY 
    product_name
ORDER BY 
    carbon_footprint_pcf DESC
LIMIT 10;

```
### Ouput:
| product_name                                                                                                                       | carbon_footprint_pcf | 
| ---------------------------------------------------------------------------------------------------------------------------------: | -------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044              | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187              | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608              | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625              | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687               | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000               | 
| TCDE                                                                                                                               | 99075                | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000                | 
| Electric Motor                                                                                                                     | 87589                | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000                | 
> ![products contribute t](https://github.com/ctamdz/Carbon-Emission-Analysis/blob/main/carbon_footprint_pcf%20vs.%20product_name.png?raw=true)
## 🧾 Conclusion
### 🔍 Key Findings:

- **Wind turbines**, while central to renewable energy production, ranked highest in production-related emissions. This highlights the **environmental cost of manufacturing** large-scale infrastructure, even when intended for green energy.
- **Automobiles**, particularly large and luxury models such as the Land Cruiser and Mercedes-Benz GLE, consistently appear in the top carbon-emitting products.
- **Industrial equipment and infrastructure**, like retaining wall systems and electric motors, also account for significant emissions, largely due to raw material usage and energy-intensive processes.

### 4.2 What are the industry groups of these products?
#### 🧾 SQL Query:
```sql
SELECT 
    product_name,
    MAX(ROUND(carbon_footprint_pcf, 2)) AS carbon_footprint_pcf,
    ig.industry_group
FROM 
    product_emissions pe
JOIN 
    industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY 
    product_name, ig.industry_group
ORDER BY 
    carbon_footprint_pcf DESC
LIMIT 10;

```
### Output: 
| product_name                                                                                                                       | carbon_footprint_pcf | industry_group                     | 
| ---------------------------------------------------------------------------------------------------------------------------------: | -------------------: | ---------------------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044              | Electrical Equipment and Machinery | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187              | Electrical Equipment and Machinery | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608              | Electrical Equipment and Machinery | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625              | Electrical Equipment and Machinery | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687               | Automobiles & Components           | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000               | Materials                          | 
| TCDE                                                                                                                               | 99075                | Materials                          | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000                | Automobiles & Components           | 
| Electric Motor                                                                                                                     | 87589                | Capital Goods                      | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000                | Automobiles & Components           | 

### 📘 Conclusion – Industry Groups of the Highest Carbon-Emitting Products

After eliminating duplicate product entries and grouping by `product_name`, the analysis revealed the following insights about the **industry groups** associated with the top carbon-emitting products:

### 🔍 Key Observations:

- The **Electrical Equipment and Machinery** sector dominates the top 4 positions with wind turbine models, each emitting over 1 million CO₂e units.  
  This reflects the **heavy manufacturing requirements** and **complex materials** used in renewable energy infrastructure.
  
- The **Automobiles & Components** industry appears **three times**, represented by well-known vehicle models such as the **Land Cruiser Prado**, **Mercedes-Benz GLE**, and **S-Class**.  
  These high-end vehicles have significant footprints, reinforcing concerns about the **environmental cost of automotive luxury**.
  
  ### 4.3 What are the industries with the highest contribution to carbon emissions?

#### 🧾 SQL Query:
```sql
SELECT 
    ig.industry_group,
    ROUND(SUM(pe.carbon_footprint_pcf), 2) AS total_carbon_emissions
FROM 
    product_emissions pe
JOIN 
    industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY 
    ig.industry_group
ORDER BY 
    total_carbon_emissions DESC
LIMIT 10;

```
### Output
| industry_group                                   | total_carbon_emissions | 
| -----------------------------------------------: | ---------------------: | 
| Electrical Equipment and Machinery               | 9801558.00             | 
| Automobiles & Components                         | 2582264.00             | 
| Materials                                        | 577595.00              | 
| Technology Hardware & Equipment                  | 363776.00              | 
| Capital Goods                                    | 258712.00              | 
| "Food, Beverage & Tobacco"                       | 111131.00              | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486.00               | 
| Chemicals                                        | 62369.00               | 
| Software & Services                              | 46544.00               | 
| Media                                            | 23017.00               | 

### 4.4 What are the companies with the highest contribution to carbon emissions?
#### 🧾 SQL Query:
```sql
SELECT 
    company_name,
    ROUND(SUM(pe.carbon_footprint_pcf), 2) AS total_carbon_emissions
FROM 
    product_emissions pe
JOIN 
    companies company ON pe.company_id = company.id
GROUP BY 
    company_name
ORDER BY 
    total_carbon_emissions DESC
LIMIT 10;

```
### Output :
| company_name                            | total_carbon_emissions | 
| --------------------------------------: | ---------------------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464.00             | 
| Daimler AG                              | 1594300.00             | 
| Volkswagen AG                           | 655960.00              | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016.00              | 
| "Hino Motors, Ltd."                     | 191687.00              | 
| Arcelor Mittal                          | 167007.00              | 
| Weg S/A                                 | 160655.00              | 
| General Motors Company                  | 137007.00              | 
| "Lexmark International, Inc."           | 132012.00              | 
| "Daikin Industries, Ltd."               | 105600.00              | 

### 4.5 What are the countries with the highest contribution to carbon emissions?
#### 🧾 SQL Query:
```sql
SELECT 
    country.country_name,
    ROUND(SUM(pe.carbon_footprint_pcf), 2) AS total_carbon_emissions
FROM 
    product_emissions pe
JOIN 
    countries country ON pe.country_id = country.id
GROUP BY 
    country.country_name
ORDER BY 
    total_carbon_emissions DESC
LIMIT 10;

```
### Output:
| country_name | total_carbon_emissions | 
| -----------: | ---------------------: | 
| Spain        | 9786130.00             | 
| Germany      | 2251225.00             | 
| Japan        | 653237.00              | 
| USA          | 518381.00              | 
| South Korea  | 186965.00              | 
| Brazil       | 169337.00              | 
| Luxembourg   | 167007.00              | 
| Netherlands  | 70417.00               | 
| Taiwan       | 62875.00               | 
| India        | 24574.00               | 

### 4.6 What is the trend of carbon footprints (PCFs) over the years?
#### 🧾 SQL Query:
```sql
SELECT 
    year,
    ROUND(avg(carbon_footprint_pcf), 2) AS avg_emissions,
	ROUND(sum(carbon_footprint_pcf), 2) AS total_emissions
FROM 
    product_emissions
GROUP BY 
    year
ORDER BY 
    year;

```
### Ouput:
| year | avg_emissions | total_emissions | 
| ---: | ------------: | --------------: | 
| 2013 | 2399.32       | 503857.00       | 
| 2014 | 2457.58       | 624226.00       | 
| 2015 | 43188.90      | 10840415.00     | 
| 2016 | 6891.52       | 1640182.00      | 
| 2017 | 4050.85       | 340271.00       | 
> ![sum](https://github.com/ctamdz/Carbon-Emission-Analysis/blob/main/total_emissions%20vs.%20year.png?raw=true)

> ![AVG](https://github.com/ctamdz/Carbon-Emission-Analysis/blob/main/avg_emissions%20vs.%20year.png?raw=true)
### 📘 Conclusion – Carbon Footprint Trends Over the Years

Based on both total and average emissions between 2013 and 2017, the analysis reveals **significant variation** in the carbon footprint patterns across the years.

### 🔍 Key Observations:

- 📈 **2015 marked the highest peak** in total carbon emissions (~10.8 million CO₂e) and also had the **highest average emissions per product** (~43,189 CO₂e).
- 📉 In contrast, 2013, 2014, and 2017 showed **significantly lower total and average emissions**, suggesting either fewer high-impact products or more efficient production methods in those years.
- **2016** presents an interesting transition, where total emissions remained high (~1.64 million CO₂e) but **average per-product emissions dropped** compared to 2015 — indicating broader production but potentially improved efficiency.

### 4.7 Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
 
#### 🧾 SQL Query:
```sql
SELECT 
    ig.industry_group,
    pe.year,
    ROUND(SUM(pe.carbon_footprint_pcf), 2) AS total_emissions
FROM 
    product_emissions pe
JOIN 
    industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY 
    ig.industry_group, pe.year
ORDER BY 
    ig.industry_group, pe.year;

```
### Output:
| industry_group                                                         | year | total_emissions | 
| ---------------------------------------------------------------------: | ---: | --------------: | 
| "Consumer Durables, Household and Personal Products"                   | 2015 | 931.00          | 
| "Food, Beverage & Tobacco"                                             | 2013 | 4995.00         | 
| "Food, Beverage & Tobacco"                                             | 2014 | 2685.00         | 
| "Food, Beverage & Tobacco"                                             | 2015 | 0.00            | 
| "Food, Beverage & Tobacco"                                             | 2016 | 100289.00       | 
| "Food, Beverage & Tobacco"                                             | 2017 | 3162.00         | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 2015 | 8909.00         | 
| "Mining - Iron, Aluminum, Other Metals"                                | 2015 | 8181.00         | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2013 | 32271.00        | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2014 | 40215.00        | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 2015 | 387.00          | 
| Automobiles & Components                                               | 2013 | 130189.00       | 
| Automobiles & Components                                               | 2014 | 230015.00       | 
| Automobiles & Components                                               | 2015 | 817227.00       | 
| Automobiles & Components                                               | 2016 | 1404833.00      | 
| Capital Goods                                                          | 2013 | 60190.00        | 
| Capital Goods                                                          | 2014 | 93699.00        | 
| Capital Goods                                                          | 2015 | 3505.00         | 
| Capital Goods                                                          | 2016 | 6369.00         | 
| Capital Goods                                                          | 2017 | 94949.00        | 
| Chemicals                                                              | 2015 | 62369.00        | 
| Commercial & Professional Services                                     | 2013 | 1157.00         | 
| Commercial & Professional Services                                     | 2014 | 477.00          | 
| Commercial & Professional Services                                     | 2016 | 2890.00         | 
| Commercial & Professional Services                                     | 2017 | 741.00          | 
| Consumer Durables & Apparel                                            | 2013 | 2867.00         | 
| Consumer Durables & Apparel                                            | 2014 | 3280.00         | 
| Consumer Durables & Apparel                                            | 2016 | 1162.00         | 
| Containers & Packaging                                                 | 2015 | 2988.00         | 
| Electrical Equipment and Machinery                                     | 2015 | 9801558.00      | 
| Energy                                                                 | 2013 | 750.00          | 
| Energy                                                                 | 2016 | 10024.00        | 
| Food & Beverage Processing                                             | 2015 | 141.00          | 
| Food & Staples Retailing                                               | 2014 | 773.00          | 
| Food & Staples Retailing                                               | 2015 | 706.00          | 
| Food & Staples Retailing                                               | 2016 | 2.00            | 
| Gas Utilities                                                          | 2015 | 122.00          | 
| Household & Personal Products                                          | 2013 | 0.00            | 
| Materials                                                              | 2013 | 200513.00       | 
| Materials                                                              | 2014 | 75678.00        | 
| Materials                                                              | 2016 | 88267.00        | 
| Materials                                                              | 2017 | 213137.00       | 
| Media                                                                  | 2013 | 9645.00         | 
| Media                                                                  | 2014 | 9645.00         | 
| Media                                                                  | 2015 | 1919.00         | 
| Media                                                                  | 2016 | 1808.00         | 
| Retailing                                                              | 2014 | 19.00           | 
| Retailing                                                              | 2015 | 11.00           | 
| Semiconductors & Semiconductor Equipment                               | 2014 | 50.00           | 
| Semiconductors & Semiconductor Equipment                               | 2016 | 4.00            | 
| Semiconductors & Semiconductors Equipment                              | 2015 | 3.00            | 
| Software & Services                                                    | 2013 | 6.00            | 
| Software & Services                                                    | 2014 | 146.00          | 
| Software & Services                                                    | 2015 | 22856.00        | 
| Software & Services                                                    | 2016 | 22846.00        | 
| Software & Services                                                    | 2017 | 690.00          | 
| Technology Hardware & Equipment                                        | 2013 | 61100.00        | 
| Technology Hardware & Equipment                                        | 2014 | 167361.00       | 
| Technology Hardware & Equipment                                        | 2015 | 106157.00       | 
| Technology Hardware & Equipment                                        | 2016 | 1566.00         | 
| Technology Hardware & Equipment                                        | 2017 | 27592.00        | 
| Telecommunication Services                                             | 2013 | 52.00           | 
| Telecommunication Services                                             | 2014 | 183.00          | 
| Telecommunication Services                                             | 2015 | 183.00          | 
| Tires                                                                  | 2015 | 2022.00         | 
| Tobacco                                                                | 2015 | 1.00            | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 2015 | 239.00          | 
| Utilities                                                              | 2013 | 122.00          | 
| Utilities                                                              | 2016 | 122.00          | 
---
### 📊 Total Carbon Emissions by Industry Group (2013–2017)

| Industry Group                                                       | 2013   | 2014   | 2015    | 2016    | 2017   |
|----------------------------------------------------------------------|--------|--------|---------|---------|--------|
| Consumer Durables, Household and Personal Products                   |        |        | 931     |         |        |
| Food, Beverage & Tobacco                                             | 4995   | 2685   | 0       | 100289  | 3162   |
| Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber |        |        | 8909    |         |        |
| Mining - Iron, Aluminum, Other Metals                                |        |        | 8181    |         |        |
| Pharmaceuticals, Biotechnology & Life Sciences                       | 32271  | 40215  |         |         |        |
| Textiles, Apparel, Footwear and Luxury Goods                         |        |        | 387     |         |        |
| Automobiles & Components                                             | 130189 | 230015 | 817227  | 1404833 |        |
| Capital Goods                                                        | 60190  | 93699  | 3505    | 6369    | 94949  |
| Chemicals                                                            |        |        | 62369   |         |        |
| Commercial & Professional Services                                   | 1157   | 477    |         | 2890    | 741    |
| Consumer Durables & Apparel                                          | 2867   | 3280   |         | 1162    |        |
| Containers & Packaging                                               |        |        | 2988    |         |        |
| Electrical Equipment and Machinery                                   |        |        | 9801558 |         |        |
| Energy                                                               | 750    |        |         | 10024   |        |
| Food & Beverage Processing                                           |        |        | 141     |         |        |
| Food & Staples Retailing                                             |        | 773    | 706     | 2       |        |
| Gas Utilities                                                        |        |        | 122     |         |        |
| Household & Personal Products                                        | 0      |        |         |         |        |
| Materials                                                            | 200513 | 75678  |         | 88267   | 213137 |
| Media                                                                | 9645   | 9645   | 1919    | 1808    |        |
| Retailing                                                            |        | 19     | 11      |         |        |
| Semiconductors & Semiconductor Equipment                             |        | 50     |         | 4       |        |
| Semiconductors & Semiconductors Equipment                            |        |        | 3       |         |        |
| Software & Services                                                  | 6      | 146    | 22856   | 22846   | 690    |
| Technology Hardware & Equipment                                      | 61100  | 167361 | 106157  | 1566    | 27592  |
| Telecommunication Services                                           | 52     | 183    | 183     |         |        |
| Tires                                                                |        |        | 2022    |         |        |
| Tobacco                                                              |        |        | 1       |         |        |
| Trading Companies & Distributors and Commercial Services & Supplies  |        |        | 239     |         |        |
| Utilities                                                            | 122    |        |         | 122     |        |

