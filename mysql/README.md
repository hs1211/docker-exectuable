# Mysql Setting
mysql 설정 시에 권한이 없을 경우에 실패메지가 발생한다. 특히 nodejs 경우에는 'authenticate fail'이라는 메시지가 뜨면서 동작을 하지 않는 경우가 발생하는데 이는 권한 문제로 발생하는 경우가 많음.

## Issue?
아래와 같은 문제가 발생할때는 권한으로 인한 문제이다.
```
➜ yarn run migrate
yarn run v1.12.3
$ NODE_ENV=develop node ./src/migrations
sequelize deprecated String based operators are now deprecated. Please use Symbol based operators for better security, read more at http://docs.sequelizejs.com/manual/tutorial/querying.html#operators node_modules/sequelize/lib/sequelize.js:242:13
{ SequelizeConnectionError: Client does not support authentication protocol requested by server; consider upgrading MySQL client
    at Utils.Promise.tap.then.catch.err (/Users/kim.hyunsung/superb/milano/node_modules/sequelize/lib/dialects/mysql/connection-manager.js:149:19)
    at tryCatcher (/Users/kim.hyunsung/superb/milano/node_modules/bluebird/js/release/util.js:16:23)
    at Promise._settlePromiseFromHandler (/Users/kim.hyunsung/superb/milano/node_modules/bluebird/js/release/promise.js:512:31)
    at Promise._settlePromise (/Users/kim.hyunsung/superb/milano/node_modules/bluebird/js/release/promise.js:569:18)
    at Promise._settlePromise0 (/Users/kim.hyunsung/superb/milano/node_modules/bluebird/js/release/promise.js:614:10)
    at Promise._settlePromises (/Users/kim.hyunsung/superb/milano/node_modules/bluebird/js/release/promise.js:690:18)
    at _drainQueueStep (/Users/kim.hyunsung/superb/milano/node_modules/bluebird/js/release/async.js:138:12)
    at _drainQueue (/Users/kim.hyunsung/superb/milano/node_modules/bluebird/js/release/async.js:131:9)
    at Async._drainQueues (/Users/kim.hyunsung/superb/milano/node_modules/bluebird/js/release/async.js:147:5)
    at Immediate.Async.drainQueues [as _onImmediate] (/Users/kim.hyunsung/superb/milano/node_modules/bluebird/js/release/async.js:17:14)
    at runCallback (timers.js:705:18)
    at tryOnImmediate (timers.js:676:5)
    at processImmediate (timers.js:658:5)
    at process.topLevelDomainCallback (domain.js:120:23)
  name: 'SequelizeConnectionError',
  parent:
   { Error: Client does not support authentication protocol requested by server; consider upgrading MySQL client
       at Packet.asError (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/packets/packet.js:684:17)
       at ClientHandshake.execute (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/commands/command.js:28:26)
       at Connection.handlePacket (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/connection.js:455:32)
       at PacketParser.onPacket (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/connection.js:73:18)
       at PacketParser.executeStart (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/packet_parser.js:75:16)
       at Socket.<anonymous> (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/connection.js:80:31)
       at Socket.emit (events.js:182:13)
       at Socket.EventEmitter.emit (domain.js:441:20)
       at addChunk (_stream_readable.js:283:12)
       at readableAddChunk (_stream_readable.js:264:11)
       at Socket.Readable.push (_stream_readable.js:219:10)
       at TCP.onStreamRead [as onread] (internal/stream_base_commons.js:94:17)
     code: 'ER_NOT_SUPPORTED_AUTH_MODE',
     errno: 1251,
     sqlState: '08004',
     sqlMessage:
      'Client does not support authentication protocol requested by server; consider upgrading MySQL client' },
  original:
   { Error: Client does not support authentication protocol requested by server; consider upgrading MySQL client
       at Packet.asError (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/packets/packet.js:684:17)
       at ClientHandshake.execute (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/commands/command.js:28:26)
       at Connection.handlePacket (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/connection.js:455:32)
       at PacketParser.onPacket (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/connection.js:73:18)
       at PacketParser.executeStart (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/packet_parser.js:75:16)
       at Socket.<anonymous> (/Users/kim.hyunsung/superb/milano/node_modules/mysql2/lib/connection.js:80:31)
       at Socket.emit (events.js:182:13)
       at Socket.EventEmitter.emit (domain.js:441:20)
       at addChunk (_stream_readable.js:283:12)
       at readableAddChunk (_stream_readable.js:264:11)
       at Socket.Readable.push (_stream_readable.js:219:10)
       at TCP.onStreamRead [as onread] (internal/stream_base_commons.js:94:17)
     code: 'ER_NOT_SUPPORTED_AUTH_MODE',
     errno: 1251,
     sqlState: '08004',
     sqlMessage:
      'Client does not support authentication protocol requested by server; consider upgrading MySQL client' } } 'Something went wrong with database update!'
✨  Done in 3.15s.
```

## How to set up?
```bash

$ docker exec -it mysql1 mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.

mysql> CREATE USER 'admin'@'%' IDENTIFIED BY '1111';
Query OK, 0 rows affected (0.00 sec)

mysql> GRANT ALL PRIVILEGES ON * . * TO 'admin'@'%';
Query OK, 0 rows affected (0.00 sec)

mysql> ALTER USER 'admin'@'%' IDENTIFIED WITH mysql_native_password BY '1111';
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```
