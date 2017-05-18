---

layout: post
title: Mac终端使用Tips
description: 本文主要总结了一些常用的mac终端命令行操作。熟练使用 command line 可以大大提高工作效率。
category: blog

---

本文主要总结了一些常用的mac终端命令行操作。熟练使用 command line 可以大大提高工作效率。

## 个性化Mac终端
打开终端，首先映入眼帘的文字就是命令提示符了。提示符的状态是通过PS参数来设置的。基本的颜色代码和参数如下：

	# PS的作用
	PS1	提示文字和颜色
	PS2	指定光标	 
	# Reset
	ENDCOLOR='\e[0m'
	# Regular Colors
	Black='\e[0;30m'	# Black
	Red='\e[0;31m'		# Red
	Green='\e[0;32m'	# Green
	Yellow='\e[0;33m' 	# Yellow
	Blue='\e[0;34m'		# Blue
	Purple='\e[0;35m'	# Purple
	Cyan='\e[0;36m'		# Cyan
	White='\e[0;37m'	# White
	
	# 参数的含义
	\n 换行
	\r 回车
	\d 当前日期
	\t 当前时间（24小时制）
	\h 主机名（短）
	\u 当前用户名
	\$ root用户显示#,普通用户显示$
	\w 当前所在目录（短）
	
通过使用上面的代码和参数来实现自己想要的效果。具体操作如下:

打开终端，输入

	vi .bash_profile
	
在.bash_profile文件中，设置想要的提示符显示方式，如下是本人的一些配置：
	
	# xTerm-256color
	export LS_OPTIONS='--color=auto'
	export CLICOLOR='Yes'
	export LSCOLORS='Exfxcxdxbxegedabagacad'
	
	# Modify Terminal Prompt and Color
	case $(id -u) in
		0)
		STARTCOLOUR='\[\e[0;32m\]';
			;;
		*)
		STARTCOLOUR='\[\e[0;35m\]';
		;;
	esac
	ENDCOLOR="\[\e[0m\]"
	UNDERLINEBLUE="\[\e[4;34m\]"
	MYWORD="🐶 🐶 🐶 :"
	PS1="\n$STARTCOLOUR$MYWORD$ENDECOLOR $UNDERLINEBLUE\w$ENDCOLOR\n\$ ";
	
其中，`xTerm-256color`部分，是为了让执行 `ls `命令的时候不同的文件类型能够用颜色来区别，开启了 `ls Color`功能。同时，要确定 `偏好设置-描述文件-高级-Terminfo `声明终端为： `xterm-256color `

这之后重启终端后就可以看到效果了


## 常用终端命令

### man
大多数命令都会包含一个使用指南，会告诉你任何你需要知道的关于这个命令的所有细节，在命令行中输入 `man command-name `即可获取。

使用上下左右箭头来移动翻页，按` Q `来退出使用指南界面。

### pwd
显示当前目录的绝对路径。

### cd
改变当前目录到你指定的目录。如果你不指定，则会返回你的 home folder。`cd ..`返回上一个指定目录

### ls
查看指定目录下的文件。

`ls -la`可查看隐藏文件。添加了`-l`选项，目的是控制输出格式。如果你注意输出内容的话，会发现还包括.和..两项，它们分别表示当前文件夹和父文件夹。如果你不想显示这两项，只需要把`-a`改成`-A`即可。

### cat
`cat`是“concatenate”的意思，会按顺序读取文件并输出到 Terminal 窗口，语法为cat后接你需要查看的文件的路径。`cat`命令也可以用>>来增加文本文件的内容，例如命令`cat ../textOne.txt >> textTwo.txt`会把 textOne.txt 的内容添加到 textTwo.txt 的结尾。

