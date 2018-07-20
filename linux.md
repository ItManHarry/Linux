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
