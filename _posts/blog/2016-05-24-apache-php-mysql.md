---

layout: post
title: 在OSX 10.11 EI Capitan中运行Apache，MySQL，PHP和phpMyAdmin
description: 在OSX 10.11 EI Capitan中，系统已经自带Apache和PHP，本文将介绍如何在OSX 10.11 EI Capitan系统中搭建Apache、PHP、MySQL以及phpMyAdmin。
category: blog

---

在OSX 10.11 EI Capitan中，系统已经自带Apache和PHP，本文将介绍如何在OSX 10.11 EI Capitan系统中搭建Apache、PHP、MySQL以及phpMyAdmin。

## Apache
在OSX 10.11 EI Capitan中，系统已经自带Apache，我们可以用终端来管理它。

启动Apache：

	sudo apachectl start
	
停止：

	sudo apachectl stop
	
重启：

	sudo apachectl restart
	
查看Apache版本：

	httpd -v
	
在启动Apache后，在浏览器中输入`http://localhost`，你会看见“It works！”字样。
你可以查看Apache的配置文件是否有错，通过输入如下命令：

	apachectl configtest
	
在`http://localhost`中，你展示的文件位于`/Library/WebServer/Documents/`这个系统目录下，另一个web根目录是用户的`~/Sites`目录，这个目录默认是不存在的，用户可以自己创建，创建完成之后，你会发现这个文件夹图标有一个特殊的唯一的图标，如下图所示：

![user-level-webroot-sites-folder](/images/apache_php_mysql/user-level-webroot-sites-folder.png)

之后，我们需要在`/etc/apache2/users/`目录下添加一个`username.conf`文件，具体做法如下，在终端输入如下命令：

	cd /etc/apache2/users
	
	sudo nano username.conf
	
然后在username.conf中输入如下代码：

	<Directory "/Users/username/Sites/">
	AllowOverride All
	Options Indexes MultiViews FollowSymLinks
	Require all granted
	</Directory>
	
其中`username`为当前账户的名称，之后按`ctrl`+`x`，再按`y`保存退出，该文件的属性应该如下
	
	-rw-r--r--  1 root  wheel  127  5 10 16:55 natsu.conf
	
输入`ls -l`可查看文件属性。

如果你的文件属性和上面有所不同，可通过如下命令来更改：

	sudo chmod 644 username.conf
	
接下来，你需要做的是打开`http.conf`并开放一些modules。

	sudo nano /etc/apache2/httpd.conf
	
取消以下modules的注释：(删除语句前面的`#`号)

	LoadModule authz_core_module libexec/apache2/mod_authz_core.so
	
	LoadModule authz_host_module libexec/apache2/mod_authz_host.so
	
	LoadModule userdir_module libexec/apache2/mod_userdir.so
	
	LoadModule include_module libexec/apache2/mod_include.so
	
	LoadModule rewrite_module libexec/apache2/mod_rewrite.so
	
如果你想运行php的话，你还需要取消这行代码的注释：

	LoadModule php5_module libexec/apache2/libphp5.so
	
之后，你需要取消如下的注释，来允许用户文件路径。

	Include /private/etc/apache2/extra/httpd-userdir.conf
	
打开另一个Apache配置文件：

	sudo nano /etc/apache2/extra/httpd-userdir.conf
	
