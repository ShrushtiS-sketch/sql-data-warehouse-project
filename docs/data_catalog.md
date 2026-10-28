# Data Catalog for Gold Layer

## Overview

The Gold Layer represents business-ready data structured to support analytics, reporting, and decision-making. It consists of dimension tables and fact tables that organize customer, product, and sales information for efficient analysis.

---

## 1. gold.dim_customers

**Purpose:** Stores customer information enriched with demographic and geographic data.

### Columns

| Column Name | Data Type | Description |
|---|---|---|
| customer_key | INT | Surrogate key uniquely identifying each customer record in the dimension table. |
| customer_id | INT | Unique numerical identifier assigned to each customer. |
| customer_number | NVARCHAR(50) | Alphanumeric identifier used to track and reference customers. |
| first_name | NVARCHAR(50) | Customer's first name. |
| last_name | NVARCHAR(50) | Customer's last name or family name. |
| country | NVARCHAR(50) | Country associated with the customer's location. |
| marital_status | NVARCHAR(50) | Customer's marital status, such as Married or Single. |
| gender | NVARCHAR(50) | Customer's gender, such as Male, Female, or n/a. |
| birthdate | DATE | Customer's date of birth. |
| create_date | DATE | Date the customer record was created in the source system. |

---

## 2. gold.dim_products

**Purpose:** Stores product information and attributes, including product categories, subcategories, costs, and product lines.

### Columns

| Column Name | Data Type | Description |
|---|---|---|
| product_key | INT | Surrogate key uniquely identifying each product record in the dimension table. |
| product_id | INT | Unique identifier assigned to the product. |
| product_number | NVARCHAR(50) | Alphanumeric code used to identify and reference the product. |
| product_name | NVARCHAR(50) | Descriptive name of the product, including relevant attributes such as type, color, or size. |
| category_id | NVARCHAR(50) | Identifier representing the product's category. |
| category | NVARCHAR(50) | Broad classification of the product, such as Bikes or Components. |
| subcategory | NVARCHAR(50) | More specific classification of the product within its category. |
| maintenance | NVARCHAR(50) | Indicates whether the product requires maintenance. |
| cost | INT | Product cost in the source data's monetary units. |
| product_line | NVARCHAR(50) | Product line or series to which the product belongs, such as Road or Mountain. |
| start_date | DATE | Date from which the product record becomes effective or the product becomes available, according to the source data. |

---

## 3. gold.fact_sales

**Purpose:** Stores sales transaction data, including order details, customer and product references, dates, quantities, and sales amounts. It supports sales performance analysis and reporting.

### Columns

| Column Name | Data Type | Description |
|---|---|---|
| order_number | NVARCHAR(50) | Identifier representing the sales order, such as SO54496. |
| product_key | INT | Surrogate key linking the sales record to the product dimension table. |
| customer_key | INT | Surrogate key linking the sales record to the customer dimension table. |
| order_date | DATE | Date when the order was placed. |
| shipping_date | DATE | Date when the order was shipped to the customer. |
| due_date | DATE | Due date associated with the order. |
| sales_amount | INT | Total sales amount for the sales line item, in the source data's monetary units. |
| quantity | INT | Number of units ordered for the sales line item. |
| price | INT | Price per unit for the sales line item, in the source data's monetary units. |

---

## Gold Layer Data Model

The Gold Layer follows a dimensional modeling approach:

- **Dimension tables:** `gold.dim_customers` and `gold.dim_products` provide descriptive information about customers and products.
- **Fact table:** `gold.fact_sales` stores measurable sales transactions.
- **Surrogate keys:** `customer_key` and `product_key` connect sales records to their respective dimension tables.
- **Analytical purpose:** This structure supports reporting and analysis of sales by customer, product, and date.

### Relationships

| Fact Table Column | Dimension Table | Dimension Column |
|---|---|---|
| product_key | gold.dim_products | product_key |
| customer_key | gold.dim_customers | customer_key |

---

## Note

The data types and column descriptions in this catalog should be verified against the actual SQL definitions of the Gold Layer views or tables. In particular, `ROW_NUMBER()`-generated surrogate keys in views may change when the underlying data changes.
