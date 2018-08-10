# Windows下安装Mysql5.7

## 1. 下载

```
下载地址
https://dev.mysql.com/downloads/file/?id=478884
选择No thanks, just start my download.
```

## 2.设置系统环境变量

1. 新建环境变量  

​    MYSQL_HOME:G:\Program Files\mysql-5.7.21-winx64

2. 在path 后面添加 ;%MYSQL_HOME%\bin

## 3.添加Mysql配置文件

#### 3.1 在mysql安装文件根目录下添加**my.ini文件**

**my.ini内加入如下代码**

```properties
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[mysqld]
#设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=G:\Program Files\mysql-5.7.21-winx64
# 设置mysql数据库的数据的存放目录
datadir=G:\Program Files\mysql-5.7.21-winx64\data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

**请注意:**

basedir和datadir，请根据自己的实际安装目录进行修改

data和my.ini是自己新建 

## 4.安装数据库服务

#### 4.1 初始化数据库

```
mysqld --initialize --user=mysql --console
```

执行完成后最后一行会打印出临时密码假设为：-rypr)0oz3kU 

#### 4.2 安装数据库服务

```
G:\Program Files\mysql-5.7.21-winx64\bin>mysqld --install
```

**注意：**

**一定要以管理员身份运行cmd.exe（目录：C:\Windows\System32\cmd.exe）程序,然后执行上述指令，不然会报错**

## 5. 启动Mysql服务,修改初始配置

#### 5.1服务启动指令

```properties
#启动服务
net start mysql
#关闭服务
net stop mysql
#删除服务
mysqld --remove
#安装服务
mysqld --install
```

#### 5.2 连接Mysql

```properties
#链接Mysql
mysql -uroot -p
#修改初始密码
set password for root@localhost=password('你的密码');
```

**注意如果连接出现以下错误**：

 Access denied for user 'root'@'localhost' (using password: YES)

修改步骤如下：

```properties
#1.关闭mysql服务
net stop mysql
#2.用安全模式打开mysql，这个时候，光标会一直闪。注意，不要动，打开另一个命令行窗口。
mysqld --skip-grant-tables
#3.在新窗口中链接mysql，不用输入密码直接回车进入mysql
mysql -uroot -p  
#4.修改ROOT密码
flush privileges;
set password for root@localhost=password('你的密码');
flush privileges；

```







#### 5.3 Mysql的一些通用设置

```




```







































