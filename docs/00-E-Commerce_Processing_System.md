<img src="https://data-analytics-fullstack-assets.s3.eu-west-3.amazonaws.com/M05-Python_Programming/M02-D01_ShopStream.png"/>

# E-Commerce Order Processing System

You work as a data analyst at **ShopStream**, an online retailer that processes thousands of orders daily. The operations team manually reviews each order for eligibility, applies discounts, validates inventory, and routes orders to fulfillment centers. This manual process causes delays and inconsistencies.

Your manager wants you to build an automated order processing system. The system must check order validity, apply business rules, calculate discounts, validate stock levels, and categorize orders by priority. You will use conditions to make decisions and loops to process multiple orders efficiently.

This notebook contains 8 tasks, covering conditions, loops, and control flow. Each task restates the requirements followed by a worked solution.

## Task 1: Validate customer eligibility

ShopStream requires customers to be 18 or older to place orders. Check if a customer born in 2008 can place an order. The current year is 2024.

Calculate the customer's age. Use an `if` statement to check if they meet the minimum age requirement of 18. Print an appropriate message indicating whether they can place an order.


```python
# Customer information
birth_year = 2008
current_year = 2024
minimum_age = 18

# Calculate age and check eligibility


# Create a if statement to check if the customer is old enough to place orders

```

## Task 2: Categorize order by value

ShopStream uses order value to determine shipping priority. Orders under \$50 ship standard. Orders between \$50 and \$200 ship expedited. Orders over \$200 ship premium with free shipping.

Use `if`, `elif`, and `else` to categorize each order and assign the appropriate shipping method. Print the shipping category and whether free shipping applies.

Test the logic with three order values:
- \$35.99
- \$175.50
- \$245.00


```python
# First order
order_value_1 = 35.99

# Create a if statement to check the shipping method based on the order value


# Second order
order_value_2 = 175.50

# Create a if statement to check the shipping method based on the order value

# Third order
order_value_3 = 245.00

# Create a if statement to check the shipping method based on the order value

```

## Task 3: Apply discount codes

ShopStream offers various discount codes. Code "SAVE10" gives 10% off orders over \$100. Code "WELCOME" gives \$15 off any order. Code "PREMIUM" gives 25% off orders over \$200.

Check whether each code is valid for the given order value. If valid, calculate the discount amount and final price. If invalid, print a message explaining why.

Test these scenarios:
- Order: \$150, Code: "SAVE10"
- Order: \$85, Code: "SAVE10"
- Order: \$150, Code: "WELCOME"
- Order: \$250, Code: "PREMIUM"


```python
# First scenario
order_value = 150
discount_code = "SAVE10"

# Use if/else statements to check if discount codes are valid depending on order_value.
# Compute final price and print a message


```


```python
# Second scenario
order_value = 85
discount_code = "SAVE10"

# Use if/else statements to check if discount codes are valid depending on order_value.
# Compute final price and print a message


```


```python
# Third scenario
order_value = 150
discount_code = "WELCOME"

# Use if/else statements to check if discount codes are valid depending on order_value.
# Compute final price and print a message


```


```python
# Fourth scenario
order_value = 250
discount_code = "PREMIUM"

# Use if/else statements to check if discount codes are valid depending on order_value.
# Compute final price and print a message


```

## Task 4: Process daily order batch

ShopStream receives order IDs numbered sequentially. Process all orders from 1001 to 1010. Use a `for` loop with `range()` to iterate through these order numbers.

For each order, print a processing message: "Processing order #[number]". This simulates the system handling each order in sequence.


```python
# Process orders 1001 through 1010

```

## Task 5: Calculate shipping costs

ShopStream charges shipping based on distance zones. Zone 1 costs \$5.99. Zone 2 costs \$8.99. Zone 3 costs \$12.99. Process orders for all three zones and calculate total shipping revenue.

There are 5 orders in Zone 1, 3 orders in Zone 2, and 2 orders in Zone 3. Use a `for` loop with `range()` to count orders in each zone. Calculate the shipping cost for each order and accumulate the total.

Print the shipping cost for each order and the final total revenue from all zones.


```python
# Shipping rates
zone_1_rate = 5.99
zone_2_rate = 8.99
zone_3_rate = 12.99

# Order counts per zone
zone_1_orders = 5
zone_2_orders = 3
zone_3_orders = 2

# Calculate total shipping revenue

```

## Task 6: Apply tiered loyalty discounts

ShopStream rewards loyal customers with tiered discounts based on purchase count:
- Bronze (1-5 purchases): 5% discount
- Silver (6-15 purchases): 10% discount
- Gold (16-30 purchases): 15% discount
- Platinum (31+ purchases): 20% discount

Process these customers and their purchase counts: 3, 12, 28, 45. Store purchase counts in a list and loop through it. Use `if`/`elif`/`else` inside the loop to assign the correct tier. Print each customer's tier and discount rate.


```python
# Customer purchase counts
purchase_counts = [3, 12, 28, 45]

# Determine tiers and discounts
for i, count in enumerate(purchase_counts, 1):
    # YOUR CODE HERE
```

## Task 7: Monitor inventory reorder points

Products need reordering when stock falls below the reorder point. Check these products and their current stock levels:
- Product A: 150 units, reorder point 200
- Product B: 75 units, reorder point 50
- Product C: 25 units, reorder point 30
- Product D: 180 units, reorder point 100

Store the data in lists for stock levels and reorder points. Use a `for` loop with `range()` to check each product by index. If stock is below the reorder point, print a reorder alert with the shortage. Count how many products need reordering.


```python
# Product data
product_names = ["Product A", "Product B", "Product C", "Product D"]
stock_levels = [150, 75, 25, 180]
reorder_points = [200, 50, 30, 100]

# Check reorder requirements
reorder_needed = 0

for i in range(len(product_names)):
  # YOUR CODE HERE
```

## Task 8: Process orders with multiple conditions

Orders must meet several requirements before shipping: payment verified, inventory available, address validated, and fraud check passed.

Process these orders with their status:
- Order 1: Payment OK, Inventory OK, Address OK, Fraud Check OK
- Order 2: Payment OK, Inventory Low, Address OK, Fraud Check OK
- Order 3: Payment OK, Inventory OK, Address Invalid, Fraud Check OK
- Order 4: Payment OK, Inventory OK, Address OK, Fraud Check Failed

Store the status for each check as booleans. Use a loop to process each order and check all conditions with `and`. If all pass, approve for shipping. If any fail, identify which checks failed and print specific error messages.


```python
# Order status data
order_ids = [1, 2, 3, 4]
payment_verified = [True, True, True, True]
inventory_available = [True, False, True, True]
address_validated = [True, True, False, True]
fraud_check_passed = [True, True, True, False]

# Process each order
for i in range(len(order_ids)):
    # YOUR CODE HERE
```
