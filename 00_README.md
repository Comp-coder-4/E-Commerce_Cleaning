# <ins>E-Commerce Transactions Cleaning Project</ins> :computer:

The dataset looks at E-Commerce transactions across categories of products. It gives details such as order status and amount spent in each transaction.

______________________________________ 
The data source provides a messy e-commerce sales table

## Business Objective: Clean the sales data

The goal was to find quality issues with the dataset and clean it to improve data quality.

Tool Used: Excel

Key Excel functions used:
- Logical (IF, AND, OR)
- Date (DATE)

## File Structure
1. **Imgs folder** - screenshots
2. **raw_data.csv** - raw data
3. **cleaned_ecommerce_sales_data.xlsx** - data after cleaning

## Workflow
### <ins>Step 1. Profile the data with Excel Power Query</ins>

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
    
### <ins>Step 2. Duplicates</ins>
**_(Screenshots: Imgs/Removing Duplicates)_**

1. Conditional formatting:
   - ID has duplicates
2. Created a CONCAT column
   - concatenates all columns to give each row a unique value

If ID was duplicated **AND if both** of their **CONCAT column values were the same**, that meant the entire row was a duplicate. See **_Imgs/Removing Duplicates/Finding duplicate rows.png_**. In this screenshot:
* ID 146 was duplicated 
* ID 142 is not duplicated
* One row for ID 142 had incorrect value in Total column (indicated by yellow box)

### <ins>Step 3. Find & Replace Values</ins>
#### <ins>Text Columns</ins>
#### Changes made for invalid values in text columns:
**_(Screenshots: Imgs/Replacing Values/Text Columns)_**
1. Order_date 
   1. "00:00:00" &rarr; "" (empty string)
  2. single-digit months (**<ins>m</ins>**/dd/yyyy) &rarr; 2-digit months (**<ins>mm</ins>**/dd/yyyy)
     1. Parse order_date into month and day/year columns
     2. Replace m &rarr; mm values in month column
     3. Merge month and day/year columns
  3. "abc/" &rarr; "" (empty string)
  4. "Jan 5 2023" &rarr; "01/05/2023"
2. Product
   1. "shoes" &rarr; "Shoes"
3. Category
   1. "electronic", "Electronicss", "ELECTRONICS" &rarr; "Electronics"
   2. "sports" &rarr; "Sports"
   3. "nan" &rarr; "null"

#### <ins>Numeric Columns</ins>

**_(Screenshots: Imgs/Replacing Values/Numeric Columns)_**

#### Exploring the table further revealed the following:
1. **Problem 1:** Prices were unreasonably high (**_Imgs/Replacing Values/Numeric Column/Prices_**)
     - E.g. Biography book price was 552.
     - In reality, wouldn't price of a biography wouldn't be more than 20
     - **Solution 1:** A variety of categories of products sold. Likely not selling expensive products (e.g. expensive sport shop). Each product has different price so cannot replace price values from other orders. Therefore, for this analysis, I divide Price by 10 and find Price Per Unit based on this value
2. **Problem 2:** Some products had wrong category (**_Imgs/Replacing Values/Text Columns/Category 2_**)
    - E.g. ID = 166, product = t-shirt, category = "electronics"
    - **Solution 2:** 
      - Use pivot table to find categories each product was in
      - Identify which products were in wrong categories:
        - Wrong products in Electronics: Basketball, blender, lamp, microwave, t-shirt, vacuum, yoga mat
      - In Excel main table, make flag column where Category = "Electronics" and product is either "Basketball", "Blender", "Lamp", "Microwave", "T-shirt", "Vacuum" or "Yoga Mat"
      - Write Excel IF function to make correct Category Column
     
#### Changes made for invalid values in numeric columns:

**Negative values are consistent within a single row i.e. quantity x price = total so negative symbols can be removed**

1. Price
   - 300$ &rarr; 300. **Assumption:** Price = 300$ should be 300. Similar orders have price in range 206 to 868
   - "abd" %rarr; 400
2. Quantity
   - Deduce quantity from new price column and total
4. Total
   - Calculate total = price x quantity
   - **Problem:** Received errors when price and quantity were missing. **Solution:** Used logical functions to calculate total and return empty string if quantity and price were missing (**_Imgs/Replacing Values/Numeric Columns/Total Column/Fix Error in Total column.png_**)

### <ins>Step 4. Missing Values</ins>
#### <ins>Non-numeric Columns</ins>
#### Changes made for invalid values in non-numeric columns:
1. Category
   - Fill based on product (Using pivot table with products and their category)
2. Order_date
   - Can't deduce date so left blank
  
#### <ins>Numeric Columns</ins>
Already filled in previous steps

### <ins>Step 5. Fix Data Types</ins>
(**_Screenshots: Imgs/Fixing Data Types_**)
1. Using Power Query:
- ID INT
- Customer_Name Text
- Order_ID Text
- Product Text
- Category Text
- Quantity INT
- Payment_Method Text
- Status Text

2. Order_date (**_Imgs/Fixing Data Types/Order Date_**)
   - Parse order_date to 3 text columns: day, month and year
   - **Problem 1:** Year column had values that were missing first digit of the year **Solution 1:** Concatenate 2 to the start of year values that had first digit missing
   - Make Order_Date DATE column using DATE() function
   - **Problem 2:** Order_date in wrong format after being loaded from Power Query to Excel
   - **Solution 2:** I changed the locale to match the dd/mm/yyyy format

3. Price
   - Round to 2 d.p.
  
### <ins>Step 6. Fix Columns</ins>

Remove old columns and columns used for cleaning. 

Make column names consistent.

### <ins>Step 6. Outliers</ins>

**_(Screenshots Imgs/Outliers)_**

Price Per Unit
    - Skewness = 7.6 - Highly skewed to right
    - Outlier for ID = 117, product = Blender, **price = 500**
    - This was not caused by an error in my calculation. Instead, the original value for the price was an outlier.

## Data Source
https://www.kaggle.com/datasets/kandeelai22/messy-e-commerce-sales-dataset
