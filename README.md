# 📝 Assignment: Database Design and Normalization

## 🎯 **Learning Objectives**
* 🛠️ **Understand the principles of good database design** and **normalization**.
* 💡 **Apply normalization techniques** to improve database structure and efficiency.
* 🔍 **Learn First, Second, and Third Normal Forms** (1NF, 2NF, 3NF) to eliminate redundancy and optimize data storage.

---

## 📋 **What You'll Need**
* 💻 A computer with internet access.
* ✍️ A code editor (e.g., Visual Studio Code).
* 🖥️ MySQL Workbench or another SQL database environment.

---


## 📝 Submission Instructions  
📂 Write all your SQL queries in the **answers.sql** file.  
✍️ Answer each question concisely and make sure your queries are clear and correct.  
🗣️ Structure your responses clearly, and use comments if necessary to explain your approach.

--- 

## 📚 Assignment Questions

---

### Question 1 Achieving 1NF (First Normal Form) 🛠️
Task:
- You are given the following table **ProductDetail**:

| OrderID | CustomerName  | Products                        |
|---------|---------------|---------------------------------|
| 101     | John Doe      | Laptop, Mouse                   |
| 102     | Jane Smith    | Tablet, Keyboard, Mouse         |
| 103     | Emily Clark   | Phone                           |


- In the table above, the **Products column** contains multiple values, which violates **1NF**.
- **Write an SQL query** to transform this table into **1NF**, ensuring that each row represents a single product for an order

--- 

### Question 2 Achieving 2NF (Second Normal Form) 🧩

- You are given the following table **OrderDetails**, which is already in **1NF** but still contains partial dependencies:

| OrderID | CustomerName  | Product      | Quantity |
|---------|---------------|--------------|----------|
| 101     | John Doe      | Laptop       | 2        |
| 101     | John Doe      | Mouse        | 1        |
| 102     | Jane Smith    | Tablet       | 3        |
| 102     | Jane Smith    | Keyboard     | 1        |
| 102     | Jane Smith    | Mouse        | 2        |
| 103     | Emily Clark   | Phone        | 1        |

- In the table above, the **CustomerName** column depends on **OrderID** (a partial dependency), which violates **2NF**. 

- Write an SQL query to transform this table into **2NF** by removing partial dependencies. Ensure that each non-key column fully depends on the entire primary key.

--WITH
  -- Create a temporary table to store the split values
  TempProducts AS (
    SELECT
      OrderID,
      CustomerName,
      -- Split the comma-separated values into separate rows
      value AS Product
    FROM
      ProductDetail,
      LATERAL STRING_SPLIT(Products, ',') AS string_split(value)
  )
SELECT
  OrderID,
  CustomerName,
  Product
FROM
  TempProducts;
Explanation:
1. WITH TempProducts AS (...):
This defines a common table expression (CTE) called TempProducts. This CTE will hold the transformed data before it's used in the final SELECT statement.
2. LATERAL STRING_SPLIT(Products, ',') AS string_split(value):
This is the core part of the query. The LATERAL STRING_SPLIT function splits the Products column, which contains comma-separated values, into separate rows. Each comma-separated value is assigned to the value column in the string_split alias.
3. SELECT OrderID, CustomerName, value AS Product:
This selects the OrderID, CustomerName, and the split value (aliased as Product) from the TempProducts CTE.
4. SELECT OrderID, CustomerName, Product FROM TempProducts:
Finally, this selects the OrderID, CustomerName, and the Product from the TempProducts CTE, which now contains the normalized data where each row represents a single product for a given order. 
Result:
The query will produce a table where each row represents a single product ordered by a customer, ensuring the table is in 1NF. 
For the given input, the transformed table will look like this:
OrderID
CustomerName
Product
101
John Doe
Laptop
101
John Doe
Mouse
102
Jane Smith
Tablet
102
Jane Smith
Keyboard
102
Jane Smith
Mouse
103
Emily Clark
Phone

AI responses may include mistakes.
-
Good luck 🚀
