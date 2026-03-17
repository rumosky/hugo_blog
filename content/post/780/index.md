---
title: "Python批量发送HTML邮件"
slug: "780"
date: 2024-05-07T14:27:00+08:00
lastmod: 2024-05-07T14:55:37+08:00
categories: ["随笔"]
tags: ["python", "Windows", "邮件"]
draft: false
---

## 说明

前两天使用阿里邮件推送批量发送邮件，但是一直报错，550，今天用脚本批量发送一下。

### 内容

首先，附上代码

**config.json**

```json
{
  "SMTP": {
    "server": "smtp.qq.com",
    "port": 465,
    "username": "example@qq.com",
    "password": "yourpassword"
  },
  "EmailContent": {
    "subject": "中奖通知-如默星空",
    "html_message": "<table style=\"text-wrap: wrap; width: 1257.81px; height: 552.917px;\"><tbody><tr><td style=\"background: url(&quot;https://a.photo/images/2018/03/24/2017113018325846288465.png&quot;) rgb(250, 250, 250);\"><div style=\"border-radius: 10px; font-size: 13px; width: 666px; font-family: &quot;Century Gothic&quot;, &quot;Trebuchet MS&quot;, &quot;Hiragino Sans GB&quot;, 微软雅黑, &quot;Microsoft Yahei&quot;, Tahoma, Helvetica, Arial, SimSun, sans-serif; margin: 50px auto; border: 1px solid rgb(238, 238, 238); max-width: 100%; background: repeating-linear-gradient(-45deg, rgb(255, 255, 255), rgb(255, 255, 255) 1.125rem, transparent 1.125rem, transparent 2.25rem) rgb(255, 255, 255); box-shadow: rgba(0, 0, 0, 0.15) 0px 1px 5px;\"><div style=\"width: 664.667px; background: -webkit-linear-gradient(0deg, rgb(67, 198, 184), rgb(255, 209, 244)) rgb(73, 189, 173); color: rgb(255, 255, 255); border-radius: 10px 10px 0px 0px; height: 66px;\"><p style=\"margin-top: 0px; margin-bottom: 0px; font-size: 15px; word-break: break-all; padding: 23px 32px; background-color: rgba(255, 255, 255, 0.4); border-radius: 10px 10px 0px 0px;\"><a style=\"color: rgb(255, 255, 255);\" href=\"https://rumosky.com/\">如默星空</a>中奖通知</p></div><div style=\"margin: 40px auto; width: 598.198px;\"><p>您好</p><p style=\"margin-top: 20px; margin-bottom: 20px; background: repeating-linear-gradient(-45deg, rgb(255, 255, 255), rgb(255, 255, 255) 1.125rem, transparent 1.125rem, transparent 2.25rem) rgb(250, 250, 250); box-shadow: rgba(0, 0, 0, 0.15) 0px 2px 5px; padding: 15px; border-radius: 5px; font-size: 14px;\">恭喜您中奖<br/>奖品兑换码：88888888<br/>兑换地址：https://rumosky.com</p><p class=\"wrap\"><span class=\"w260\">如默星空</span><span class=\"w20\">&nbsp;</span><span class=\"wauto\">感谢您的支持</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</p><p class=\"wrap\">Kindest Regards<br/>Powered by <a style=\"color: rgb(18, 173, 219);\" href=\"https://rumosky.com/\">rumosky</a></p><p><a style=\"color: rgb(18, 173, 219);\" href=\"https://rumosky.com/statements.html\">[隐私政策]</a>&nbsp;|&nbsp;<a style=\"color: rgb(18, 173, 219);\" href=\"https://rumosky.com/about.html\">[关于我们]&nbsp;</a>&nbsp;|&nbsp;<a style=\"color: rgb(18, 173, 219);\" href=\"https://rumosky.com/donate.html\">[赞赏我们]</a>&nbsp;|&nbsp;<a style=\"color: rgb(18, 173, 219);\" href=\"https://rumosky.com/\">[返回首页]</a>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p></div></div></td></tr></tbody></table><p><br/></p>"
  },
  "Recipients": [
    "example@qq.com",
    "example@163.com",
    "example@outlook.com"
  ]
}
```

