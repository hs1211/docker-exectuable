# Mysql Setting
mysql 설정 시에 권한이 없을 경우에 실패메지가 발생한다. 특히 nodejs 경우에는 'authenticate fail'이라는 메시지가 뜨면서 동작을 하지 않는 경우가 발생하는데 이는 권한 문제로 발생하는 경우가 많음.


## How to set up?
```bash

$ docker exec -it mysql1 mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.

mysql> CREATE USER 'admin'@'%' IDENTIFIED BY '1111';
Query OK, 0 rows affected (0.00 sec)

mysql> GRANT ALL PRIVILEGES ON * . * TO 'admin'@'%';
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```
