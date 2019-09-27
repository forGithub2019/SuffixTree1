# Useful tools


[github推荐合集](https://github.com/wizardforcel/markdown-simple-world/blob/master/1.md)

#scan工具   

1.Hydra（爆破工具）
很好用的爆破工具，kali中有集成，直接命令行输入：
```
hydra -l [username] -P [password_list_file] ip
```
即可使用。


2. Ncrack(之前尝试都无法启动，记下这个工具也许以后有用)
 Ncrack是一个高速的网络认证破解工具，它可以帮助企业测试所有的网络主机和网络设备的密码强度，提高企业网络的安全性。安全专业人员也可使用 Ncrack做渗透测试。   
Ncrack采用了模块化设计，类似Nmap的命令行语法和一个动态的引擎，该引擎是Ncrack可以依据不同的网络反馈自适应操作。Ncrack可以可靠的进行大规模网络主机安全审计。Ncrack支持windows和linux系统，并且可以测试暴力破解测试，支持的协议包括RDP、 SSH、 HTTP(s)、 SMB、 POP3(s)、 VNC、 FTP, SIP、 Redis、 PostgreSQL、 MySQL 以及Telnet.
运行环境：   
Linux
bsd 系统
Windows
Mac OS X
安装：
```
tar -xzf ncrack-0.5.tar.gz
cd ncrack-0.5
./configure
make
su root
make install
```
github地址：[link](https://github.com/nmap/ncrack)
(windows下 ncrack6.0, 7.0版本均无法启动；linux下make error。。。)

