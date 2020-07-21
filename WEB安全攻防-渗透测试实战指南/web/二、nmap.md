# 二、nmap 介绍
## 2.1 扫描参数
### 2.1.1 扫描目标相关参数
1. -iL ：从文件中导入目标主机或目标网段。
2. -iR ：随机选取目标主机进行扫描。如果-iR指定为0，则一直扫描。
3. --exclude < host1 [，host2 ] [，host3 ]，... >：后面跟的主机或网段不在扫描范围内。
4. --excludefile ：导入文件中的主机或网段将不在扫描范围中。
### 2.1.2 扫描发现相关参数
1. -sL：List Scan（列表扫描），仅列举指定目标IP，不进行主机发现。
2. -sn ：Ping Scan，只进行主机发现，不进行端口扫描。
3. -Pn ：将所有指定主机看作已开启，不检测主机是否存活。
4. -PS/PA/PU/PY[ portlist ] ：使用TCP SYN/ACK或SCTP INIT/ECHO方式来发现。
5. -PE/PP/PM ：使用ICMP echo、timestamp、netmask请求包发现主机。
6. -PO[ prococollist ]：使用IP协议包探测对方主机是否开启。
7. -n/-R：-n表示不进行DNS解析；-R表示总是进行DNS解析。
8. --dns-servers < serv1 [，serv2 ]，...>：指定使用系统的DNS系统。
9. --traceroute：追踪每个路由节点。
### 2.1.3 常见的端口扫描方式
1. -sS/sT/sA/sW/sM ：指定使用TCP SYN/TCP connect()/ACK/TCP窗口扫描/Maimon scans的方式对目标主机进行扫描。
2. -sU：指定使用UDP扫描的方式确定目标主机的UDP端口状况。
3. -sN/sF/sX：指定使用TCP Null/FIN/Xmas scans密码扫描的方式协助探测对方的TCP端口状态。
4. --scanflags< flags >：定制TCP包的flags。
5. -sI < zombie host [:probeport ] >：指定使用Idle scan的方式扫描目标主机。
6. -sY/sZ：使用SCTP INIT/COOKIE-ECHO扫描SCTP协议端口开放情况。
7. -sO : 使用 IP protocol 扫描确定目标机支持的协议类型。
8. -b<FTP real host>：使用FTP bounce scan扫描方式。
### 2.1.4 端口参数和扫描顺序相关参数
1. -p<port ranges>：扫描指定参数。
2. -F：快速模式，仅扫描TCP100的端口。
3. -r：不随机扫描端口，默认是随机扫描的 
4. --top-port<number>：扫描开放概率最高的number个端口。
5. --port-ratio ：扫描指定频率以上的端口。与--top-port类似，这里以概率作为参数，概率大于--port-ratio的端口才被扫描。显然参数在0-1之间。
### 2.1.5 与版本侦测相关的参数
1. -sV：指定让nmap进行版本侦测
2. --version-intensity < level >：指定版本侦测的强度（0-9），默认为7。数值越高，探测出的服务器越准确，但运行时间也长。
3. --version-light：指定使用轻量级侦测方式（intensity 2）。
4. --version-all：尝试使用所有的probes进行侦测（intensity 9）。
5. --vension-trace：显示出详细的版本侦测过程信息。
6. --osscan-guess：推测操作系统检测结果，当namp无法确定所检测的操作系统，会尽可能提供最近的匹配，此参数nmap是默认的。
### 2.1.6 时间与性能
1. -T <0-5>:设置时序模板（更高更快）。
2. --min-hostgroup / max-hostgroup < size >：最小/最大并行主机扫描组大小。
3. --min-parallelism / max-parallelism < numprobes >：探针并行 。
4. --min-rtt-timeout / max-rtt-timeout / initial-rtt-timeout < time >： 指定探头往返时间。
5. --max-retries < tries >：扫描探针重发的端口盖数
6. --scan-delay / --max-scan-delay < time >：调整探针间的延迟。
7. --min-rate < number >：每秒发送的数据包不比< number >慢。
8. --max-rate < number >：发送包的速度不比 < 每秒 > 数字快。
### 2.1.7防火墙/IDS逃避和欺骗
1. -f；--mtu < val > ：指定使用分片、指定数据包的MTU。
2. -D < decoy1 , decoy2 [，ME ]，... >：使用诱饵隐蔽扫描。
3. -S < IP_Address >：源地址欺骗。
4. -e < interface >：使用指定的接口。
5. -g / --source-port < portnum >：使用指定源端口。
6. --proxies < url1，[ url2 ]，... >：使用HTTP或者SOCKS4的代理。
7. --data < hex string >：向发送的数据包追加自定义有效载荷。
8. --data-string < string >：添加一个自定义的ASCII字符串发送的数据包。
9. --data-length < num >：填充随机数据让数据包长度达到NUM。
10. --ip-options < options >：使用指定的 IP 选项来发送数据包。
11. --ttl < val >：设置 IP time-to-live 域。
12. --spoof-mac < mac address / prefix / vendor name >：MAC地址伪装。
13. --badsum ：使用错误的checksum来发送数据包 。
### 2.1.8 Nmap 输出
1. -oN ：将标准输出直接写入指定的文件。
2. -oX ：输出 xml 文件。
3. -oS ：将所有的输出都改为大写。
4. -oG ：输出便于通过bash或者perl处理的格式,非xml。
5. -oA < basename >：可将扫描结果以标准格式、XML 格式和Grep格式一次性输出。
6. -v ：提高输出信息的详细度。
7. -d level ：设置debug级别,最高是9。
8. --reason ：显示端口处于带确认状态的原因。
9. --open ：只输出端口状态为open的端口。
10. --packet-tra--webxml ：从 http://namp.org 得到 XML 的样式 。ce ：显示所有发送或者接收到的数据包。
11. --iflist ：显示路由信息和接口，便于调试。
12. --append-output ：追加到指定的文件。
13. --resume < filename >：恢复已停止的扫描。
14. --stylesheet < path / URL >：设置XSL样式表,转换XML输出。
15. --webxml ：从 http://namp.org 得到XML的样式。
16. --no-sytlesheet ：忽略 XML 声明的XSL样式表。
### 2.1.9 其他nmap选项
1. -6 ：开启IPv6
2. -A ：OS识别,版本探测,脚本扫描和traceroute。
3. --datadir < dirname >：说明用户 Nmap 数据文件位置。
4. --send-eth / --send-ip：使用原以太网帧发送/在原IP层发送。
5. --privileged ：假定用户具有全部权限。
6. --unprovoleged ：假定用户不具有全部权限，创建原始套接字需要root权限。


