---

layout: post
title: 一些python小脚本
description: 记录一些平时写的和收集的python小脚本
tag: python

---

遍历当前文件夹，修改文件名中的指定字符串

    import os
    import os.path
    from sys import argv

    curDir = os.getcwd()
    script, oldId, newId = argv
    for parent, dirnames, filenames in os.walk(curDir):
        for filename in filenames:
            if filename.find(oldId)!=-1:
                newName = filename.replace(oldId, newId)
                print(filename, "-------->", newName)
                os.rename(os.path.join(parent, filename), os.path.join(parent, newName))


从xlxs中读取数据，其中第一列作为文件夹名，第二列作为第一列对应文件夹下所包含的所有文件，将原始目录下的文件，按xlxs表中的对应关系分类


| A | B |
| - | :-: |
| 1 | 1,2,3,4,5 |
| 2 | 6,7,8,9,10 |
| 3 |	11,12 |
| 4 | 13,14,15 |
| 5 |	16,17,18 |
| 6 |	19,20 |
| 7 |	21,22,23 |
| 8 |	24,25 |
| 9 |	26,27 |
| 10 |	28,29,30 |

    from openpyxl import load_workbook
    import shutil
    import os
    from sys import argv
    
    wb = load_workbook("sample.xlsx")
    ws = wb.active
    
    for i in range(1,11):
        dirname = str(ws.cell(row = i, column = 1).value)
        dst = str(argv[2]) + "/" + dirname
        os.mkdir(dst)
        filelist = ws.cell(row = i, column = 2).value
        dirpaths = filelist.split(",")
        for f_name in dirpaths:
            shutil.copy(str(argv[1]) + "/" + f_name, dst + "/")


smtp发送带附件邮件

    import smtplib
    from email.mime.text import MIMEText
    from email.mime.multipart import MIMEMultipart
    from email.utils import formataddr
     
    my_sender='发件人邮箱账号'     
    my_pass = '发件人邮箱密码'              
    my_user='收件人邮箱账号'      
    
    def mail():
        ret=True
        try:
            #创建一个带附件的实例
            message = MIMEMultipart()
            message['From'] = formataddr(["发件人",my_sender])
            message['To'] =  formataddr(["收件人",my_user])
            message['Subject'] = 'Python SMTP 邮件测试'
    
            #邮件正文内容
            message.attach(MIMEText('Python 邮件发送测试……', 'plain', 'utf-8'))
    
            # 构造附件，传送当前目录下的 test.txt 文件
            att = MIMEText(open('test.txt', 'rb').read(), 'base64', 'utf-8')
            att["Content-Type"] = 'application/octet-stream'
            # 这里的filename可以任意写，写什么名字，邮件中显示什么名字
            att["Content-Disposition"] = 'attachment; filename="test.txt"'
            message.attach(att)
     
            server=smtplib.SMTP_SSL("smtp.qq.com", 465)  # 发件人邮箱中的SMTP服务器，端口是465
            server.login(my_sender, my_pass)  # 括号中对应的是发件人邮箱账号、邮箱密码
            server.sendmail(my_sender,[my_user,],message.as_string())  # 括号中对应的是发件人邮箱账号、收件人邮箱账号、发送邮件
            server.quit()  # 关闭连接
        except Exception:  # 如果 try 中的语句没有执行，则会执行下面的 ret=False
            ret=False
        return ret
     
    ret=mail()
    if ret:
        print("邮件发送成功")
    else:
        print("邮件发送失败")




