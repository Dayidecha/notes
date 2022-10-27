**创建数据库**

```sql
DROP DATABASE IF EXISTS learnjdbc;
CREATE DATABASE learnjdbc;
```

创建表

```sql
USE learnjdbc;
CREATE TABLE students(
  id BIGINT AUTO_INCREMENT NOT NULL PRIMARY KEY,
  NAME VARCHAR(50) NOT NULL,
  gender TINYINT(1) NOT NULL,
  grade INT NOT NULL,
  score INT NOT NULL
)ENGINE=INNODB DEFAULT
CHARSET=UTF8;
```











































q27p2c 

| 色域                                       |      |
| ------------------------------------------ | ---- |
| **色深**（8bit,10bit)                      |      |
| **亮度**（一般250，普通300，良好350，400） |      |
| **对比度**（1000：1够用，1500良好）        |      |
| 响应时间（5ms=200帧）                      |      |
|                                            |      |