并取消如下内容的注释：

	Include /private/etc/apache2/users/*.conf
	
保存并退出nano，之后重启Apache。

	sudo apachectl restart
	
在浏览器输入`http://localhost/~username/`，你会看见如下页面：

![osx-yosemite-user-site](/images/apache_php_mysql/osx-yosemite-user-site.png)

## PHP
要使用php，只需要在`httpd.conf`中取消一行注释内容即可：
打开`httpd.conf`：
	
	sudo nano /etc/apache2/httpd.conf
	
取消如下内容的注释：

	LoadModule php5_module libexec/apache2/libphp5.so
	
之后，重启Apache即可。

## MySQL
在OSX 10.11 中并不自带MySQL，因此你需要<a href="http://dev.mysql.com/downloads/mysql/">下载</a>相应版本的MySQL,直接选择`DMG Archive`格式的压缩包下载即可。之后按照步骤安装，安装途中会出现如下弹窗，你需要记住这个初始密码（root@localhoost:后面的字符串）。

![mysql-root-password](/images/apache_php_mysql/mysql-root-password.png)

打开`系统偏好设置`,在最底下的那一排，你会发现`MySQL`的图标，点击它，之后点击`Start MySQL Server`,或者你可以直接通过命令行来开关MySQL。

打开：

	sudo launchctl load -F  /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
	
关闭：

	sudo launchctl unload -F  /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
	
查看版本信息：

	/usr/local/mysql/bin/mysql -v -u root -p
	
如果你不想每次使用`mysql`都输入这么长的路径，你可以将`mysql`的路径配置到你的`shell`路径中。

	cd ; nano .bash_profile
	
输入：

	export PATH="/usr/local/mysql/bin:$PATH"
	
之后使配置文件生效：

	source ~/.bash_profile
	
接下来你就可以直接使用如下命令了：

	mysql -v -u root -p
	
改变`MySQL`的密码方法如下：

	/usr/local/mysql/bin/mysqladmin -u root -p'temppassword' password 'newpassword'
	
解决2002 MySQL Socket中的问题：

	sudo mkdir /var/mysql
	
	sudo ln -s /tmp/mysql.sock /var/mysql/mysql.sock
	
## phpMyAdmin

下载[phpMyAdmin](https://www.phpmyadmin.net/downloads/),将解压后的文件夹`phpmyadmin`移动到`~/Sites`目录下。

创建config文件夹：

	mkdir ~/Sites/phpmyadmin/config
	
设置文件夹权限：

	chmod o+w ~/Sites/phpmyadmin/config
	
在浏览器中输入`http://localhost/~username/phpmyadmin/setup/`

选择`New server`

![phpmyadmin-set-new-server](/images/apache_php_mysql/phpmyadmin-set-new-server.png)

选择`Authentication`,输入user和password,之后点`Save`

![phpmyadmin-set-new-server-mysql-password](/images/apache_php_mysql/phpmyadmin-set-new-server-mysql-password.png)

回到之前页面，你会发现`servers`下多了一项，点击`Save`按钮。

![phpmyadmin-save-config](/images/apache_php_mysql/phpmyadmin-save-config.png)

回到之前创建的`config`文件夹，你会发现多了一个`config.inc.php`，将它移动到`/phpmyadmin`文件夹，之后删除空的`config`文件夹。

现在，打开`http://localhost/~username/phpmyadmin/`,你会发现你可以连接到MySQL数据库了。

![phpmyadmin-4-no-frames](/images/apache_php_mysql/phpmyadmin-4-no-frames.png)

要更新`phpmyadmin`的话，只需下载最新的版本，替换掉`config.inc.php`文件即可。

## Permissons
要想在本地没有权限限制的情况下运行一个网站，最好设置web根目录和其内容是可写入的。

如果，你有一个网站文件放在`~/Sites/testsite`下，你可以通过如下方法把它设置为可写的：

	sudo chmod -R a+w ~/Sites/testsite
	
如果你担心安全问题，不想让所有人都可以写入，你可以将所有者设置成`Apache _www`,但是在操作文件时你需要以管理员身份进行多次验证你不是所有者。你可以这样做：

	sudo chown -R _www ~/Sites/testsite
		
这将使文件夹的内容归Apache用户所有。

如果你的网站放在系统文件目录下，你还需要这样做：

	sudo chown -R _www /Library/WebServer/Documents/testsite

做完这些，你就可以在OSX EI Capitan本地运行一个AMP堆栈了。


<br/>
<br/>
原文地址：[https://coolestguidesontheplanet.com/get-apache-mysql-php-and-phpmyadmin-working-on-osx-10-11-el-capitan/](https://coolestguidesontheplanet.com/get-apache-mysql-php-and-phpmyadmin-working-on-osx-10-11-el-capitan/)