配置文件里面的HTML内容需要转义成json格式，保证格式正确，邮件内容如下图：

![中奖通知](https://cdn.rumosky.com/usr/uploads/2024/05/4028300366.png)

AutoSendMail.py

```py
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
import json


def send_email(subject, html_message, from_email, to_email, smtp_info):
    # 创建 MIME 对象
    msg = MIMEMultipart()
    msg["From"] = from_email
    msg["To"] = to_email
    msg["Subject"] = subject

    # 添加 HTML 邮件正文
    msg.attach(MIMEText(html_message, "html"))

    # 连接到 SMTP 服务器
    try:
        server = smtplib.SMTP_SSL(smtp_info["server"], smtp_info["port"])
        server.login(smtp_info["username"], smtp_info["password"])
        server.sendmail(from_email, [to_email], msg.as_string())
        server.quit()
        return f"Email to {to_email} sent successfully."
    except Exception as e:
        return f"Failed to send email to {to_email}: {str(e)}"


def load_config(config_path):
    # 加载配置文件
    with open(config_path, "r", encoding="utf-8") as file:
        config = json.load(file)
    smtp_info = config["SMTP"]
    email_content = config["EmailContent"]
    recipients = config["Recipients"]
    return smtp_info, email_content, recipients


config_path = "config.json"
smtp_info, email_content, to_emails = load_config(config_path)
from_email = smtp_info["username"]

# 发送邮件并打印每个发送的结果
for email in to_emails:
    result = send_email(
        email_content["subject"],
        email_content["html_message"],
        from_email,
        email,
        smtp_info,
    )
    print(result)

```

直接运行即可，用的库都是标准库，除非是精简版Python，否则都支持。

#### 延迟发送

上面的脚本可以发送，但一次性发送太多可能会触发退信，被对方邮箱标记为垃圾邮件，所以添加随机延迟发送，修改后的代码如下：

```py
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
import json
import time
import random


def send_email(subject, html_message, from_email, to_email, smtp_info):
    # 创建 MIME 对象
    msg = MIMEMultipart()
    msg["From"] = from_email
    msg["To"] = to_email
    msg["Subject"] = subject

    # 添加 HTML 邮件正文
    msg.attach(MIMEText(html_message, "html"))

    # 连接到 SMTP 服务器
    try:
        server = smtplib.SMTP_SSL(smtp_info["server"], smtp_info["port"])
        server.login(smtp_info["username"], smtp_info["password"])
        server.sendmail(from_email, [to_email], msg.as_string())
        server.quit()
        return True, f"Email to {to_email} sent successfully."
    except Exception as e:
        return False, f"Failed to send email to {to_email}: {str(e)}"


def load_config(config_path):
    # 加载配置文件
    with open(config_path, "r", encoding="utf-8") as file:
        config = json.load(file)
    smtp_info = config["SMTP"]
    email_content = config["EmailContent"]
    recipients = config["Recipients"]
    return smtp_info, email_content, recipients


config_path = "config.json"
smtp_info, email_content, to_emails = load_config(config_path)
from_email = smtp_info["username"]

# 发送邮件并打印每个发送的结果，添加随机的延时
total_emails = len(to_emails)
for index, email in enumerate(to_emails, 1):
    success, message = send_email(
        email_content["subject"],
        email_content["html_message"],
        from_email,
        email,
        smtp_info,
    )
    print(f"Sending email {index}/{total_emails}: {message}")

    if index < total_emails:  # 如果不是最后一封邮件，等待随机时间
        sleep_time = random.randint(0, 15)  # 生成一个0到15秒之间的随机整数
        print(f"Waiting for {sleep_time} seconds...")
        time.sleep(sleep_time)

```

## 最后

阿里云邮件推送批量发送免费总额度是2000封，但日免费额度只有200，超过两百收费。脚本发送则没有限制，但是建议分批分时段发送比较稳定。