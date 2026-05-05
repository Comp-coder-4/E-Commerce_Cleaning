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
1. **messy_ecommerce_sales.xlsx**
   * Columns used to clean data are orange
3. **Imgs folder** - screenshots
4. **raw_data.csv** - raw data

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
1. **Problem** Prices were unreasonably high
  - E.g. Biography book price was 552.
  - In reality, wouldn't price wouldn't be more than 20
  - **Solution** A variety of categories of products sold. Likely not selling expensive products (e.g. expensive sport shop). Each product has different price so cannot replace price values from other orders. Therefore, for this analysis, I divide Price by 10 and find Price Per Unit based on this value (**Imgs/.../Prices**)
2. **Problem** Some products had wrong category
    - E.g. ID = 166, product = t-shirt, category = "electronics"
    - **Solution** 
      - Use pivot table to find categories each product was in (**Imgs/.../Category/Pivot table (product,categories).png**)
      - Identify which products were in wrong categories:
        - Wrong products in Electronics: Basketball, blender, lamp, microwave, t-shirt, vacuum, yoga mat
      - In Excel main table, make flag column where Category = "Electronics" and product is either "Basketball", "Blender", "Lamp", "Microwave", "T-shirt", "Vacuum" or "Yoga Mat" (**Imgs/.../Category/Wrong Category FLAG.png**)
      - Write Excel IF function to make correct Category Column (**Imgs/.../Category/Correct Category Column.png**)
     
#### Changes made for invalid values in numeric columns:

**Negative values are consistent within a single row i.e. quantity * price = total so negative symbols can be removed**

1. Price
   - 300$ &rarr; 300. **Assumption:** Price = 300$ should be 300. Similar orders have price in range 206 to 868 (**Imgs/../Prices/Price=300$**)
   - "abd" %rarr; 400
2. Quantity
   - Deduce quantity from new price column and total
4. Total
   - Calculate total = price / quantity
   - **Problem:** Received errors when price and quantity were missing. **Solution:** Used logical functions to calculate total and return empty string if quantity and price were missing (**Imgs/../Total Column/Fix Error in Total column.png**)

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
(**Screenshots: Imgs/.../Fixing Data Types**)
1. Using Power Query:
- ID INT
- Customer_Name Text
- Order_ID Text
- Product Text
- Category Text
- Quantity INT
- Payment_Method Text
- Status Text

2. Order_date (**Imgs/.../Fixing Data Types/Order Date**)
   - Parse to 3 text columns: day, month and year
   - **Problem:** Year column had values that were missing first digit of the year **Solution:** Concatenate 2 to the start of year values that had first digit missing
   - Make Order_Date DATE column using DATE() function
   - **Problem 2:** Order_date in wrong format after being loaded from Power Query to Excel
   - **Solution 2:** I changed the locale to match the dd/mm/yyyy format
  
### <ins>Step 6. Fix Columns</ins>

Remove old columns and columns used for cleaning. 

Make column names consistent.

## Data Source
https://www.kaggle.com/datasets/kandeelai22/messy-e-commerce-sales-dataset
