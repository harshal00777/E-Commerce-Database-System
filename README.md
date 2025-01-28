# E-Commerce-Database-System
This project is a comprehensive SQL Server database system designed for an e-commerce platform. It includes advanced SQL features like stored procedures, triggers, indexing, and analytical queries tailored to the Indian market.
# Features
- **Indian Context**: Uses Indian customer names, products, and categories.
- **Advanced SQL**: Implements stored procedures, triggers, and indexing for performance optimization.
- **Analytics**: Provides insights into top-selling products, customer lifetime value (CLV), and popular product categories.
- **Error Handling**: Ensures data integrity with triggers to prevent negative stock quantities.
# Tables
1. **Customers**: Stores customer information.
2. **Products**: Stores product details like name, price, stock quantity, and category.
3. **Orders**: Stores order information.
4. **OrderDetails**: Stores details of each order.
# Advanced Features
- **Stored Procedure**: `PlaceOrder` to handle new orders.
- **Trigger**: `trg_PreventNegativeStock` to prevent negative stock quantities.
- **Indexes**: Optimized for frequently queried columns.
## Analytical Queries
1. **Top Selling Products**: Identifies the top 5 best-selling products.
2. **Customer Lifetime Value (CLV)**: Calculates the total spending of each customer.
3. **Popular Categories**: Shows the most popular product categories.
# How to Use
1.Open the SQL script in SQL Server Management Studio (SSMS).
2.Execute the script to create the database, tables, and sample data.
3.Explore the stored procedures, triggers, and analytical queries.
