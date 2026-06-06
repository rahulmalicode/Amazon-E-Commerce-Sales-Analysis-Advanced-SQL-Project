# Amazon-E-Commerce-Sales-Analysis-Advanced-SQL-Project

## Difficulty Level: Advanced

### Project Overview

I have worked on analyzing a dataset of over 20,000 sales records from an Amazon-like e-commerce platform. This project involves extensive querying of customer behavior, product performance, and sales trends using PostgreSQL. Through this project, I have tackled various SQL problems, including revenue analysis, customer segmentation, and inventory management.

The project also focuses on data cleaning, handling null values, and solving real-world business problems using structured queries.

An ERD diagram is included to visually represent the database schema and relationships between tables.

---

### **Database Setup & Design**

### **Schema Structure**
- The database contains 9 tables: *category* , *customers*, *sellers*, *products*, *orders*, *order items*, *inventory*, *payments*, and *shippings*.
- These tables are designed with primary keys, foreign key constraints, and proper indexing to maintain data integrity and optimize query performance

### **Constraints**
- Referential integrity is enforced using foreign keys.
- Default values and data types are applied where necessary to maintain consistency.
- Uniqueness is guaranteed for fields like product_id, order_id, and customer_id.

  
## 🔧 Tools & Technologies
- **Database:** MYSQL
- **Language:** SQL (Advanced)
- **Concepts Used:** Window Functions, CTEs, Subqueries, Joins,
  Aggregations, Date Functions, CASE statements

---

## **Task: Data Cleaning**

I cleaned the dataset by:

- **Removing duplicates**: Duplicates in the customer and order tables were identified and removed.
- **Handling missing values**: Null values in critical fields (e.g., customer address, payment status) were either filled with default values or handled using appropriate methods.

---

## **Handling Null Values**

Null values were handled based on their context:

- **Customer addresses**: Missing addresses were assigned default placeholder values.
- **Payment statuses**: Orders with null payment statuses were categorized as "Pending."
- **Shipping information**: Null return dates were left as is, as not all shipments are returned.

---

## **Identifying Business Problems**

Key business problems identified:

-- Store Produre
create a function as soon as the product is sold the same quantity should reduced from inventory table
after adding any sales records it should update the stock in the inventory table based on the product and qty purchase .

SELECT * FROM products;
 -- product_id 2 -- laptop -- 24 stock
 -- product_id 3 -- tablet -- 7 stock
select * from inventory;

SELECT * from orders;
SELECT * from order_items;
SELECT * from inventory;
SELECT * FROM products
order_id,
order_date,
customer_id,
seller_id,
order_item_id,
product_id,
quantity,
```sql
CREATE OR REPLACE PROCEDURE add_sales
(
p_order_id INT,
p_order_date INT,
p_customer_id INT,
p_seller_id INT,
p_order_item_id INT,
p_product_id INT,
p_quantity INT,

)
LANGUAGE 
AS $$

DECLARE
-- all variable
v_count INT;
v_price FLOAT;
v_product VARCHAR(50)
BEGIN
-- Fetching product name and price based p id entered

SELECT price ,product_name
 INTO v_price , v_product
 FROM products
 WHERE product_id = p_product_id;
 
 -- Checking stock AND product availability in inventory
SELECT 
	COUNT(*)
    INTO
    v_count
    FROM inventory
    where
		product_id = p_product_id
        AND
        stock >= p_quantity;
 IF v_count > 0 THEN
 -- add into orders and order_items table
 -- update inventory
     INSERT INTO orders(order_id, order_date , customer_id, seller_id)
	 VALUES
     (P_order_id,CURRENT_DATE,p_customer_id,p_seller_id);
     
     -- adding into order list
     INSERT INTO order_items(order_item_id, order_id, product_id, quantity, price_per_unit, total_sale)
	  VALUES
      (p_order_item_id, p_order_id, p_product_id, p_quantity, v_price, v_price*p_quantity);
   
   -- updating inventory
   UPDATE inventory
   SET stock = stock - p_quantity
   WHERE product_id = p_product_id;
   
   RAISE NOTICE 'Thank you product: % sale has been added also inventory stock updates',v_product;

   ELSE 

     RAISE NOTICE 'Thank you for your info the product:is not available',v_product;
 END IF;
 
 END;
 $$

```

1. Low product availability due to inconsistent restocking.
2. High return rates for specific product categories.
3. Significant delays in shipments and inconsistencies în delivery times.
4. High customer acquisition costs with a low customer retention rate.

---

## **Solving Business Problems**

### Solutions Implemented:

- **Restock Prediction**: By forecasting product demand based on past sales, I optimized restocking cycles, minimizing stockouts
- **Product Performance**: Identified high-return products and optimized their sales strategies, such as product bundling and pricing adjustments.
- **Shipping Optimization**: Analyzed shipping times and delivery providers to recommend better logistics strategies and improve customer satisfaction.
- **Customer Segmentation**: Conducted RFM analysis to target marketing efforts towards "At-Risk" customers, improving retention and loyalty.

### **Qbjectives**
The primary objective of this project is to showcase SQL proficiency through complex queries that address real-world e-commerce business challenges. The analysis covers various aspects of e-commerce operations, including:
- Customer behavior
- Sales trends
- Inventory management
- Payment and shipping analysis
- Forecasting and product performance

---

## **Learning Outcomes**

This project enabled me to:
- Design and implement a normalized database schema.
- Clean and preprocess real-world datasets for analysis.
- Use advanced SQL techniques, including window functions, subqueries, and joins.
- Conduct in-depth business analysis using SQL.
- Optimize query performance and handle large datasets efficiently.

---

## **Conclusions**

This advanced SQL project successfully demonstrates my ability to solve real-world e-commerce problems using structured queries. From improving customer retention to optimizing inventory and logistics, the project provides valuable insights into operational challenges and solutions.

By completing this project, I have gained a deeper understanding of how SQL can be used to tackle complex data problems and drive business decision-making-

---
### **ERD DIAGRAM**

<img width="1395" height="855" alt="amazon erd" src="https://github.com/user-attachments/assets/a1c1a6c2-deaf-4b5f-b6da-b09c136f314e" />