### less
这个命令更适合用来查看长文本文件，因为它会允许你查找文本。语法为 `less`后接文件路径，和`cat`一样。用`less`命令打开的文件其实和你查看命令使用指南的时候使用的是一个查看器，所以操作是相同的，同样可以使用箭头来上下移动文本，使用空格来翻页，输入`/`和关键字来按照关键字搜索，按`Q`来退出使用指南页面。除此之外，按`V`键来使用`vi`文本编辑器。

### which
这个命令会定位某个命令的文件路径。换言之，它会告诉你你执行某个具体命令的时候，在使用哪个文件。语法为`which`后接某个命令。

### file
这个命令会尝试根据文件的内容输出文件类型。如果一个文件缺失了扩展名，那么这个命令可能会非常有用。语法为`file`后接文件路径。

### find
这个命令用来根据搜索关键词定位文件路径。 `find`命令不使用 Spotlight 搜索服务，但是它允许你设置非常具体的搜索条件，以及通配符。语法为`find`后接搜索的起始路径，后接定义搜索的选项，后接搜索内容（包含在引号里）。

### 使用通配符（Wildcard Characters）
下面是常用的通配符：

* 星号（＊，Asterisk）——代表任何长度的任何字符。例如`*.tiff`代表所有格式为tiff的文件。
* 问号（?，Question mark）——代表任何单个字符。例如`b?ok`匹配 book 但是不匹配 brook。
* 方括号（[]，Square brackets）——定义一定范围的字符，例如`[Dd]ocument`匹配 Document 以及 document；`doc[1-9]`匹配doc1, doc2, …, doc9。

配合使用上面三种通配符可以大大提高效率。

### mkdir
用来创建文件夹，语法为`mkdir`后接新文件夹的目录。可以用-p选项，来一起创建路径中不存在的文件夹（这样你就不用挨层创建了）。

### cp
用来把文件从一处复制到另一处。语法为`cp`后接原始路径，后接目标路径。如果你想复制整个文件夹和所有内容，需要添加`-R`选项。如果指定的目标路径不含文件名，则 cp 命令会按原名复制。如果指定的目标路径包括文件名，则会复制为你指定的文件名。如果仅指定新文件名，则会在原处以新名称创建文件副本。注意，系统会自动替换同名文件而不出现提示。

### mv
用来移动文件。语法为`mv`后接原路径，后接新路径。mv 的指定路径规则和 cp 是一样的（没错，如果仅指定新文件名，它就成了重命名命令）。

### rm
会永久删除文件。注意，command-line中没有废纸篓。语法为`rm`后接文件路径。然而，使用 rm 命令删除的文件有可能可以通过数据恢复工具恢复。如果希望安全删除文件，可以使用`srm`命令。

### rmdir和rm -R
rmdir是“remove directory”的缩写，这个命令会永久删除文件夹。再强调一遍，CLI 中木有废纸篓。语法为`rmdir`后接希望删除目录的路径。然而，rmdir 命令无法删除含有任何其他文件的文件夹，所以大多数情形下`rmdir`命令是不适用的。不过，你可以利用`rm`添加`-R`选项来删除文件夹及包含的所有文件。

### vi
vi 是 command line 中最常见的文本编辑器。用`vi`打开文本文件，只需要输入vi后接文件路径即可。Mac OS X 还提供了`nano`，一个更加现代的文本编辑器。它也更加方便，例如在底部有常用的快捷键列表.

和`less`命令类似，`vi`命令会占用整个 Terminal 空间来显示文件内容。打开后，在“command模式”，vi 会等你输入一些预定义字符来告诉 vi 你想做什么。你也可以使用键盘上的箭头键单纯地浏览文件。你想编辑时，按`I`开始（会进入编辑模式）。文字会插入到光标处。如果你想保存，需要先退出编辑模式进入 command 模式。方法是按下esc键。回到 command 模式后，输入`:wq`,回车。如果你不想保存，在 command 模式输入`:q!`，回车退出。

### sudo
用sudo执行一个命令会使用 root 账户权限。

### sqlite3
mac自带sqlite3命令行工具。

