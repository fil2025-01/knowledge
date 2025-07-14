Child Pages
1. [Page 1.1](/child/page-01.1.md)

Password for mysql
```
W&k9@pZ$v3!m
```

Create a user
```
CREATE USER 'fil'@'%' IDENTIFIED BY 'W&k9@pZ$v3!m';
```

Grant privileges
```
GRANT ALL PRIVILEGES ON `value_chain_api_01`.* TO 'fil'@'%';
```

Flush Privileges
```
FLUSH PRIVILEGES;
```

dumping table
```
mysqldump -u root -p value_chain_api_01 users > users.sql
```