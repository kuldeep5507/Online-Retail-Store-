
# **📊 Online Retail Store Database Queries**
## **🔢 🗂️ Assumed Schema**

**Customers**(*customer_id, name, email, City, join_date*)    
**Products** (*product_id, Product_name, category, price*)    
**Orders** (*order_id, customer_id, order_date*)     
**Order_Items** (*order_id, product_id, quantity, Total_price*)     
**Payments** (*payment_id, order_id, payment_date, payment_method, amount*)         
**Shipments** (*Shipment_id, Order_id, Shipment_date, Delivery_date, Carrier, status*)
     
## **🟢 Beginner Level Queries (1–7)**

### **1️⃣✅ Show all products with name, price, and Category ?**   
**🔰*View the current product listings and their availability.***

SELECT Product_name   
, price   
, Category    
FROM Products;

### **2️⃣🧑 List all customers with email and join date ?**    
**🔰*Useful for sending welcome offers or newsletters.***

SELECT Name  
, Email   
, Signup_date   
FROM Customers;

### **3️⃣🛒 Retrieve all orders made by customers with End of Month & Year ?**   
**🔰*Tracks when and what customers ordered.***

SELECT order_id  
, customer_id    
, order_date    
FROM Orders   
ORDER BY Order_Date DESC;

### **4️⃣📦 Get all Low-of-stock products price ?**    
**🔰*Identifies products that have low price less than 50.***

SELECT Product_name      
FROM Products      
WHERE Price <= 50;

### **5️⃣🔍 Search products by category 'Electronics' ?**   
**🔰*Filters products for browsing in a specific category.***

SELECT *      
FROM Products     
WHERE category = 'Electronics';

### **6️⃣💸 Find all payments made using 'Credit Card' ?**        
**🔰*Understand how often each payment method is used.***

SELECT *     
FROM Payments     
WHERE payment_method = 'Credit Card';

### 7️⃣📝 Show all Details for a specific Order ?**  
**🔰*Customer Query for a given Order (e.g., Order_Id = 250)***

SELECT *      
FROM shipments     
WHERE Order_Id = 250;

## **🟡 Moderate Level Queries (8–14)**

### **8️⃣🔎 Top 10 Best-Selling Products ?**    
**🔰*Finds most sold products by quantity***

SELECT Products.Product_name   
, SUM(Order_items.quantity) AS total_sold    
FROM Order_items         
JOIN Products ON Order_items.product_id = Products.product_id     
GROUP BY Products.Product_name       
ORDER BY total_sold DESC         
LIMIT 10;


### **9️⃣👥 Top 5 customers by number of orders ?**
**🔰*Identifies the most frequent buyers***
   
SELECT C.name    
, COUNT(O.order_id) AS total_orders    
FROM Customers C    
JOIN Orders O ON C.customer_id = O.customer_id    
GROUP BY C.name    
ORDER BY total_orders DESC    
LIMIT 5;


### **🔟💰 Total revenue by month ?**
**🔰*Analyzes sales trends over time***

SELECT DATE_TRUNC('month', order_date) AS month    
, SUM(OI.Total_Price) AS monthly_revenue    
FROM Orders O      
JOIN Order_Items OI ON O.Order_ID = OI.Order_ID    
GROUP BY month    
ORDER BY month; 

### **1️⃣1️⃣🧾Most sold products (by quantity) ?**
**🔰*Helps decide what products to keep or promote.***

SELECT P.Product_name   
, SUM(OI.quantity) AS total_sold  
FROM Order_Items OI  
JOIN Products P ON OI.product_id = P.product_id   
GROUP BY P.Product_name   
ORDER BY total_sold DESC    
LIMIT 10;

### **1️⃣2️⃣ 🔁 Repeat customers ?**
**🔰*Target these users with loyalty campaigns***

SELECT Customer_id,    
COUNT(*) AS Order_Count    
FROM Orders    
GROUP BY Customer_id    
HAVING COUNT(*) > 1    
ORDER BY Order_Count DESC;


### **1️⃣3️⃣ ⏳ Repeat customers Having the Most Order Counts ?** 
**🔰*Target this users with loyalty campaigns***

WITH TopCustomer AS (  
    SELECT Customer_id,    
COUNT(*) AS Order_Count    
FROM Orders     
GROUP BY Customer_id    
ORDER BY Order_Count DESC     
LIMIT 1   
)
SELECT C.x 
FROM Customers C    
JOIN TopCustomer T ON C.Customer_id = T.Customer_id;


### **1️⃣4️⃣⏳ Orders placed in the last 30 days ?**
**🔰*Get recent activity or new trends***

SELECT *     
FROM Orders   
WHERE order_date >= CURRENT_DATE **-** INTERVAL '30 Days';


## **🔴 Advanced Level Queries (15–20)**
### **1️⃣5️⃣📬 Customers with no orders (abandoned signups) ?**
**🔰*Reach out with personalized emails to re-engage.***

SELECT C.customer_id, C.name    
FROM Customers C   
LEFT JOIN Orders O ON C.customer_id = O.customer_id    
WHERE O.order_id IS NULL;


### **1️⃣6️⃣📊 Profit by product (requires cost_price in Products) ?**
**🔰*Determines which products generate the most profit.***

SELECT P.Product_Name   
, SUM((OI.Total_price - P.price) * OI.quantity) AS total_profit   
FROM Order_Items OI    
JOIN Products P ON OI.product_id = P.product_id   
GROUP BY P.Product_Name    
ORDER BY total_profit DESC;

### **1️⃣7️⃣ 🛍️ Frequently bought together products ?**
**🔰*Used to suggest combos or bundles.***

SELECT oi1.product_id AS product_1   
, oi2.product_id AS product_2    
, COUNT(*) AS frequency    
FROM Order_Items oi1    
JOIN Order_Items oi2 ON oi1.order_id = oi2.order_id AND oi1.product_id < oi2.product_id   
GROUP BY product_1, product_2   
ORDER BY frequency DESC    
LIMIT 10;


### **1️⃣8️⃣ 🕒 Average delivery time (requires delivery_date) ?**
**🔰*Monitors logistics performance.***

SELECT ROUND(AVG(S.delivery_date - O.order_date),3) AS avg_delivery_days   
FROM Orders O    
JOIN Shipments S ON O.Order_ID = S.Order_ID    
WHERE delivery_date IS NOT NULL;

### **1️⃣9️⃣ 🔢 Product category contribution to revenue ?**
**🔰*Identify top-performing categories.***

SELECT P.category, SUM(OI.Quantity * OI.Total_price) AS revenue    
FROM Order_Items OI    
JOIN Products P ON OI.product_id = P.product_id     
GROUP BY P.category    
ORDER BY revenue DESC;


### **2️⃣0️⃣ 🔗 Most active days (peak order days)**
**🔰*Helps plan marketing and promotions around high-traffic days.***

SELECT Order_date, COUNT(*) AS Total_Orders   
FROM Orders    
GROUP BY Order_date   
ORDER BY Total_Orders DESC   
LIMIT 10;

# **✅ Online Retail Store File's Embedded**

### **🔰Online Retail Store related csv file's**

[customers.csv](https://github.com/user-attachments/files/20846818/customers.csv)

[order_items.csv](https://github.com/user-attachments/files/20846820/order_items.csv)

[orders.csv](https://github.com/user-attachments/files/20846821/orders.csv)

[payments.csv](https://github.com/user-attachments/files/20846822/payments.csv)

[products.csv](https://github.com/user-attachments/files/20846819/products.csv)

[shipments.csv](https://github.com/user-attachments/files/20846817/shipments.csv)




