# <ins>E-Commerce Transactions Cleaning Project</ins> :computer:

**This Project is a work in progress. The following is the cleaning work done this far 😃:**

The dataset looks at E-Commerce transactions across categories of products. It gives details such as order status and amount spent in each transaction.

______________________________________ 
The data source provides a messy e-commerce sales table 


## Business Objective: Clean the sales data

The goal was to find quality issues with the dataset and clean it to improve data quality. This is done using .....

Tool Used: Excel

Key Excel techniques/functions used:
- Logical functions (IF, AND)


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

If ID was duplicated **AND if both** of their **CONCAT column values were the same**, that meant the entire row was a duplicate. See Imgs/Removing Duplicates/Finding duplicate rows.png. In this screenshot:
* ID 146 was duplicated 
* ID 142 is not duplicated
* One row for ID 142 had incorrect value in Total column (indicated by yellow box)

### Step 3. Find & Replace Values

#### Changes made for invalid values in text columns:
**(Screenshots: Img/Replacing Values/Text Columns)**
1. Order_date 
   1. "00:00:00" --> "" (empty string)
  2. single-digit months (**m**/dd/yyyy) --> 2-digit months (**mm**/dd/yyyy)
     1. Parse order_date into month and day/year columns
     2. Replace m --> mm values in month column
     3. Merge month and day/year columns
  3. "abc/" --> "" (empty string)
  4. "Jan 5 2023" --> "01/05/2023"
2. Product
   1. "shoes" --> "Shoes"
3. Category
   1. "electronic", "Electronicss", "ELECTRONICS" --> "Electronics"
   2. "sports" --> "Sports"
   3. "nan" --> "null"

#### Changes made for invalid values in numeric columns:
**(Screenshots: Img/Replacing Values/Numeric Columns)**
Exploring the table further revealed the following:
1. **Problem** Prices were unreasonably high
  - E.g. Biography book price was 552.
  - In reality, wouldn't price wouldn't be more than 20
  - **Solution** A variety of categories of products sold. Likely not selling expensive products (e.g. expensive sport shop). Each product has different prices so cannot replace price values from same table. Therefore, for this analysis, I divide Price by 10 and find Price Per Unit based on this value
2. **Problem** Some products had wrong category
    - E.g. ID = 166, product = t-shirt, category = "electronics"
    - **Solution** 
      - Use pivot table to find categories each product was in (**Imgs/.../Pivot table (product,categories).png**)
      - Identify which products were in wrong categories:
        - Wrong products in Electronics: Basketball, blender, lamp, microwave, t-shirt, vacuum, yoga mat
      - In Excel main table, make flag column where Category = "Electronics" and product is either "Basketball", "Blender", "Lamp", "Microwave", "T-shirt", "Vacuum" or "Yoga Mat" (**See Imgs/.../Wrong Category FLAG.png**)
      - Write Excel IF function to make correct Category Column (**See Imgs/.../Correct Category Column.png**)


## Data Source
https://www.kaggle.com/datasets/kandeelai22/messy-e-commerce-sales-dataset
