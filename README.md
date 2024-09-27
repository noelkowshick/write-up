SQL Injection Challenge Write-Up

Introduction

In this challenge, a vulnerable web application permitted unauthorized access to sensitive information due to an SQL injection flaw. SQL injection occurs when an attacker can insert or "inject" malicious SQL code into a query, thereby manipulating it in ways not intended by the developer. In this write-up, I will detail how I identified and exploited this vulnerability, along with recommended mitigations to prevent such attacks.

Step 1: Reconnaissance
The first step was to identify potential input points within the web application that could be susceptible to SQL injection. These typically include:Login forms
By monitoring the HTTP requests made by the web application during interactions, I was able to discern how the application processed user inputs

Step2: Testing for Vulnerabilities
I started by focusing on the login form, a common target for SQL injection attacks. The initial request I tested was:
Username: admin
Password: admin

When I submitted this request, the response indicated a failed login attempt. This suggested the query might look something like this:
SELECT * FROM users WHERE username = 'admin' AND password = 'admin';

I started by focusing on the login form, a common target for SQL injection attacks. The initial request I tested was:
Username: anyusername
Password: anything

Upon submission, the application returned a response indicating a failed login attempt. This suggested that the underlying SQL query might look something like this:
SELECT * FROM users WHERE username = 'anyusername' AND password = 'anything';

To test for SQL injection vulnerabilities, I experimented with an input that included a malicious payload in the password field:
Username: anyusername 
Password: anything' or 'x'='x

This modifies the query to:
SELECT * FROM users WHERE username = 'anyusername' AND password = 'anything' OR 'x'='x';

Since the condition OR 'x'='x' is always true, this manipulation causes the query to return all users associated with anyusername, effectively bypassing the password check. As a result, I was able to gain unauthorized access to the application.

Conclusion:
In this challenge, I successfully exploited an SQL injection vulnerability in a web application's login mechanism, granting me unauthorized access and allowing me to extract sensitive information. To prevent such vulnerabilities, employing prepared statements, validating inputs, and adhering to secure coding practices are essential.

References:
OWASP Foundation. (2021). SQL Injection. Retrieved from OWASP - Open Web Application Security Project.

SQL Injection - Wikipedia. (2024). SQL Injection. Retrieved from Wikipedia.

Veracode. (2023). Understanding SQL Injection. Retrieved from Veracode.

SANS Institute. (2022). SQL Injection Attack. Retrieved from SANS Institute.

Acunetix. (2023). What is SQL Injection?. Retrieved from Acunetix.

PortSwigger. (2023). SQL Injection Cheat Sheet. Retrieved from PortSwigger.

Mozilla Developer Network (MDN). (2023). Security: SQL Injection. Retrieved from MDN Web Docs.