`sqlite3 filename.sqlitedb`连接到指定的数据库文件。

`sqlite>.exit`退出SQLite客户端软件。

`sqlite>.tables`显示用于创建一个表的SQL语句。

`sqlite>.dump table-name`将整个表的内容转储成SQL语句。

`sqlite>.output filename`将后续命令输出重定位到磁盘上的文件。

`sqlite>.headers on`打开表头显示。

`sqlite>select * from table-name；`执行SQL语句（注：sql语句最后要加";"）。

### nslookup
nslookup(name server lookup)是一个程序的名字，是一个用于查询Internet域名信息或诊断DNS服务器问题的工具。用户通过输入一个主机名可以发现相应的IP地址，相反的，输入IP地址可以找出主机名。用法如下：
	
	nslookup domain [dns-server]
	
如果没有指定dns-server，将会使用系统默认的服务器。

例如输入：

	nslookup www.baidu.com
	
返回的信息是：

	Server:		8.8.8.8
	Address:	8.8.8.8#53
	
	Non-authoritative answer:
	www.baidu.com	canonical name = www.a.shifen.com.
	Name:	www.a.shifen.com
	Address: 119.75.213.51
	Name:	www.a.shifen.com
	Address: 119.75.213.50
	
还可以指定一些其他参数，来查询更具体信息。例如：

	nslookup -qt=type domain [dns-server]
	
	nslookup –d [其他参数] domain [dns-server]
	
	
### kill
kill命令用来终止一个进程。用法为：

	kill [－s signal | －p] [－a] pid ...
	kill －l [signal]
	
其中`－s`指定发送的信号，`－p`模拟发送信号，`－l`：指定信号的名称列表，`pid`要中止进程的ID号，`signal`表示信号。

例如：

	kill 1105
	
将会终止PID为1105的进程。

### ping
ping命令用来检测主机网络接口状态，使用方法如下：

	ping [-dfnqrRv][-c][-i][-I][-l][-p][-s][-t] IP地址
	
`-d`：使用Socket的SO_DEBUG功能。
`-c`：设置完成要求回应的次数。
`-f`：极限检测。
`-i`：指定收发信息的间隔秒数。
`-I`：网络界面使用指定的网络界面送出数据包。
`-l`：前置载入，设置在送出要求信息之前，先行发出的数据包。
`-n`：只输出数值。
`-p`：设置填满数据包的范本样式。
`-q`：不显示指令执行过程，开头和结尾的相关信息除外。
`-r`：忽略普通的Routing Table，直接将数据包送到远端主机上。
`-R`：记录路由过程。
`-s`：设置数据包的大小。
`-t`：设置存活数值TTL的大小。
`-v`：详细显示指令的执行过程。
	
### shutdown
shutdown命令可用来设置mac的定时关机、重启、睡眠。用法如下：

	shutdown [-h | -r | -s] time

其中，h/r/s分别代表关机/重启/睡眠，time表示时间。如果要立即执行，可将time设置为now。如果，要取消的话，使用kill命令即可。

### mdfind
mdfind命令可使用Spotlight搜索服务来搜索关键字，它会返回所有匹配关键字的文件路径。用法如下：

	mdfind image  #查找包含image关键字的文件
	mdfind -onlyin ~ image	#查找~目录下，包含image关键字的文件
	mdfind -name stdlib.h	#查找名为stdlib.h的文件
	mdfind -live MyFavoriteAuthor	#持续查找MyFavoriteAuthor文件


## 参考文章

[《Mac OS X Terminal 101：终端使用初级教程》](https://www.renfei.org/blog/mac-os-x-terminal-101.html)

[《给 Terminal 施点魔法》](http://imwuyu.me/talk-about/modify-mac-osx-terminal-prompt-and-color.html)

[《常用cmd命令——nslookup命令》](http://www.cnblogs.com/earl86/archive/2009/02/06/1666439.html)