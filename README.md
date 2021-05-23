EagleOS 7.9 说明文档
# 一、系统简介
    EagleOS 7.9基于CentOS 7.9进行深度定制优化。
    EagleOS 7.9根据CentOS 7.9的Everything版本进行精简，全程完全自动化无人值守安装，支持大于2TB磁盘自动分区，集成安装568个软件包，包含了常用的工具及依赖库等，使装机效率更高效。
    EagleOS 7.9安装后自动进行主要如下几项系统优化：
    (1)禁用selinux
    (2)禁用ipv6
    (3)一些服务的禁用、内核参数等优化
    (4)集成epel源，更改系统默认yum源为163源，epel源为aliyun源
    (5)集成安装unrar、p7zip、pip等软件包
	(6)升級系統openssl版本至1.1.1b
    安装完成后，ssh端口：49156，默认ip：192.168.0.157，系统超级用户名：root，密码：www.ip40.com
# 二、系统安装说明   
## EagleOS系统iso文件下载
请到天翼云盘下载eagleos7.9.iso：
https://cloud.189.cn/t/22uiyazYrQzq (访问码:0zra)

## EagleOS系统安装方式
- 光盘安装:请将eagleos7.9.iso刻录成光盘，目标服务器设置从光驱启动进行安装。
- PXE网络安装（Linux）: 在Linux上安装syslinux，拷贝/usr/share/syslinux/pxelinux.0到tftp根目录下，其余略。
- PXE网络安装（Windows）:
    (1)将本光盘目录/images/pxeboot拷贝到windows系统任意目录下，直接双击本目录下的tftpd64.exe，即自动启动pxe服务器（含dhcp,tftp等服务），可修改相关配置，直接生效。
    (2)在windows系统下自建web服务，如win2019系统安装自带iis中的web角色，在web根目录如D:\web\pxe下建立如eagleos7.9子目录，将eagleos7.9.iso解压到此eagleos7.9子目录下，修改D:\web\pxe\eagleos7.9\images\pxeboot中10个eagleos7.9*.cfg及D:\web\pxe\eagleos7.9\images\pxeboot\pxelinux.cfg\default中的192.168.0.185为您的web服务器IP。
    (3)将如下代码存为web.config，保存于web根目录如D:\web\pxe下，以确保安装过程中能正常下载安装文件。
    (4)目标服务器设置从网络启动进行安装，即可开始从PXE安装EagleOS 7.9，当然目标服务器与您的web服务器必须网络是相通的。

---------------------------Start of web.config-------------------- 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <directoryBrowse enabled="true" ></directoryBrowse>
        <staticContent>
            <mimeMap fileExtension=".cfg" mimeType="application/octet-stream" ></mimeMap>
            <mimeMap fileExtension=".img" mimeType="application/octet-stream" ></mimeMap>
            <mimeMap fileExtension=".bz2" mimeType="application/octet-stream" ></mimeMap>
        </staticContent>
        <security>
            <requestFiltering allowDoubleEscaping="true"></requestFiltering>      
        </security>
    </system.webServer>
