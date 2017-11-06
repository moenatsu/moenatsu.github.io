---

layout: post
title: CocoaHTTPServer的简单使用
description: CocoaHTTPServer是Mac OS X和iOS平台上一个轻量级、可嵌入的HTTP服务器框架。它是一个开源的项目，支持异步socket、ipv4、ipv6以及SSL/TLS加密
tag: iOS

---


## 简介
[CocoaHTTPServer](https://github.com/robbiehanson/CocoaHTTPServer)是Mac OS X和iOS平台上一个轻量级、可嵌入的HTTP服务器框架。它是一个开源的项目，支持异步socket、ipv4、ipv6以及SSL/TLS加密。

使用CocoaHTTPServer框架，可以在iOS设备上建立一个本地服务器，当电脑和移动设备处于同一局域网时，就可以通过电脑访问iOS的服务器页面，并能够实现文件之间的传输。

## 使用
可使用cocoapods快速集成CocoaHTTPServer到项目中。

	pod 'CocoaHTTPServer'
	
也可以手动导入，手动导入步骤如下：

1.下载[CocoaHTTPServer](https://github.com/robbiehanson/CocoaHTTPServer)

2.将CocoaHTTPServer-master目录下的Core导入工程。 

3.打开Vendor，将其中的CocoaAsyncSocket、CocoaLumberjack文件夹导入。

开启服务器的代码如下：

	- (void)openHttpServer {
	    self.httpServer = [[HTTPServer alloc] init];
	    [self.httpServer setType:@"_http._tcp."];  // 设置服务类型
	//    [self.httpServer setPort:12345]; // 设置服务器端口
	    NSString *webPath = [[[NSBundle mainBundle] resourcePath] stringByAppendingPathComponent:@"Web"];
	    
	    NSLog(@"-------------\nSetting document root: %@\n", webPath);
	    // 设置服务器路径
	    [self.httpServer setDocumentRoot:webPath];
	    NSError *error;
	    if(![self.httpServer start:&error])
	    {
	        NSLog(@"-------------\nError starting HTTP Server: %@\n", error);
	    }
	    else {
	        NSLog(@"port %hu",[self.httpServer listeningPort]);
	        
	    }
	}
	
获取设备的ip地址的方法如下：

	+ (NSString *)deviceIPAdress {
	    NSString *address = @"an error occurred when obtaining ip address";
	    struct ifaddrs *interfaces = NULL;
	    struct ifaddrs *temp_addr = NULL;
	    int success = 0;
	    
	    success = getifaddrs(&interfaces);
	    
	    if (success == 0) { // 0 表示获取成功
	        
	        temp_addr = interfaces;
	        while (temp_addr != NULL) {
	            if( temp_addr->ifa_addr->sa_family == AF_INET) {
	                // Check if interface is en0 which is the wifi connection on the iPhone
	                if ([[NSString stringWithUTF8String:temp_addr->ifa_name] isEqualToString:@"en0"]) {
	                    // Get NSString from C String
	                    address = [NSString stringWithUTF8String:inet_ntoa(((struct sockaddr_in *)temp_addr->ifa_addr)->sin_addr)];
	                }
	            }
	            
	            temp_addr = temp_addr->ifa_next;
	        }
	    }
	    
	    freeifaddrs(interfaces);
	    return address;
	}
	
运行程序后，同一局域网的设备即可通过ip:port访问该设备的指定文件。

如果想要通过其他设备传输文件到本设备上，可参考如下操作：

1.打开Samples/SimpleFileUploadServer文件夹，将其中的MyHTTPConnection类文件、Web文件夹导入工程。 

2.在MyHTTPConnection.m的`- (void) processStartOfPartWithHeader:(MultipartMessageHeader*) header`方法中，将`uploadDirPath`改为你想要在iOS设备中存放接收的文件的位置即可。

3.在之前的`- (void)openHttpServer`方法中添加如下代码：

	[self.httpServer setConnectionClass:[MyHTTPConnection class]];
	
运行程序，同一局域网的设备访问ip:port将会看到如下界面：

![uploadfile](/images/cocoahttpserver/uploadfile.png)

选取文件点击`submit`之后，就可以将文件传输到上面`uploadDirPath`指定的路径下面。

该页面显示的就是Web文件夹下的index.html，可根据需要自行更改。

