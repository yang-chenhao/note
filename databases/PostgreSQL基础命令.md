### 安装

首先，安装PostgreSQL客户端

```shell
sudo apt-get install postgresql-client
```

然后安装PostgreSQL服务器

```shell
sudo apt-get install postgresql
```

### 使用shell命令添加新用户和新数据库

PostgreSQL提供了命令行程序createuser和createdb

首先, 创建数据库用户dbuser, 并指定其为超级用户

```shell
sudo -u postgres createuser --superuser dbuser
```

然后登陆数据库控制台， 设置dbuser用户密码, 完成后退出控制台

```shell
sudo -u postgres psql
\password dbuser
\q
```

接着, 在shell命令行下创建数据库exampledb, 并指定所有者为dbuser

```shell
sudo -u postgres createdb -O dbuser exampledb
```

### 登陆数据库

添加新用户和新数据库以后，就要以新用户的名义登陆数据库，这时使用的是psql命令

```shell
psql -U dbuser -d exampledb -h 127.0.0.1 -p 5432
```

输入上面命令后，系统会提示输入密码。输入正确，就可以登陆控制台了。

psql命令存在简写形式。如果当前linux系统用户，同时也是PostgreSQL用户，则可以省略用户名。举例来说，我的Linux系统用户名为ruanyf，且PostgreSQL数据库存在同名用户，则我以ruanyf身份登录Linux系统后，可以直接使用下面的命令登录数据库，且不需要密码。

```shell
psql exampledb
```

### 控制台命令

控制台提供的一系列其他命令。

* \h：查看SQL命令的解释，比如\h select。
* \?：查看psql命令列表。
* \l：列出所有数据库。
* \c [database_name]：连接其他数据库。
* \d：列出当前数据库的所有表格。
* \d [table_name]：列出某一张表格的结构。
* \du：列出所有用户。
* \e：打开文本编辑器。
* \conninfo：列出当前数据库和连接的信息。

### 数据库操作

```SQL
# 创建新表 
CREATE TABLE user_tbl(name VARCHAR(20), signup_date DATE);

# 插入数据 
INSERT INTO user_tbl(name, signup_date) VALUES('张三', '2013-12-22');

# 选择记录 
SELECT * FROM user_tbl;

# 更新数据 
UPDATE user_tbl set name = '李四' WHERE name = '张三';

# 删除记录 
DELETE FROM user_tbl WHERE name = '李四' ;

# 添加栏位 
ALTER TABLE user_tbl ADD email VARCHAR(40);

# 更新结构 
ALTER TABLE user_tbl ALTER COLUMN signup_date SET NOT NULL;

# 更名栏位 
ALTER TABLE user_tbl RENAME COLUMN signup_date TO signup;

# 删除栏位 
ALTER TABLE user_tbl DROP COLUMN email;

# 表格更名 
ALTER TABLE user_tbl RENAME TO backup_tbl;

# 删除表格 
DROP TABLE IF EXISTS backup_tbl;
```