e.g.: nmap -T4 -A -V ip
-A:表示进攻性方式扫描
-T4:表示指定扫描过程使用的时序。共6个级别（0-5）级别越高扫描越快，但被发现的也快。
-v表示显示冗余信息，在扫描过程中显示扫描细节
## 2.2 nmap端口状态
1. open：开放的，表示应用程序正在监听该端口的连接，外部可以访问。
2. filtered：被过滤的，表示端口被防火强或其他网络设备阻止，不能访问。
3. closed：关闭的，表示目标主机未开启该端口。
4. unfiletered：被过滤的，表示nmap无法确定端口的状态。
5. open/filtered：开放的或被过滤的，namp不能识别。
6. close/filtered：关闭的或被过滤的，nmap不能识别。

## 2.3 常用方式
### 2.3.1 扫描单个地址
nmap ip
### 2.3.2 扫描多个地址
nmap ip1 ip2
### 2.3.3 扫描一个范围内的目标地址
nmap 192.168.0.1-255
### 2.3.4 扫描目标地址所在的某个网段
nmap 192.168.0.1/24
### 2.3.5 扫描主机列表targets.txt中的所有目标地址
nmap -iL targets.txt
### 2.3.6 扫描除某一个目标地址之外所有目标地址
nmap 192.168.0.1/24 -exclude 192.168.0.12
### 2.3.7 扫描除某一文件中的目标地址之外的目标地址
nmap 192.168.0.1/24 -exclusefile targets.txt
### 2.3.8 扫描某一目标地址的20、22、23、80
nmap 192.168.0.1 -p 20,22,23,80
### 2.3.9 对目标地址进行路由跟踪
nmap -traceroute 192.168.0.1
### 2.3.10 扫描目标地址所在C段的在线情况
nmap -sP 192.168.0.1/24
### 2.3.11 目标地址的操作系统指纹识别
nmap -O 192.168.0.1
### 2.3.12 目标地址提供的服务版本检测
nmap -sV 192.168.0.1
### 2.3.13 探测防火墙状态
nmap -sF -T4 192.168.0.1

## 2.4 nmap进阶
### 2.4.1 常见的脚本
1. -sC/--script=default：使用默认的脚本进行扫描
2. --script=<Lua script>：使用某个脚本进行扫描
3. --script-args=key1=value1,key2=value2...:该参数用户传递脚本里的参数，key1是参数名，该参数对应value这个值。用逗号连接。
4. --script-args-file=filename:使用文件为脚本提供参数。
5. --script-trace：如果设置此参数，则显示脚本执行过程中发送与接受的数据
6. --script-updatedb：在nmap的script目录中有script.db文件，该文件保存当前nmap可用的脚本，如果使用此参数则对nmap目录中的扩展脚本进行数据库更新。
### 2.4.2 进阶举例
#### 2.4.2.1 鉴权扫描
1. 使用--script=auth可以对目标主机或网段进行弱口令检测
2. e.g.：nmap --scrip=auth 192.168.0.1
#### 2.4.2.2 暴力破解攻击
1. nmap具有暴力破解功能，可对数据库、SMB、SNMP等进行简单的密码暴力破解
2. e.g.：nmap --script-brute 192.168.0.1
#### 2.4.2.3 扫描常见的漏洞
1. nmap具有漏洞扫描功能，可以检测目标主机或网段是否存在常见的漏洞
2. e.g.：nmap --script=vuln 192.168.0.1
#### 2.4.2.4 应用服务器扫描
1. nmap具备很多常见应用服务的扫描脚本，如VNC服务,MYSQL服务，telnet服务，Rsync服务等，
2. 以VNC服务为例e.g.: nmap --script=realvnc-auth-bypass 192.168.0.1
#### 2.4.2.5 探测局域网内更多服务开启情况
1. e.g.：nmap -n -p --script=broadcast 192.168.0.105
#### 2.4.2.6 whois解析
1. e.g.：nmap -script external baidu.com