</configuration>
```  
---------------------------End of web.config---------------------- 
## EagleOS系统安装步骤
(1)	请先确定目标服务器已经配置好raid等磁盘配置，后续安装过程将清空硬盘数据并进行自动化分区，请确定目标服务器目标磁盘上如有重要数据已经备份到其他服务器!!!
(2)	安装过程将自动配置有连接网络(只要物理联通即可)的网卡，所以建议将网线接到第一网卡进行操作，特别是在PXE安装情况下，否则安装后您得手工编辑配置网络。
(3)	成功从光盘或pxe网络引导启动目标服务器后，您将看到如下的系统安装界面 (进入此界面时，请快速随意按下键盘方向键上下键，避免安装界面短暂的6秒延时超过而自动开始系统安装) :
![](http://127.0.0.1/server/index.php?s=/api/attachment/visitFile/sign/ffe9c5991017c9eb74618abc6eb0714f)
    安装启动界面安装选项说明：
    ①第一项是“Auto Install EagleOS System By www.xmyy.com”，安装过程中只需要您手工进行硬盘分区，格式化完成后的界面中指定好引导启动的硬盘，之后都是自动化无人值守安装操作。
    ②第二项是“Auto Install EagleOS and !Auto Partition! on sda”，在/dev/sda上进行系统安装。
    ③第三项是“Auto Install EagleOS and !Auto Partition! on sdb”，在/dev/sdb上进行系统安装。   
    ④第四项是“Auto Install EagleOS and !Auto Partition! on vda”，在/dev/vda上进行系统安装。
    ⑤第五项是“Auto Install EagleOS and !Auto Partition! on vdb”，在/dev/vdb上进行系统安装。   
    ⑥第六项是“Auto Install EagleOS with nginx By xmyy.com”，安装过程中只需要您手工进行硬盘分区，格式化完成后的界面中指定好引导启动的硬盘，之后都是自动化无人值守安装操作，安装完成后自动启动nginx服务。
    ⑦第七项是“Auto Install EagleOS,nginx and !Auto Partition! on sda”，在/dev/sda上进行系统安装，安装完成后自动启动nginx服务。
    ⑧第八项是“Auto Install EagleOS,nginx and !Auto Partition! on sdb”，在/dev/sdb上进行系统安装，安装完成后自动启动nginx服务。   
    ⑨第九项是“Auto Install EagleOS,nginx and !Auto Partition! on vda”，在/dev/vda上进行系统安装，安装完成后自动启动nginx服务。
    ⑩第十项是“Auto Install EagleOS,nginx and !Auto Partition! on vdb”，在/dev/vdb上进行系统安装，安装完成后自动启动nginx服务。   
备注:第(6)-(10)项，自动安装nginx，相关软件包是经手工编译优化安装的，对应软件包及版本为:openssl 1.1.1b、jemalloc 5.2.0、pcre 8.43、tengine 2.3.2。
(4)	安装界面默认停留在上面第一项。可根据需要选择相应安装选项按回车后开始安装。视硬件配置不同，安装过程15-30分钟左右。
(5)	自动化安装过程中将会出现这种界面:
![](http://127.0.0.1/server/index.php?s=/api/attachment/visitFile/sign/93eab5e54213c732bb15b55d138c2029)
直到最后出现黑色的登录界面，此时系统即成功安装完毕!
(6)	在登录界面中，输入用户名:root，密码: www.ip40.com，即可登录系统，然后修改网卡配置，假设您的内网IP是:192.168.29.100,网关是:192.168.29.254, 则可执行如下命令(注意您的服务器网卡名称如果不一样，需要修改如下红色网卡名称em1为您的服务器网卡名称)修改:
vi /etc/sysconfig/network-scripts/ifcfg-em1
```
# Generated by parse-kickstart
UUID="dfd48d4c-06d9-412a-9f1b-a67f2fa1f964"
DNS2="218.85.157.99"
DNS1="119.29.29.29"
IPADDR="192.168.29.100"
GATEWAY="192.168.29.254"
NETMASK="255.255.255.0"
BOOTPROTO="static"
DEVICE="em1"
ONBOOT="yes"
IPV6INIT="no"
```
需要修改的即为如上红色字样内容(UUID一行不同服务器是不同的值，此行保留不变或删除即可)，然后按键盘ESC键退出编辑状态，再输入:wq保存即可退出，然后执行命令: systemctl restart network重启网络服务即可。您就可以在本机电脑上通过SecureCRT或Xshell等远程登录软件进行远程登录此台服务器了!

感谢您的使用!有何建议和意见欢迎反馈给雄鹰:oktjp@163.com!

By Eagle
技术支持：www.xmyy.com
2021.05.23
