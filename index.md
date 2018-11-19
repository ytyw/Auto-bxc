# N1-bxc-Wifi #
感谢各路大神提供的方案我只是做了一个小小的改动
## v1.0 ##
1. 修改bxc使用wifi绑定Mac的脚本下载地址：https://github.com/ytyw/Auto-bxc/blob/b18c1d45d19a7474b028a69f66d0e4b1a3e5ca6c/bxc.sh


使用方法：
-----
**加载启用无线模块** 

	modprobe dhd && echo dhd >> /etc/modules

**启用图形化界面 **

	armbian-config					

**连接wifi热点**
	
	选择Network--wlan0--wifi--可用wifi目录上下键盘选择，回车后输入密码--<ok> 
	如果没连上排查密码或者按多连接几次如果成功了就会在你连接的wiif上多一个< * >号**

# 安装bxc#
	
输入命令创建bxc目录并切换到该目录：

	mkdir bxc && cd bxc 
下载bxc-wifi脚本

	wget https://github.com/ytyw/Auto-bxc/blob/b18c1d45d19a7474b028a69f66d0e4b1a3e5ca6c/bxc.sh

赋予x权限

	chmod +x bxc.sh

安装并初始化博纳云客户端，初始化结束会提示输入Email和Bcode绑定设备。	
	
	./bxc.sh init

启动/关闭bxc脚本

	/root/bxc/bxc.sh start			启动脚本
	/root/bxc/bxc.sh stop			关闭脚本

开机自动/禁止后台启动
	
	./bxc.sh enable 	开启开机启动
	./bxc.sh disable	禁止开机启动

重启

	reboot
	
查看是bxc是否启动
	
	ps -u root

查看列表中是否有bxc-nwtwork bxc-worker 两个进程

	///////////
	  ...
	bxc-nwtwork   
	bxc-worker
	  ...
	//////////

# 固定Mac #

*我发现很多人在重启后发现bxc会掉线，自己也是在掉线2天后跟群友讨论研究后找到了解决方法，主要是由于重启后mac发现了变化于是需要手动固定mac*

-----
	vi /etc/network/interfaces
	i    //按下i对文档进行编辑 

	////////////////////////////////////////////////
	# Wired adapter #1
	allow-hotplug eth0
	no-auto-down eth0
	iface eth0 inet dhcp
	pre-up ifconfig eth0 hw ether aa:bb:cc:dd:ee:ff  #//添加这行是你的有线eth0绑定Mac （aa:bb:cc:dd:ee:ff换成bxc网站上绑定好）  
	#address 192.168.0.100
	#netmask 255.255.255.0
	#gateway 192.168.0.1
	#dns-nameservers 8.8.8.8 8.8.4.4
	#       hwaddress ether # if you want to set MAC manually
	#       pre-up /sbin/ifconfig eth0 mtu 3838 # setting MTU for DHCP, static just: mtu 3838

	# Wireless adapter #1
	# Armbian ships with network-manager installed by default. To save you time
	# and hassles consider using 'sudo nmtui' instead of configuring Wi-Fi settings
	# manually. The below lines are only meant as an example how configuration could
	# be done in an anachronistic way:
	# 
	#allow-hotplug wlan0    #//如果你是挂的无线Mac 上面的mac就不用加 去掉这行的#号
	#iface wlan0 inet dhcp	 #//如果你是挂的无线Mac地址去掉这行的#号
	#pre-up ifconfig wlan0 hw ether aa:bb:cc:dd:ee:ff  #//这行添加你无线wlan0绑定Mac（aa:bb:cc:dd:ee:ff换成bxc网站上绑定好） 需要去掉#
	#address 192.168.0.100
	#netmask 255.255.255.0
	#gateway 192.168.0.1
	#dns-nameservers 8.8.8.8 8.8.4.4
	#   wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
	# Disable power saving on compatible chipsets (prevents SSH/connection dropouts over WiFi)
	#wireless-mode Managed
	#wireless-power off
	////////////////////////////////////////////////
	
**编辑完成后按下 【Esc】键  继续输入 【：wq】 保存并退出编辑状态**
	
	apt-get install net-tools        //安装查询工具
	ifconfig -a  	//如果你连上了网就能看到mac地址了

然后reboot 重启 
输入账号和密码后再次 ifconfig -a 查看你修改后的mac是成功