# SQL-Data-Analysis-Restaurants-Orders
Analyze order data to identify the most and least popular menu items and types of cuisine

**- OBJECTIVE 1** - Explore the Items table: first objective is to better understand the items table by finding the number of rows in the table, the least and most expensive items, and the item prices within each category.

1. Number of items on the menu: 32
```sql
SELECT COUNT(*)
FROM menu_items;
```
2. What are the least and most expensive items on the menu?
- The least expensive item: Edamame - Asian- $5.00
```sql
SELECT *
FROM menu_items
ORDER BY price
LIMIT 1;
```
- The most expensive item: Shrimp Scampi - Italian - $19.95
```sql
SELECT *
FROM menu_items
ORDER BY price DESC
LIMIT 1;
```
3. How many Italian dishes are on the menu? 9 
```sql
SELECT COUNT(category)
FROM menu_items
WHERE category = 'Italian';
```
4. What are the least and most expensive Italian dishes on the menu?
- The least expensive: Spaghetti - $14.50
```sql
SELECT item_name, price
FROM menu_items
WHERE category = 'Italian'
ORDER BY price
LIMIT 1;
```
- The most expensive: Shrimp Scampi - $19.95
```sql
SELECT item_name, price
FROM menu_items
WHERE category = 'Italian'
ORDER BY price DESC
LIMIT 1;
```
5. How many dishes are in each category?
American - 6
Asian - 8
Italian - 9
Mexican - 9
```sql
SELECT category, COUNT(item_name) AS num_dishes
FROM menu_items
GROUP BY category;
```
6. What is the average dish price within each category?
American - $10.10
Asian - $13.48
Italian - $16.75
Mexican - $11.80
```sql
SELECT category, AVG(price) AS avg_price
FROM menu_items
GROUP BY category;
```

**- OBJECTIVE 2** - Explore the orders table

1. What is the date range of the table? 01/01/2023 - 09/03/2023
```sql
SELECT MAX(order_date), MIN(order_date)
FROM order_details;
```
2. How many orders were made within this date range? 5370
```sql
SELECT COUNT(DISTINCT order_id)
FROM order_details;
```
3. How many items were ordered within this date range? 12234
```sql
SELECT COUNT(order_id)
FROM order_details;
```
4. Which orders had the most number of items?
order_id = 440, 330, 443, 3473, 4305, 4482, 2675, 1957 ordered 14 items
```sql
SELECT order_id, COUNT(item_id)AS num_items
FROM order_details
GROUP BY order_id
ORDER BY num_items DESC;
```
6. How many orders had more than 12 items? 23
```sql
SELECT COUNT(*) FROM (SELECT order_id, COUNT(item_id)AS num_items
FROM order_details
GROUP BY order_id
HAVING num_items>12) AS num_items;
```

**OBJECTIVE 3 -** Analyze customer behavior

1. What were the least and most ordered items? What categories were they in?
- The least ordered item: Chicken Tacos - Mexican - ordered 123 times
```sql
SELECT item_name, category, COUNT(item_id) AS num_orders
FROM order_details
CROSS JOIN menu_items
  ON order_details.item_id = menu_items.menu_item_id
GROUP BY item_name
ORDER BY num_orders
LIMIT 1;
```

- The most ordered item: Hamburger - American - ordered 622 times

```sql
SELECT item_name, category, COUNT(item_id) AS num_orders
FROM order_details
CROSS JOIN menu_items
  ON order_details.item_id = menu_items.menu_item_id
GROUP BY item_name
ORDER BY num_orders DESC
LIMIT 1;
```

2. What were the top 5 orders that spent the most money?

- order_id | total_spend 
440 - $192.15;
2075 - $191.05;
1957 - $190.10;
330 - $189.70;
2675 - $185.10;
  
```sql
SELECT order_id, SUM(price) AS total_spend
FROM order_details
CROSS JOIN menu_items
  ON order_details.item_id = menu_items.menu_item_id
GROUP BY order_id
ORDER BY total_spend DESC
LIMIT 5;
```

3. View the details of the highest spend order. Which specific items were purchased?

Steak Tacos - Mexican - $13.95;
Hot Dog - American - $9.00;
Spaghetti - Italian - $14.50;
Spaghetti & Meatballs - Italian - $17.95;
Spaghetti & Meatballs - Italian - $17.95;
Fettuccine Alfredo - Italian - $14.50;
Fettuccine Alfredo - Italian - $14.50;
Korean Beef Bowl - Asian - $17.95;
Meat Lasagna - Italian - $17.95;
Edamame - Asian - $5.00;
Chips & Salsa - Mexican - $7.00;
Chicken Parmesan - Italian - $17.95;
French Fries - American - $7.00;
Eggplant Parmesan - Italian - $16.95;
```sql
SELECT item_name, category, price
FROM order_details
CROSS JOIN menu_items
  ON order_details.item_id = menu_items.menu_item_id
WHERE order_id = 440;
```
