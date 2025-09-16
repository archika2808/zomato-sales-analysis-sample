# Zomato Delivery Analytics


## Motivation:-
Food delivery platforms like **Zomato** handle **millions of orders daily**.  
However, **delays in food delivery** are one of the biggest reasons for **customer dissatisfaction and order cancellations**.  

The goal of this project is to:-
- Identify **patterns in delivery delays**
- Understand **which cuisines, cities, and time slots face the most problems**
- Suggest **data-driven solutions** to improve efficiency
- Showcase **SQL + Python + Business Thinking** for Data Analyst roles


## 📌 Project Overview:-
This project analyzes **Zomato food delivery performance** using both **SQL** and **Python**.  
The goal is to identify **delivery delays**, understand **which cuisines/cities are most affected**, analyze **customer behavior**, and propose **data-driven solutions**.  

The project demonstrates **end-to-end Data Analyst skills**:
- **SQL** for database creation and querying  
- **Python (Pandas, Matplotlib)** for analysis and visualization  
- **Business insights** to solve real-world problems  


##  Tech Stack:-
- **SQL** → Data storage and queries  
- **Python** → Data analysis and visualization  
  - `pandas` → data manipulation  
  - `matplotlib` → plotting charts  
- **GitHub** → version control & portfolio  


##  Dataset:-
A synthetic dataset of **50 Zomato food delivery orders** with:-
- `order_id` → unique order number  
- `customer_name` → customer details  
- `city` → location of delivery  
- `cuisine` → type of food  
- `order_time` → when the order was placed  
- `delivery_time` → actual delivery time (minutes)  
- `expected_time` → promised delivery time (minutes)  
- `price` → order value  


## SQL Analysis & Queries:-

### 1. Average delivery vs expected time per city
```sql
SELECT city,
       AVG(delivery_time) AS avg_delivery,
       AVG(expected_time) AS avg_expected,
       AVG(delivery_time - expected_time) AS avg_delay
FROM zomato_orders
GROUP BY city;

✅ Sample Output:-

City	Avg Delivery	Avg Expected	Avg Delay
Delhi	41.6	34.2	7.4
Mumbai	51.2	40.4	10.8
Bangalore	44.3	34.7	9.6
Hyderabad	53.8	44.2	9.6
Kolkata	60.5	47.8	12.7
Pune	46.5	37.0	9.5

Insight:-Some cities (like Kolkata & Hyderabad) show higher delays due to traffic and peak hour congestion.



2. Which cuisines face most delays

SELECT cuisine,
       COUNT(*) AS total_orders,
       AVG(delivery_time - expected_time) AS avg_delay
FROM zomato_orders
GROUP BY cuisine
ORDER BY avg_delay DESC;

✅ Sample Output:-

Cuisine	Total Orders	Avg Delay
Mughlai	6	13.2
Biryani	7	11.0
Chinese	8	9.5
Pizza	6	7.3
North Indian	7	6.8
South Indian	6	5.2
Burger	5	4.8

Insight:-

1. Mughlai & Biryani orders have higher delays (complex preparation).
2. Fast foods like Burger, South Indian show minimal delays.


3. Peak hours (Lunch vs Dinner vs Other):-

SELECT 
   CASE 
      WHEN EXTRACT(HOUR FROM order_time) BETWEEN 12 AND 14 THEN 'Lunch'
      WHEN EXTRACT(HOUR FROM order_time) BETWEEN 19 AND 22 THEN 'Dinner'
      ELSE 'Other'
   END AS time_slot,
   COUNT(*) AS orders,
   AVG(delivery_time - expected_time) AS avg_delay
FROM zomato_orders
GROUP BY time_slot;

Sample Output:-

Time Slot	Orders	Avg Delay
Lunch	15	6.2
Dinner	25	11.5
Other	10	5.4

Insight:-

1. Dinner slot (7–10 PM) has maximum delays due to high demand.
2. Lunch is relatively smoother.


4. Top 5 customers by spending:-

SELECT customer_name, SUM(price) AS total_spent
FROM zomato_orders
GROUP BY customer_name
ORDER BY total_spent DESC
LIMIT 5;

Sample Output:-

Customer Name	Total Spent
Rohit Yadav	1600
Neha Gupta	1450
Simran Kaur	1300
Priya Singh	1200
Rahul Verma	1150

Insight:- Identifies loyal high-value customers (potential for premium offers).


5. Orders delayed more than 10 minutes:-

SELECT order_id, customer_name, city, cuisine, delivery_time, expected_time, (delivery_time - expected_time) AS delay
FROM zomato_orders
WHERE (delivery_time - expected_time) > 10;

Sample Output:-

Order ID	Customer Name	City	Cuisine	Delivery Time	Expected Time	Delay
7	Rohit Yadav	Kolkata	Mughlai	65	50	15
11	Varun Malhotra	Hyderabad	Biryani	60	45	15
22	Kavya Agarwal	Bangalore	Mughlai	64	50	14
31	Rachna Gupta	Kolkata	North Indian	61	45	16
45	Kabir Anand	Mumbai	Mughlai	62	50	12

Insight:- Filters problematic orders for escalation & customer service recovery.


Python Visualizations:-

Average Delay Per City:
![Average delay per city](images/Average_delay_per_city.png)

Average Delay Per Cuisine:
![Average delay per cuisine](images/Average_delay_per_cuisine.png)

Orders by Time Slots:
![Orders by time slots](images/Orders_by_time_slots.png)

Top 5 Customers by Spending:
![Top 5 customers by spending](images/Top_5_customers_by_spending.png)


Libraries Used:-

import pandas as pd
import matplotlib.pyplot as plt


1. Average Delay per City

📌 Observation:- Delhi performs better, while Kolkata/Hyderabad suffer higher delays.

2. Average Delay per Cuisine

📌 Observation:-

1. Mughlai & Biryani → most delays (complex dishes).

2. Burgers & South Indian → least delay (quick prep).


3. Orders by Time Slot

📌 Observation:- Dinner time has the largest share of orders and highest delays.

4. Top 5 Customers by Spending

📌 Observation:- Helps Zomato identify loyal big spenders for loyalty programs.

Business Problem:-

1. Customers face frequent delays in specific cuisines (Mughlai, Biryani) and during peak hours (Dinner).
2. This can impact customer satisfaction and repeat orders.


Recommendations:-

1. Kitchen Optimization:-

Pre-prep ingredients for Mughlai/Biryani during peak hours.

2. Delivery Optimization:-

More delivery partners active during 7–10 PM.

AI-based route optimization for traffic-heavy cities (Hyderabad, Kolkata).

3. Customer Communication:-

Dynamic ETA updates during peak hours.

Offer coupons/refunds on delays >10 mins to retain trust.

4. Loyalty Program:-

Reward top spenders with exclusive offers.


Conclusion:-

This project shows how SQL + Python + Business Insights together can solve real-world food delivery problems.
