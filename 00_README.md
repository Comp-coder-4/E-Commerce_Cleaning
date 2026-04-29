# <ins>E-Commerce Transactions Cleaning Project</ins> :computer:

**This Project is a work in progress. The following is the cleaning work done this far 😃:**

The dataset looks at E-Commerce transactions across categories of products. It gives details such as order status and amount spent in each transaction.

______________________________________ 
The data source provides a messy e-commerce sales table 


## Business Objective: Clean the sales data

The goal was to find quality issues with the dataset and clean it to improve data quality. This is done using .....

Tool Used: Excel

Key Excel techniques/functions used:


## File Structure
1. **messy_ecommerce_sales.xlsx**
   * Columns used to clean data are orange
3. **Imgs folder** - screenshots
4. **raw_data.csv** - raw data


## Workflow
### Step 1. Profile the data with Excel Power Query

Issues identified:
1. Data Type
    - Order_date is text
    - Quantity is text
    - Price is text
2. Invalid data
    - Date fields and numeric fields have text values
    - Numeric fields have negative values and unwanted symbols
3. Inconsistency
    - Inconsistent formats in order_date field ("Jan 5 2023, mm,dd,yyyy, m,dd,yyyy, mm,dd,yyyy 00:00:00)
    - Inconsistent letter casing:
      - Product field: "Shoes" and "shoes"
      - Category field: "electronic", "Electronic", "ELECTRONICS"
    - Mispellings: Category has "Electronicss"
4. Missing values:
   - Category has "nan" values and blanks
   - Quantity and Price have blanks
   - Total has nulls
    
### Step 2. Duplicates
1. Conditional formatting:
   - ID has duplicates
2. Created a CONCAT column
   - concatenates all columns to give each row unique value

If ID was duplicated and if both of their CONCAT column values were the same, that meant the entire row was a duplicate. See Imgs/Removing Duplicates/Finding duplicate rows.png. In this screenshot
    - ID 146 was duplicated 
    - ID 142 is not duplicated
    - One row for ID 142 had incorrect value in Total column (indicated by yellow box)


## Data Source
https://www.kaggle.com/datasets/kandeelai22/messy-e-commerce-sales-dataset
