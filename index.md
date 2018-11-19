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
	
	
##固定Mac##
*我发现很多人在重启后发现bxc会掉线，自己也是在掉线2天后跟群友讨论研究后找到了解决方法，主要是由于重启后mac发现了变化于是需要手动固定mac*
-----
	vi /etc/network/interfaces
	i    //按下i对文档进行编辑 
	////////////////////////////////////////////////
	# Wired adapter #1
	allow-hotplug eth0
	no-auto-down eth0
	iface eth0 inet dhcp
	pre-up ifconfig eth0 hw ether aa:bb:cc:dd:ee:ff  #//添加这行是你的有线eth0绑定Mac
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
	#allow-hotplug wlan0          	#//如果你是挂的无线Mac去掉这行的#号
	#iface wlan0 inet dhcp			#//如果你是挂的无线Mac地址去掉这行的#号
	#pre-up ifconfig eth0 hw wlan0 aa:bb:cc:dd:ee:ff  #//这行添加你无线wlan0绑定Mac也需要去掉#
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