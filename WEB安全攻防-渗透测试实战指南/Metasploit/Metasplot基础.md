# Metasploit模块
1. Auxiliaries 辅助模块
负责执行扫描，嗅探，指纹识别等相关功能以辅助渗透测试。
2. Exploit 漏洞利用模块
利用一个系统、应用或服务中的安全漏洞进行的攻击行为。
3. Payload 攻击载荷模块
成功渗透目标后，用于在目标系统上运行任意命令或者特定代码，在Metasploit框架中可以自由选择、传送和植入。
4. Post 后期渗透模块
主要负责获取到目标系统远程控制权限后，进行一系列的后渗透攻击动作。
5. Encoders 编码工具模块
负责在渗透测试中负责免杀，防止被安全设备或软件识别出来。
# 渗透攻击步骤
1. 扫描目标机系统，寻找可用漏洞
2. 选择并配置一个漏洞利用模块
3. 选择并配置一个攻击载荷模块
4. 选择一个编码即使，用来绕过杀毒软件的查杀
5. 渗透攻击
# 主机扫描
## 使用辅助模块进行端口扫描
     1.msfconsole
     2.search portscan
     3.use auxiliary/scanner/portscan/tcp
     4.show options
     5.set RHOST 169.254.170.249
     6.set PORTS 1-65535
     7.set THREADS 10
     8.show options
     9.run
## 常用的扫描模块及功能
p193
## 漏洞利用
  
     实验下载地址：https://sourceforge.net/projects/metasploitable/files/Metasploitable2
     默认的登录名和密码是msfadmin：msfadmin
