# WAF
## WAF分类
1. 软件型WAF
2. 硬件型WAF
3. 云WAF
4. 网站系统内置WAF
## WAF判断
1. SQLMap

       sqlmap.py www.baidu.com --identify-waf --batch
2. 手工判断

## 一些WAF的绕过
1. 大小写混写
2. URL全编码和url二次全编码
3. 替换关键字（双写）
4. 使用注释
 
       Union select 1,2,3,4
       变换成
       union/*2333*/select/*aaaaa*/1,2,3,4
5. 多参数请求拆分
对于多个参数拼接到同一条SQL语句中的情况，可以将注入语句分割插入。
例如：

       a=[input1]&b=[input2]
       将GET的参数a和参数b拼接到SQL语句中
       and a=[input1] and b=[input2]
       这时候就可以将注入语句拆分为:
       a=union/*&b=*/select 1,2,3,4
       最终将参数a和参数b拼接得到：
       and a=union /*and b=*/select 1,2,3,4
6. HTTP参数污染
HTTP参数污染是指同一参数出现多次，不同的中间件会解析为不同的结果。
  
       e.g.：url = www.baidu.com/index.php?&a=1&b=2
       IIS会解析为a=1,2,
       中简介不同，解析的就不同
7. 生僻函数

       e.g.：报错函数经常用polygon()替代updatexml()
       SELECT polygon ((select*from(select*from(select@@version)f)x));
8. 寻找网站源站IP
对于云WAF防护，只要找到网站IP地址就能通过IP访问网络从而绕过WAF
9. 注入参数到cookie中
代码中使用$_REQUEST获取参数，而$_REQUEST会依次从GET/POST/cookie中获取参数，如果WAF只检测了GET/POST而没有检测cookie，可以将注入语句放入cookie中进行绕过。


