---

layout: post
title: python网络数据采集
description: python网络数据采集的学习笔记
tag: python

---

## BeautifulSoup
安装BeautifulSoup

	$ pip install beautifulsoup4
	
运行BeautifulSoup

	from urllib.request import urlopen
	from bs4 import BeautifulSoup
	html = urlopen("http://www.pythonscraping.com/pages/page1.html") 
	bsObj = BeautifulSoup(html.read(), "html.parser")
	print(bsObj.h1)
	
对于一个标签对象，可以用下面的代码获取它的全部属性:

	myTag.attrs
要注意这行代码返回的是一个 Python 字典对象，可以获取和操作这些属性。比如要获取图 片的资源位置 src，可以用下面这行代码:

	myImgTag.attrs["src"]
     
Lambda表达式
获取有两个属性的标签:

     soup.findAll(lambda tag: len(tag.attrs) == 2)
     
## 虚拟环境
用虚拟环境保存库文件:

当一个 Python 库不用虚拟环境安装的时候，你实际上是全局安装它。这通常需要有管理员权限，或者以root身份安装，这个库文件对设备上的每个用户和每个项目都是存在的。这时可以选择用虚拟环境。

首先安装virtualenv：

	$ pip install virtualenv
	
创建虚拟环境：

	$ virtualenv scrapingEnv		#创建一个叫作 scrapingEnv 的新环境
	
激活环境：

	$ cd scrapingEnv/
    $ source bin/activate
 
 退出环境：
 
 	$ deactivate
	
## 正则表达式

<table>
	<tr>
		<th>符号</th>
		<th>含义</th>
		<th>例子</th>
		<th>匹配结果</th>
	</tr>
	<tr>
		<td>*</td>
		<td>匹配前面的字符、子表达式或括号里的字符0次或多次</td>
		<td>a*b*</td>
		<td>aaaaaaaa，aaabbbbb， bbbbbb</td>
	</tr>
	<tr>
		<td>+</td>
		<td>匹配前面的字符、子表达式或括号里的字符至少1次</td>
		<td>a+b+</td>
		<td>aaaaaaab，aaabbbbb， abbbbbb</td>
	</tr>
	<tr>
		<td>[]</td>
		<td>匹配任意一个字符(相当于“任选一个”)</td>
		<td>[A-Z]*</td>
		<td>APPLE，CAPIT ALS， QWERTY</td>
	</tr>
	<tr>
		<td>()</td>
		<td>表达式编组(在正则表达式的规则里编组会优先运行)</td>
		<td>(a*b)*</td>
		<td>aaabaab，abaaab， ababaaaaab</td>
	</tr>
	<tr>
		<td>{m,n}</td>
		<td>匹配前面的字符、子表达式或括号里的字符 m 到 n次(包含m或n)</td>
		<td>a{2,3}b{2,3}</td>
		<td>aabbb，aaabbb，aabb</td>
	</tr>
	<tr>
		<td>[^]</td>
		<td>匹配任意一个不在中括号里的字符</td>
		<td>[^A-Z]*</td>
		<td>apple，lowercase， qwerty</td>
	</tr>
	<tr>
		<td>|</td>
		<td>匹配任意一个由竖线分割的字符、子表达式(注 意是竖线，不是大字字母 I)</td>
		<td>b(a|i|e)d</td>
		<td>bad，bid，bed</td>
	</tr>
	<tr>
		<td>.</td>
		<td>匹配任意单个字符(包括符号、数字和空格等)</td>
		<td>b.d</td>
		<td>bad，bzd，b$d，b d</td>
	</tr>
	<tr>
		<td>^</td>
		<td>指字符串开始位置的字符或子表达式</td>
		<td>^a</td>
		<td>apple，asdf，a</td>
	</tr>
	<tr>
		<td>\</td>
		<td>转义字符(把有特殊含义的字符转换成字面形式)</td>
		<td>\.\ | \\</td>
		<td>.| \</td>
	</tr>
	<tr>
		<td>$</td>
		<td>经常用在正则表达式的末尾，表示“从字符串的
末端匹配”。如果不用它，每个正则表达式实际都 带着“.*”模式，只会从字符串开头进行匹配。这 个符号可以看成是 ^ 符号的反义词</td>
		<td>[A-Z]*[a-z]*$</td>
		<td>ABCabc，zzzyx，Bob</td>
	</tr>
	<tr>
		<td>?!</td>
		<td>“不包含”。这个奇怪的组合通常放在字符或正则 表达式前面，表示字符不能出现在目标字符串里。 这个符号比较难用，字符通常会在字符串的不同 部位出现。如果要在整个字符串中全部排除某个 字符，就加上^和$符号</td>
		<td>^((?![A-Z]).)*$</td>
		<td>no-caps-here，$ymb0ls a4e f!ne</td>
	</tr>
