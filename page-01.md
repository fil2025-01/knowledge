password for mysql
```
W&k9@pZ$v3!m
```

Create a user
```
CREATE USER 'fil'@'%' IDENTIFIED BY 'W&k9@pZ$v3!m';
```

Grant privileges
```
GRANT ALL PRIVILEGES ON `value_chain_api`.* TO 'fil'@'%';
```


FLUSH PRIVILEGES;
