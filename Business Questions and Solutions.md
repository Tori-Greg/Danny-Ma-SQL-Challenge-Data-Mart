# Data Mart Business Questions and Solutions

We delved deeply into Danny's critical business questions by conducting thorough data cleansing and exploratory analysis on the provided dataset.
The data cleansing process included the creation of a new table, converting the date data type from varchar to the appropriate date format, and generating new columns as necessary.

# Part 1: Data Cleansing
Kindly check full challenge for data cleansing instructions. Click [here](https://8weeksqlchallenge.com/case-study-5/)

    CREATE TABLE DataMart..clean_weekly_sales (
        week_date DATE,
        week_number INT,
        month_number INT,
        calendar_year INT,
        age_band VARCHAR(25),
        demographic VARCHAR(25),
        avg_transaction DECIMAL(20, 2),
	      sales BIGINT
    )

    INSERT INTO DataMart..clean_weekly_sales 
    SELECT 
        TRY_CAST(week_date AS DATE) AS week_date,
        DATEPART(WEEK, TRY_CAST(week_date AS DATE)) AS week_number,
        DATEPART(MONTH, TRY_CAST(week_date AS DATE)) AS month_number,
        YEAR(TRY_CAST(week_date AS DATE)) AS calendar_year,
    CASE
        WHEN segment LIKE '%1' THEN 'Young Adults' 
        WHEN segment LIKE '%2' THEN 'Middle Aged'
        WHEN segment LIKE '%4' THEN 'Retirees'
        ELSE 'NULL'
    END AS age_band,
    CASE
        WHEN segment LIKE '%C%' THEN 'Couples' 
        ELSE
            CASE
                WHEN segment LIKE '%F%' THEN 'Families'
                ELSE 'NULL'
            END
    END AS demographic,
      (sales/transactions) AS avg_transaction
    FROM DataMart..weekly_sales
    WHERE TRY_CAST(week_date AS DATE) IS NOT NULL;
    
**Summary:** This query outlines the procedures for establishing a fresh table named "clean_weekly_sales," which served as the foundation for introducing new columns and rectifying data types from varchar to date whiile filtering out invalid dates.

---

# Part 2: Data Exploration
