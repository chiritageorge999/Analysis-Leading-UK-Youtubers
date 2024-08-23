# Data Portofolio Project: Excel, SQL, PowerBi


# Objective 
- What is the key pain point?
The Head of Marketing wants to identify the top YouTubers in the UK market for 2024. This information will be crucial in guiding future marketing campaign decisions for the year.

- What is the ideal solution?
A dashboard will be ideal in this situation as it will provide easy access to insights like:
- subscriber count
- total views
- total videos
- engagement metrics

# Data Source

- What data is needed to achieve our objective?
  We need data on the top YouTubers for the UK in 2024 that includes their:
- channel_names
- total_subscribers
- total_views
- total_videos_uploaded

The data is sourced from Kaggle, [access it here.](https://www.kaggle.com/datasets/bhavyadhingra00020/top-100-social-media-influencers-2024-countrywise?resource=download)

# Stages
- Design
- Development
- Testing
- Analysis

# Design
## Dashboard components required
- What should the dashboard contain based on the requirements provided?

To understand what it should contain, we have to figure out what questions we need the dashboard to answer:

1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

These questions need answers. As we progress, they may need to be reconsidered.

## Dashboard mockup

- What should it look like?

Some visuals that may be appropriate for answering our questions include:

1. Table
2. Treemap
3. Scorecards
4. Horizontal bar chart

## Tools 

| Tool | Purpose |
| --- | --- |
| Excel | Exploration, Testing |
| SQL Server & SSMS | Cleaning, Testing, Analyzing | 
| Power BI | Visualizing the data in an interactive dashboard | 
| GitHub | Hosting the project documentation and version control | 

# Development

## Pseudocode

- What's the general approach in creating the solution from start to finish?

1. Get the data
2. Explore the data in Excel
3. Load the data into SQL Server
4. Clean the data with SQL
5. Test the data with SQL
6. Visualize the data in PowerBi
7. Generate the findings based on the insights
8. Write the documentation and commentary
9. Publish the data to GitHub Pages

## Data exploration notes

This is the stage where we scan the data for errors, inconsistencies, bugs, weird and corrupted characters, etc.

- Initial observations:
1. There are at least 4 columns that contain the data we need for this analysis, we don't need to gather any more data.
2. The first column contains the channel ID with what appears to be the channels IDS, separated by a @ symbol - we need to extract the channel name from this.
3. Some of the cell header names are in a different language - we need to address them.
4. We have more data than we need, we would have to drop some of the columns.

## Data Cleaning
- What do we expect the clean data to look like?

The aim is to refine our dataset to ensure it is structured and ready for analysis.

The cleaned data should meet the following criteria and constraints: 

- Only relevant columns should be retained.
- All data types should be appropriate for the contents of each column.
- No column should contain null values, indicating complete data for all records

Data constraints on the cleaned dataset:

| Property | Description |
| --- | --- |
| Number of rows | 100 |
| Number of columns | 4 |

Tabular representations of the expected schema for the clean data: 

| Column name  | Description |
| --- | --- |
| channel_name | VARCHAR | NO |
| total_subscribers | INTEGER | NO |
| total_views | INTEGER | NO |
| total_videos | INTEGER | NO |

- What steps are needed to clean and shape the data into the desired format?

1. Remove unnecessary columns by only selecting the ones you need
2. Extract YouTube channel names from the first column
3. Rename columns using aliases



### Transform the data

```SQL
/*
# 1. Extract the channel name from the 'NOMBRE' column
# 2. Select the required columns
*/

-- 1.
SELECT
    SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE) -1) AS channel_name,
-- 2.
    total_subscribers,
    total_views,
    total_videos

FROM
    top_uk_youtubers_2024
```

### Create the SQL view

```SQL
/*
# 1. Create a view to store the transformed data
# 2. Cast the extracted channel name as VARCHAR(100)
# 3. Select the required columns from the top_uk_youtubers_2024 SQL table 
*/

-- 1.
CREATE VIEW view_uk_youtubers_2024 AS

-- 2.
SELECT
    CAST(SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE) -1) AS VARCHAR(100)) AS channel_name,

-- 3.
    total_subscribers,
    total_views,
    total_videos
FROM
    top_uk_youtubers_2024

```

# Testing

- Data quality and validation checks

## Row count check
```SQL
/*
# Count the total number of records (or rows) in the SQL view
*/

SELECT
    COUNT(*) AS no_of_rows
FROM
    view_uk_youtubers_2024;

```

![Row count check](assets/images/1_row_count_check.png)

## Column count check
### SQL query
```SQL
/*
# Count the total number of columns (or fields) in the SQL view
*/

SELECT
    COUNT(*) AS column_count
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024'
```
### Output 
![Column count check](assets/images/2_column_count_check.png)

## Data type check
### SQL query
```SQL
/*
# Check the data types of each column from the view by checking the INFORMATION SCHEMA view
*/

-- 1.
SELECT
    COLUMN_NAME,
    DATA_TYPE
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024';
```
### Output
![Data type check](assets/images/3_data_type_check.png)


## Duplicate count check
### SQL query 
```SQL
/*
# 1. Check for duplicate rows in the view
# 2. Group by the channel name
# 3. Filter for groups with more than one row
*/

-- 1.
SELECT
    channel_name,
    COUNT(*) AS duplicate_count
FROM
    view_uk_youtubers_2024

-- 2.
GROUP BY
    channel_name

-- 3.
HAVING
    COUNT(*) > 1;
```
### Output
![Duplicate count check](assets/images/4_duplicate_records_check.png)





















