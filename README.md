# SQLchallenge-PizzaRunner
📍 Hi there! My name is Fer and I'm following the Case study #2 - https://8weeksqlchallenge.com/case-study-2/ by Data with Danny

![PizzaRunner_logo]()

## Table of contents
- [Introduction & Business Challenge](#introduction)
- [Skills showcased](#skills-showcased)
- [Dataset Overview](#dataset-overview)
- [Case Study Questions & Solutions](#case-study-questions--solutions)
- [Insights & Recommendations](#insights--recommendations)
- [Tools & Technologies Used](#tools--technologies-used)
- [Future Improvements](#future-improvements)

## Introduction
Danny’s got a passion for pizza 🍕—and a knack for thinking big. After seeing the potential in 80s retro styling and the growing trend of food delivery services, he came up with the idea of Pizza Runner. Combining fresh pizza with on-demand delivery, he built a mobile app, recruited runners, and got his pizza empire off the ground. Now, he needs some help optimizing his operations with data to take his business to the next level.
### Business Challenge
Danny’s goal is to fine-tune his operations by better understanding customer and runner behavior, improving delivery efficiency, and analyzing ingredients and pricing strategies. With data already collected, the next step is cleaning it up and applying some solid analysis to boost performance and grow the business📈.

## Skills showcased
- **Data Cleaning & Transformation**
  - Cleaned and formatted data from multiple tables (e.g., exclusions, extras).
  - Ensured data integrity for reporting on pizza orders, customer activities, and runner performance.
- **Advanced SQL Techniques**
  - Created joins and subqueries to answer complex business questions.
  - Used window functions (like RANK(), DENSE_RANK()) to analyze and rank customer purchases and runner performance.
- **Data Analysis & Problem Solving**
  - Analyzed customer ordering behavior, purchase frequency, and delivery times.
  - Optimized ingredient usage, pricing strategies, and delivery logistics.
- **Visualization & Reporting**
  - Presented key insights in an easily digestible format to support decision-making for operational improvements.

## Dataset Overview
The dataset consists of 5 primary tables:
- <code>runners</code>: Details about the delivery runners (ID, registration date).
- <code>customer_orders</code>: Orders placed by customers, including pizza type, exclusions, and extras.
- <code>runner_orders</code>: Assignment of orders to runners, with pickup times, distances, and cancellations.
- <code>pizza_names</code>: Names of available pizza types.
- <code>pizza_recipes</code>: Ingredients for each pizza.

This is the entity relationship diagram:
![Tables_relationship_diagram]()

### pizza_runner Database Schema
<details>
  <summary> 
    SQL Schema for <code>runners</code>, <code>customer_orders</code>, <code>runner_orders</code>, <code>pizza_names</code>, and <code>pizza_recipes</code> tables 
  </summary>

  ```SQL
    CREATE SCHEMA pizza_runner;
    SET search_path = pizza_runner; -- Create and populate tables
  ```

  ```SQL
    DROP TABLE IF EXISTS runners;
    CREATE TABLE runners (
      "runner_id" INTEGER,
      "registration_date" DATE
    );

    INSERT INTO runners
      ("runner_id", "registration_date")
    VALUES
      (1, '2021-01-01'),
      (2, '2021-01-03'),
      (3, '2021-01-08'),
      (4, '2021-01-15');
  ```

  ```SQL
    DROP TABLE IF EXISTS customer_orders;
    CREATE TABLE customer_orders (
      "order_id" INTEGER,
      "customer_id" INTEGER,
      "pizza_id" INTEGER,
      "exclusions" VARCHAR(4),
      "extras" VARCHAR(4),
      "order_time" TIMESTAMP
    );

    INSERT INTO customer_orders
      ("order_id", "customer_id", "pizza_id", "exclusions", "extras", "order_time")
    VALUES
      ('1', '101', '1', '', '', '2020-01-01 18:05:02'),
      ('2', '101', '1', '', '', '2020-01-01 19:00:52'),
      ('3', '102', '1', '', '', '2020-01-02 23:51:23'),
      ('3', '102', '2', '', NULL, '2020-01-02 23:51:23'),
      ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
      ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
      ('4', '103', '2', '4', '', '2020-01-04 13:23:46'),
      ('5', '104', '1', 'null', '1', '2020-01-08 21:00:29'),
      ('6', '101', '2', 'null', 'null', '2020-01-08 21:03:13'),
      ('7', '105', '2', 'null', '1', '2020-01-08 21:20:29'),
      ('8', '102', '1', 'null', 'null', '2020-01-09 23:54:33'),
      ('9', '103', '1', '4', '1, 5', '2020-01-10 11:22:59'),
      ('10', '104', '1', 'null', 'null', '2020-01-11 18:34:49'),
      ('10', '104', '1', '2, 6', '1, 4', '2020-01-11 18:34:49');
  ```

  ```SQL
    DROP TABLE IF EXISTS runner_orders;
    CREATE TABLE runner_orders (
      "order_id" INTEGER,
      "runner_id" INTEGER,
      "pickup_time" VARCHAR(19),
      "distance" VARCHAR(7),
      "duration" VARCHAR(10),
      "cancellation" VARCHAR(23)
    );

    INSERT INTO runner_orders
      ("order_id", "runner_id", "pickup_time", "distance", "duration", "cancellation")
    VALUES
      ('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),
      ('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),
      ('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),
      ('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),
      ('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),
      ('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),
      ('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),
      ('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),
      ('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),
      ('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null');
  ```

  ```SQL
    DROP TABLE IF EXISTS pizza_names;
    CREATE TABLE pizza_names (
      "pizza_id" INTEGER,
      "pizza_name" TEXT
    );

    INSERT INTO pizza_names
      ("pizza_id", "pizza_name")
    VALUES
      (1, 'Meatlovers'),
      (2, 'Vegetarian');
  ```

  ```SQL
    DROP TABLE IF EXISTS pizza_recipes;
    CREATE TABLE pizza_recipes (
      "pizza_id" INTEGER,
      "toppings" TEXT
    );
    INSERT INTO pizza_recipes
      ("pizza_id", "toppings")
    VALUES
      (1, '1, 2, 3, 4, 5, 6, 8, 10'),
      (2, '4, 6, 7, 9, 11, 12');
  ```

  ```SQL
    DROP TABLE IF EXISTS pizza_toppings;
    CREATE TABLE pizza_toppings (
      "topping_id" INTEGER,
      "topping_name" TEXT
    );
    INSERT INTO pizza_toppings
      ("topping_id", "topping_name")
    VALUES
      (1, 'Bacon'),
      (2, 'BBQ Sauce'),
      (3, 'Beef'),
      (4, 'Cheese'),
      (5, 'Chicken'),
      (6, 'Mushrooms'),
      (7, 'Onions'),
      (8, 'Pepperoni'),
      (9, 'Peppers'),
      (10, 'Salami'),
      (11, 'Tomatoes'),
      (12, 'Tomato Sauce');
  ```

</details>

## Case Study Questions & Solutions

### Pizza Metrics
**- How many pizzas were ordered?**
<details>
  <summary>
    SQL Query
  </summary>

  ```SQL
    
  ```
</details>

## Insights & Recommendations

## Tools & Technologies Used
- SQL (for querying and analysis)
- Tableau (for visualization)
- GitHub (for documentation)

## Future Improvements
- Develop a Tableau dashboard to visualize key metrics and trends in real-time.
- Implement predictive analytics to optimize delivery routes and improve customer experience.