</table>

## 使用Scrapy
安装：

	$ pip install Scrapy
	
创建新的Scrapy项目:

	$ scrapy startproject wikiSpider

运行：

	$ scrapy crawl article	#article由spiders文件中新建的aiticleSpider.py的name = "article" 决定
	
更多Scrapy的用法可参见这里：[http://doc.scrapy.org/en/latest/](http://doc.scrapy.org/en/latest/)


## urlopen

当使用`urlopen`出现如下问题时：

	urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed
	
解决方法如下：

	import ssl

	# This restores the same behavior as before.
	context = ssl._create_unverified_context()
	urllib.urlopen("https://no-valid-cert", context=context)

其他解决方法可参考[这里](https://stackoverflow.com/questions/27835619/urllib-and-ssl-certificate-verify-failed-error)

## Python Requests库

[Requests库](http://www.python-requests.org/)是一个擅长处理那些复杂的HTTP请求、cookie、header(响应头和请求头)等内容的 Python 第三方库。

安装

	$ pip install requests

提交一个基本表单

	import requests
	params = {'firstname': 'Ryan', 'lastname': 'Mitchell'}
	r = requests.post("http://pythonscraping.com/files/processing.php", data=params) print(r.text)

提交文件和图像

	import requests
     files = {'uploadFile': open('../files/Python-logo.png', 'rb')}
     r = requests.post("http://pythonscraping.com/pages/processing2.php",
                       files=files)
	print(r.text)
	
处理cookie（使用session函数）

	import requests
	session = requests.Session()
	params = {'username': 'username', 'password': 'password'}
	s = session.post("http://pythonscraping.com/pages/cookies/welcome.php", params) print("Cookie is set to:")
	print(s.cookies.get_dict())
	s = session.get("http://pythonscraping.com/pages/cookies/profile.php") 
	print(s.text)
	
HTTP基本接入认证

	import requests
	from requests.auth import AuthBase
	from requests.auth import HTTPBasicAuth
     auth = HTTPBasicAuth('ryan', 'password')
     r = requests.post(url="http://pythonscraping.com/pages/auth/login.php", auth=auth)
	print(r.text)
	
## 采集JavaScript

 在Python中用[Selenium](http://www.seleniumhq.org)执行JavaScript。可使用pip安装Selenium。
 Selenium 可以让浏览器自动加载页面，获取需要的数据，甚至页面截屏，或者判断网站上某些动作是否发生。
 
 Selenium 可与[PhantomJS] (http://phantomjs.org)结合使用。PhantomJS可从官网下载。
 
 下面的代码用PhantomJS库创建了一个新的Selenium WebDriver，首先用WebDriver加载页面，然后暂停执行 3 秒钟，再查看页面获取(希望已经加载完成的)内容。
 
	from selenium import webdriver
	import time
	driver = webdriver.PhantomJS(executable_path='PhantomJS 可执行文件的路径') 
	driver.get("http://pythonscraping.com/pages/javascript/ajaxDemo.html") time.sleep(3)
	print(driver.find_element_by_id('content').text)
	driver.close()

对于重定向问题，可以从页面开始加载时就“监视”DOM 中的一个元素，然后重复调用这个元素直到 Selenium 抛出一个 StaleElementReferenceException 异常；
也就是说，元素不在页面的 DOM 里了，说明这时网站已经跳转。

## 图像识别与文字处理（OCR）

安装Pillow（一个python图像处理库）

	pip install pillow

安装Tesseract。可使用homebrew进行安装。

	brew install tesseract
	
通过下面的命令运行 Tesseract，读取文件并把结果写到一个文本文件中:

     $tesseract text.png textoutput

当图片背景色是渐变的时候，处理结果会不是很理想，可使用python对图像处理后再进行识别。

	from PIL import Image 
	import subprocess
	def cleanFile(filePath, newFilePath): 
		image = Image.open(filePath)
		# 对图片进行阈值过滤，然后保存
		image = image.point(lambda x: 0 if x<143 else 255) 
		image.save(newFilePath)
		# 调用系统的tesseract命令对图片进行OCR识别 
		subprocess.call(["tesseract", newFilePath, "output"])
		# 打开文件读取结果
		outputFile = open("output.txt", 'r') 
		print(outputFile.read()) 
		outputFile.close()
     cleanFile("text_2.jpg", "text_2_clean.png")
     
