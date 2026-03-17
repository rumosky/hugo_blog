---
title: "绕过校园网web认证"
slug: "34"
date: 2019-07-12T23:23:00+08:00
lastmod: 2019-07-12T23:23:00+08:00
categories: ["技术"]
tags: ["linux", "gitea", "git", "python", "Windows", "github"]
draft: false
---

## 前言

校园网流量非常贵，不知道其他学校是什么情况， 本校10元3G，每个月给2.5G免费流量，且晚上十二点到早上六点断网。 学校的宽带也很贵，20M的一个月60,10M的一个月50，晚上十一点半到早上六点断网。
 
> 本文的方法仅适用于绕过web端校园网认证，若校园网认证是客户端软件，则可能无法绕过，请自行尝试

### 原理

（不想了解的可以直接跳过，有兴趣的建议看看） 学校的校园网连接之后，就会连接到学校的DNS服务器，这时会给你的设备分配一个IP，此时我们已经有了IP地址，为什么不能上网呢，原因是我们通过此IP发送的请求，会先经过校园网的服务器，这个服务器会查看你是否登陆，如果没有登录，就会强制重定向到登录界面，如果登录，就会发送你的请求。 **所以如果我们让一台别的服务器（可上网的设备）来接受转发我们的请求，我们就可以绕过认证，实现不计费上网。**
 
### 准备

1. 一台可以上网的服务器
2. ssh软件（推荐xshell）
3. Softenter Server软件
4. openVPN GUI软件


### 步骤

请严格按照步骤执行，一般不会有问题，若出问题，请先检查是否严格按照本文步骤执行。 
 
#### 开放端口

通常（这里只是通常，实际根据你的情况而定）开启的端口为：
 
```bash
TCP 80
TCP 443
TCP 22
UDP 53
UDP 67
UDP 68
UDP 69
```

若实在嫌麻烦，可以直接开放全部TCP+UDP端口。 阿里云服务器请添加安全组规则，相应开放教程请自行百度。 

#### 服务器端配置

先在本地计算机上安装好`xshell`软件，新建一个连接，输入`IP`和`root`账户以及`密码`即可登录服务器。（不懂的请自行百度如何连接服务器） > 本文测试服务器系统为CentOS7.3 64位

以下是Linux命令，请严格按照命令执行。 
 
1.安装GCC环境 
 
```bash
yum install gcc gcc-c++ make tar -y
```

页面出现`complete!`即安装成功 
 
2.安装linux版的softenter 64位（什么？你不知道什么是64位系统，请关闭本页面）： 
 
```bash
wget https://github.com/SoftEtherVPN/SoftEtherVPN_Stable/releases/download/v4.28-9669-beta/softether-vpnserver-v4.28-9669-beta-2018.09.11-linux-x64-64bit.tar.gz
```

32位 
 
```bash
wget https://github.com/SoftEtherVPN/SoftEtherVPN_Stable/releases/download/v4.28-9669-beta/softether-vpnserver-v4.28-9669-beta-2018.09.11-linux-x86-32bit.tar.gz
```

3.解压softenter 
 
```bash
tar -zxvf softether-vpnserver-v4.28-9669-beta-2018.09.11-linux-x64-64bit.tar.gz
```

4.进入目录 
 
```bash
cd vpnserver
```

5.编译 
 
```bash
make
```

提示输入的时候，输入1然后回车，一共要输入3次，如图

