# Linux文件权限与目录分配

- ls命令

  ls是list的意思，在于显示文件的文件名与相关属性。参数‘-al’则表示列出所有的文件详细的权限与属性。
  
  ls -al 显示的七列文件信息说明如下：
  
  第一列表示文件类型与权限
  
    共10个字符，第一个字符代表这个文件是目录/文件/链接文件：[d]表示是目录，[-]表示是文件，[l]表示是链接文件，[b]表示是设备文件里面的
    可供存储的接口设备，[c]则表示设备文件里面的串行端口设备，例如键盘/鼠标。
    接下来以三个为一组，且均为“rwx”的三个参数的组合，r表示可读，w表示可写，x表示可执行。第一组表示文件所有者的权限，第二组表示同用户组
    的权限，第三组表示其他用户组的权限。
    
  第二列表示有多少文件名连接到此节点
  
  第三列这个文件/目录的所有者账号
  
  第四列表示这个文件/目录所属账号所在的组
 
  第五列表示文件大小，单位为b
 
  第六列表示文件的创建或者最近修改的日期
 
  第七列表示该文件的文件名

- chgrp命令:修改文件所属用户组

  命令格式:chgrp [-R] 组名 dirname/filename
  
  例:
    chgrp users install.log 

- chown:改变文件所有者

  命令格式:
    
    chown [-R] 账号名称 文件或目录
    
    例:
      chown bin install.log
      
 - chmod:更改权限
 
    命令格式
  
    chmod [-R] xyz 文件/目录
    
    xyz的含义和之前提到的文件类型有关：rwx为一组，r是数字4，w是数字2，x是数字1，权限组合即使：r+w+x
    例如：rwxrw-r-x对应的值就是:765
   
# Linux目录配置标准:FHS
	
	FHS(Filesystem Hierarchy Standard)
	
	Linux各个目录说明：
	
	- /bin : 系统由很多放置执行文件的目录，但是/bin比较特殊。因为/bin放置的是在单用户维护模式下还能被操作的命令。在
	/bin下面的命令可以被root与一般账号所使用，主要有：cat,chmod,chown,date,mv,mkdir,cp,bash等常用命令。
	
	- /boot ： 这个目录主要放置开机会使用的文件，包括Linux内核文件以及开机菜单与开机所需配置文件等。Linux kernel
	常用的文件名为vmlinuz，如果使用的是grub这个引导装载程序，则还会存在/boot/grub这个目录。
	
	usr目录是UNIX Software Resource的缩写
	
	
   
# 网络命令

- 查看端口是否开启

  lsof命令：lsof -i:portNumber
  
  例：
    lsof -i:2888
    如果有输出，说明该端口已开放，否则就是没有开放。
    
 - 命令开启端口：
    
    1.开放端口命令： /sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT

    2.保存：    /etc/rc.d/init.d/iptables save

    3.重启服务：/etc/init.d/iptables restart

    4.查看端口是否开放：/sbin/iptables -L -n
    
 - 修改文件开启端口
 
    - 编辑iptables文件：
     
        vi /etc/sysconfig/iptables
    
    - 在 -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT 后面添加如下内容：
    
        -A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
     
    - 修改完保存退出，重启网卡服务：
    
        service iptables restart
      
     - 查看端口开放信息：
     
        service iptables status
      
- 起/停防火墙

  - 命令临时启停：
      
      service iptables start/stop
      
  - 命令永久启停：
  
      chkconfig iptables on/off
      
  - 查看防火墙状态：
  
      service iptables status
      
- 设置网卡信息(root用户)

  - 网卡信息存放在/etc/sysconfig/network-scripts下
  
    cd /etc/sysconfig/network-scripts
    
  - 编辑网卡信息
  
     vi ifcfg-eth0
     
     修改后的文件内容：
     
      DEVICE=eth0
      
      TYPE=Ethernet
      
      UUID=902998d3-1dd7-4843-9b22-d7ab56a4144b
      
      ONBOOT=yes          //默认为no，修改为yes
      
      NM_CONTROLLED=yes
      
      BOOTPROTO=none      //默认为dhcp，修改为none或者static
      
      HWADDR=08:00:27:62:08:61
      
      IPADDR=10.40.123.200 //填写IP地址
      
      PREFIX=24
      
      GATEWAY=10.40.123.254 //填写网关
      
      DNS1=10.40.128.21     //填写DNS
      
      DEFROUTE=yes
      
      IPV4_FAILURE_FATAL=yes
      
      IPV6INIT=no
      
      NAME="System eth0"
     
     修改完成后，保存退出
     
  - 重启网卡服务
  
    service network restart
