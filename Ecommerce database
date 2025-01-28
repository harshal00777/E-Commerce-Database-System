CREATE DATABASE IndianECommerceDB;
GO

USE IndianECommerceDB;
GO


CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY IDENTITY(1,1),
    FirstName NVARCHAR(50) NOT NULL,
    LastName NVARCHAR(50) NOT NULL,
    Email NVARCHAR(100) UNIQUE NOT NULL,
    JoinDate DATETIME DEFAULT GETDATE()
);

CREATE TABLE Products (
    ProductID INT PRIMARY KEY IDENTITY(1,1),
    ProductName NVARCHAR(100) NOT NULL,
    Price DECIMAL(10, 2) NOT NULL,
    StockQuantity INT NOT NULL,
    Category NVARCHAR(50)
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY IDENTITY(1,1),
    CustomerID INT FOREIGN KEY REFERENCES Customers(CustomerID),
    OrderDate DATETIME DEFAULT GETDATE(),
    TotalAmount DECIMAL(10, 2)
);

CREATE TABLE OrderDetails (
    OrderDetailID INT PRIMARY KEY IDENTITY(1,1),
    OrderID INT FOREIGN KEY REFERENCES Orders(OrderID),
    ProductID INT FOREIGN KEY REFERENCES Products(ProductID),
    Quantity INT NOT NULL,
    Price DECIMAL(10, 2) NOT NULL
);


INSERT INTO Customers (FirstName, LastName, Email) VALUES
('Aarav', 'Patel', 'aarav.patel@example.com'),
('Ananya', 'Sharma', 'ananya.sharma@example.com'),
('Rohan', 'Singh', 'rohan.singh@example.com'),
('Priya', 'Gupta', 'priya.gupta@example.com'),
('Vikram', 'Joshi', 'vikram.joshi@example.com');

INSERT INTO Products (ProductName, Price, StockQuantity, Category) VALUES
('Silk Saree', 2500.00, 20, 'Clothing'),
('Turmeric Powder', 50.00, 100, 'Spices'),
('Smartphone', 15000.00, 30, 'Electronics'),
('Handmade Diya Set', 200.00, 50, 'Home Decor'),
('Organic Basmati Rice', 300.00, 80, 'Groceries');

INSERT INTO Orders (CustomerID, TotalAmount) VALUES
(1, 2500.00), -- Aarav bought a Silk Saree
(2, 15000.00), -- Ananya bought a Smartphone
(3, 250.00), -- Rohan bought Turmeric Powder and Handmade Diya Set
(4, 300.00); -- Priya bought Organic Basmati Rice

INSERT INTO OrderDetails (OrderID, ProductID, Quantity, Price) VALUES
(1, 1, 1, 2500.00), -- Silk Saree
(2, 3, 1, 15000.00), -- Smartphone
(3, 2, 2, 50.00), -- Turmeric Powder
(3, 4, 1, 200.00), -- Handmade Diya Set
(4, 5, 1, 300.00); -- Organic Basmati Rice

--  Indexes for Performance Optimization
CREATE INDEX idx_CustomerEmail ON Customers(Email);
CREATE INDEX idx_ProductCategory ON Products(Category);

-- Stored Procedure: Place a New Order
CREATE PROCEDURE PlaceOrder
    @CustomerID INT,
    @ProductID INT,
    @Quantity INT
AS
BEGIN
    DECLARE @Price DECIMAL(10, 2);
    DECLARE @TotalAmount DECIMAL(10, 2);

    -- Get Product Price
    SELECT @Price = Price FROM Products WHERE ProductID = @ProductID;

    -- Calculate Total Amount
    SET @TotalAmount = @Price * @Quantity;

    -- Insert Order
    INSERT INTO Orders (CustomerID, TotalAmount) VALUES (@CustomerID, @TotalAmount);

    -- Get the newly inserted OrderID
    DECLARE @OrderID INT = SCOPE_IDENTITY();

    -- Insert Order Details
    INSERT INTO OrderDetails (OrderID, ProductID, Quantity, Price) VALUES (@OrderID, @ProductID, @Quantity, @Price);

    -- Update Stock Quantity
    UPDATE Products SET StockQuantity = StockQuantity - @Quantity WHERE ProductID = @ProductID;

    PRINT 'Order placed successfully!';
END;
GO

-- Trigger: Prevent Negative Stock
CREATE TRIGGER trg_PreventNegativeStock
ON OrderDetails
AFTER INSERT
AS
BEGIN
    IF EXISTS (SELECT 1 FROM inserted i JOIN Products p ON i.ProductID = p.ProductID WHERE p.StockQuantity < 0)
    BEGIN
        RAISERROR('Cannot place order: Stock quantity would go negative.', 16, 1);
        ROLLBACK TRANSACTION;
    END
END;
GO
-- Analytical Query: Top Selling Products in India
SELECT TOP 5 p.ProductName, SUM(od.Quantity) AS TotalSold
FROM OrderDetails od
JOIN Products p ON od.ProductID = p.ProductID
GROUP BY p.ProductName
ORDER BY TotalSold DESC;

-- Analytical Query: Customer Lifetime Value (CLV) for Indian Customers
SELECT c.CustomerID, c.FirstName, c.LastName, SUM(o.TotalAmount) AS LifetimeValue
FROM Customers c
JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerID, c.FirstName, c.LastName
ORDER BY LifetimeValue DESC;

-- Analytical Query: Most Popular Product Categories in India
SELECT p.Category, SUM(od.Quantity) AS TotalSold
FROM OrderDetails od
JOIN Products p ON od.ProductID = p.ProductID
GROUP BY p.Category
ORDER BY TotalSold DESC;