![make之后的图](https://cdn.rumosky.com/usr/uploads/2019/07/1715502815.png) 
 
6.启动VPN 
 
```bash
./vpnserver start
```

如图：
 
![启动VPN](https://cdn.rumosky.com/usr/uploads/2019/07/1662760916.png) 
 
7.打开VPN命令行 
 
```bash
./vpncmd
```

输入1后回车，然后再次按两次回车，如图：

![VPN命令行配置](https://cdn.rumosky.com/usr/uploads/2019/07/1675992079.png) 
 
配置好之后的界面就是VPN命令行交互界面
 
8.设置VPN密码 
 
```bash
ServerPasswordSet
```

然后会提示输入两次密码，如图：

![设置VPN密码](https://cdn.rumosky.com/usr/uploads/2019/07/3757032849.png) 

（记住这个密码，马上要用到，为了小白方便区分，暂且叫为SoftEther VPN Server管理密码，**不是服务器root登录密码！**） 
 
##### 补充

之前有很多人反映装不上，所以此处附上两个opven一键安装脚本： 
 
脚本1： 
 
```bash
curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
chmod +x openvpn-install.sh
```

脚本2： 
 
```bash
wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh
```

> 这两个脚本请自行测试使用，有任何问题请到对应源码地址提交issue

#### Softenter Server配置

1.安装Softenter Server软件 在本地Windows计算机上安装Softenter Server管理工具
 
![安装界面](https://cdn.rumosky.com/usr/uploads/2019/07/2619528698.png) 
 
安装过程就不赘述 安装完成的界面 
 
![安装完成](https://cdn.rumosky.com/usr/uploads/2019/07/1241912034.png) 

2.设置VPN 

![新设置](https://cdn.rumosky.com/usr/uploads/2019/07/1329221317.png) 

点击确定后，点击连接

![设置新连接](https://cdn.rumosky.com/usr/uploads/2019/07/1072115995.png) 

点击关闭 

![点击否](https://cdn.rumosky.com/usr/uploads/2019/07/181413372.png) 
点击否 

3.按照图片顺序执行（点击红色框内的按钮）

![管理虚拟HUB](https://cdn.rumosky.com/usr/uploads/2019/07/2093058557.png)

![管理用户](https://cdn.rumosky.com/usr/uploads/2019/07/2810867352.png)

4.设置新用户 

![设置新用户](https://cdn.rumosky.com/usr/uploads/2019/07/1451007757.png)

![设置用户信息](https://cdn.rumosky.com/usr/uploads/2019/07/4112567983.png) 

当前我这里账号密码都设置为001（为了区别，暂且叫为用户账号密码） 点击确定后，用户001就创建成功了

![创建成功](https://cdn.rumosky.com/usr/uploads/2019/07/2638356198.png) 

5.开启NAT 

![虚拟NAT和虚拟DHCP服务器](https://cdn.rumosky.com/usr/uploads/2019/07/4219115882.png)

回到这个界面，点击虚拟NAT和虚拟DHCP服务器 

![启用secureNAT](https://cdn.rumosky.com/usr/uploads/2019/07/4240447444.png)

![点击确定](https://cdn.rumosky.com/usr/uploads/2019/07/2894507814.png)

6.生成配置文件 

![点击openVPN/MS-MSTTP设置](https://cdn.rumosky.com/usr/uploads/2019/07/3796180361.png) 

回到管理主界面，点击openVPN/MS-MSTTP设置 

![生成配置文件](https://cdn.rumosky.com/usr/uploads/2019/07/446680502.png)

`监听openVPN的UDP端口一般用53、67、68、69，若无法连接，请依次尝试这四个端口`

> 此时桌面会有一个OpenVPN服务器压缩包，下一步会用到

#### openVPN GUI配置

1.在Windows上安装openVPN GUI 
 
2.打开上一步生成的OpenVPN服务器压缩包，如图 
 
![openVPN配置文件](https://cdn.rumosky.com/usr/uploads/2019/07/3388507587.png) 
 
3.将上一步图中的文件复制到openVPNconfig目录下，如图
 
![openVPN配置文件目录](https://cdn.rumosky.com/usr/uploads/2019/07/199756568.png)
 
> openVPN GUI默认安装之后，config目录路径为`C:\Program Files\OpenVPN\config`

### 开始上网

配置好openVPN GUI之后，直接打开软件，在Windows系统的右下角会出现一个上着锁的电脑图标，鼠标放在图标上然后点击右键，选择连接，输入设置好的VPN账户和密码，然后点击确定，连接成功图标变为绿色 
 
## 连接不上问题排查

1.检查一下是否配置文件里是默认的1194，如图： （详见Softenter Server配置第6步）
 
![默认1194端口](https://cdn.rumosky.com/usr/uploads/2019/07/1997283588.png)
 
**如果是1194则需要另外再一次生成53/67/68/68等端口的文件** 
 
2.请检查服务器防火墙是否关闭 
 
**CentOS7的命令为**
 
```bash
#停止firewall
systemctl stop firewalld.service

#禁止firewall开机启动
systemctl disable firewalld.service

#查看默认防火墙状态
firewall-cmd --state
systemctl status firewalld.service
```

3.检查主机管理面板的端口规则是否添加
 
**比如宝塔面板、阿里云安全组等** 
 
4.更换端口，如53连接不上更换67/68/69等 
 
5.本地电脑是否启用TAP的网卡 若启用TAP网卡，界面如图：
 
![网卡状况](https://cdn.rumosky.com/usr/uploads/2019/07/784986682.png)
 
## 声明

> 本文只是做技术的分享，任何商业行为都与本文无关，小白可以看看，大佬请自行忽略。

#### 其他相关文章

这里分享一下看到的其他绕过校园网认证的优质文章，可以参考： 
 
1. [使用各种方式绕过校园网认证--然而还是没有绕过](https://xiaoyou66.com/archives/1703 "使用各种方式绕过校园网认证--然而还是没有绕过")
2. [UDP 53 免费上网、DNS 隧道经验谈](https://www.bennythink.com/udp53.html "UDP 53 免费上网、DNS 隧道经验谈")
3. [SoftEther VPN Server 安装笔记](https://www.bennythink.com/softether-vpnserver.html "SoftEther VPN Server 安装笔记")
4. [利用v2ray绕过校园网认证](https://yangwenqing.com/archives/622/ "利用v2ray绕过校园网认证")
5. [使用DNS隧道绕过校园网认证](https://imjad.cn/archives/lab/using-dns-tunnel-to-bypass-campus-network-authentication "使用DNS隧道绕过校园网认证")
6. [绕过校园网WEB认证dns2tcp实现](https://www.cnblogs.com/nkqlhqc/p/7805837.html "绕过校园网WEB认证_dns2tcp实现")