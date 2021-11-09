# Ubuntu

1. 设置root密码

	安装过程中，root密码未做设置，需要安装完成后进行设置密码
	登录系统，打开终端，输入命令：	
```
	$ sudo passwd root
```	
- 首先输入当前用户密码，然后两次输入要设置的root密码即可

2. 安装Python虚拟环境

	2.1. 使用apt安装
	
```
	sudo apt install virtualenv
	sudo apt install virtualenvwrapper
```
	
	2.2. 进入home目录，输入命令ls -al，找到.bashrc文件，修改.bashrc文件
	
```
	sudo gedit ~/.bashrc
```
	
	2.3. 在.bashrc文件末尾添加两行：
	
```
	export WORKON_HOME=$HOME/.virtualenvs  						#目录改成自己的
	source /usr/share/virtualenvwrapper/virtualenvwrapper.sh	#使用find命令找到virtualenvwrapper.sh文件路径替换掉
```

	2.4. 使用source命令生效环境变量
	
```
	source ~/.bashrc
```	

	2.5. 使用virtualenv命令创建虚拟环境
	
```
	mkvirtualenv pm
```

3. 安装配置pycharm

	3.1. 官方下载社区版本
	
	3.2. 使用tar命令解压
	
```
	tar -zxf ....
```

	3.3. 创建快捷启动
	
```
	sudo ln -s /home/dell/pycharm-community-2018.1.2/bin/pycharm.sh /usr/local/bin/pycharm
```

	3.4. 终端启动
	
```
	nohup pycharm &
```

4. 安装配置SSH

	4.1. 查看ssh是否安装(默认只安装了openssh-client)
	
```
	dpkg -l|grep ssh
```
	
	4.2. 安装server
	
```
	sudo apt install openssh-server
```

	4.3. 查看ssh服务是否启动
	
```
	ps -ef|grep ssh
```
	看到sshd证明ssh服务已启动
	
	4.4. ssh-server配置文件位于/etc/ssh/sshd_config，在这里可以定义SSH的服务端口，默认端口是22，你可以自己定义
	成其他端口号，如222。（或把配置文件中的”PermitRootLogin without-password”加一个”#”号,把它注释掉，再增
	加一句”PermitRootLogin yes”），然后重启SSH服务：
```
	sudo /etc/init.d/ssh stop
	sudo /etc/init.d/ssh start
```