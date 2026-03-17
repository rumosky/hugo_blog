---
title: "Python实现Gitee码云webhook"
slug: "725"
date: 2023-11-23T11:25:00+08:00
lastmod: 2023-11-23T11:25:00+08:00
categories: ["技术"]
tags: ["git", "python"]
draft: false
---

## 前言

之前使用Vuepress做文档网站，每次更新之后都需要手动构建前端页面，繁琐，不方便，想起来webhook，自动配置之后发现还是方便，记录一下。

### 正文

众所周知，由于网络问题，GitHub总是出问题，我选择使用码云，脚本使用flask框架，先写了一个样板：

```py
import os
import time
import hmac
import hashlib
import base64
import urllib
from flask import Flask, request

app = Flask(__name__)

SECRET = 'mySecret'  # 替换为您的签名密钥

def generate_signature(timestamp):
    timestamp_str = str(timestamp)
    secret_enc = bytes(SECRET).encode('utf-8')
    string_to_sign = '{}\n{}'.format(timestamp_str, SECRET)
    string_to_sign_enc = bytes(string_to_sign).encode('utf-8')
    hmac_code = hmac.new(secret_enc, string_to_sign_enc, digestmod=hashlib.sha256).digest()
    signature = urllib.parse.quote_plus(base64.b64encode(hmac_code))
    return signature

@app.route('/webhook', methods=['POST'])
def handle_webhook():
    # 验证请求来源是否是Gitee
    if request.headers.get('User-Agent') == 'git-oschina-hook':
        # 验证签名
        timestamp = int(request.headers.get('X-Gitee-Timestamp'))
        actual_signature = request.headers.get('X-Gitee-Token')
        expected_signature = generate_signature(timestamp)
        if actual_signature != expected_signature:
            return '', 403  # 返回拒绝访问状态码

        # 在这里编写处理Webhook请求的代码逻辑
        data = request.json
        print('Received webhook request:', data)
        # 执行 git pull、pnpm install 和 pnpm build 等操作
        # ...

        return '', 200  # 返回成功状态码
    else:
        return '', 403  # 返回拒绝访问状态码

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000)
```

这里附上码云官网文档地址：[webhook密钥验证](https://help.gitee.com/webhook/how-to-verify-webhook-keys)

#### 第一个坑

代码写好了，放在服务器上运行，测试环境，可以使用`nohup python3 webhook.py > run.log &`命令，run.log是运行时的日志，通过这个文件可以看到运行状况，如果代码不正确，想要停止，先执行`ps -ef | grep webhook.py`，找到对应的PID，然后执行kill命令，再重新运行代码。

代码跑成功之后，再码云仓库设置里，添加webhook，添加好之后点击测试，会看到返回数据

上面的示例代码报403错误，跟码云后台校验不通过，初步判断问题出在对生成的签名进行url编码时，使用了错误的方法。在代码中，使用了`urllib.parse.quote_plus`对签名进行url编码，而实际上应该使用`urllib.parse.quote`方法。修改后代码如下：

```py
import os
import time
import hmac
import hashlib
import base64
import urllib
from flask import Flask, request

app = Flask(__name__)

SECRET = 'mySecret'  # 替换为您的签名密钥

def generate_signature(timestamp):
    timestamp_str = str(timestamp)
    secret_enc = SECRET.encode('utf-8')
    string_to_sign = '{}\n{}'.format(timestamp_str, SECRET)
    string_to_sign_enc = string_to_sign.encode('utf-8')
    hmac_code = hmac.new(secret_enc, string_to_sign_enc, digestmod=hashlib.sha256).digest()
    signature = urllib.parse.quote(base64.b64encode(hmac_code), safe='')
    return signature

@app.route('/webhook', methods=['POST'])
def handle_webhook():
    # 验证请求来源是否是Gitee
    if request.headers.get('User-Agent') == 'git-oschina-hook':
        # 验证签名
        timestamp = int(request.headers.get('X-Gitee-Timestamp'))
        actual_signature = request.headers.get('X-Gitee-Token')
        expected_signature = generate_signature(timestamp)
        if actual_signature != expected_signature:
            return '', 403  # 返回拒绝访问状态码

        # 在这里编写处理Webhook请求的代码逻辑
        data = request.json
        print('Received webhook request:', data)
        # 执行 git pull、pnpm install 和 pnpm build 等操作
        # ...

        return '', 200  # 返回成功状态码
    else:
        return '', 403  # 返回拒绝访问状态码

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=3002)
```

但是这样还是报403错误，再次排查，发现，是我错了。

> 主要取决于你是从哪里取他传的签名密钥了，如果你是从url参数那里取的就要url编码，如果你是从请求头那里取出来的就不用

所以，上述代码在请求头的内容没有url编码，所以需要去掉url编码，正确的代码如下：

```py
import os
import time
import hmac
import hashlib
import base64
import urllib
from flask import Flask, request

app = Flask(__name__)

SECRET = 'mySecret'  # 替换为您的签名密钥

def generate_signature(timestamp):
    timestamp_str = str(timestamp)
    secret_enc = SECRET.encode('utf-8')
    string_to_sign = '{}\n{}'.format(timestamp_str, SECRET)
    string_to_sign_enc = string_to_sign.encode('utf-8')
    hmac_code = hmac.new(secret_enc, string_to_sign_enc, digestmod=hashlib.sha256).digest()
    signature = base64.b64encode(hmac_code).decode()
    return signature

@app.route('/webhook', methods=['POST'])
def handle_webhook():
    # 验证请求来源是否是Gitee
    if request.headers.get('User-Agent') == 'git-oschina-hook':
        # 验证签名
        timestamp = int(request.headers.get('X-Gitee-Timestamp'))
        actual_signature = request.headers.get('X-Gitee-Token')
        expected_signature = generate_signature(timestamp)
        if actual_signature != expected_signature:
            return '', 403  # 返回拒绝访问状态码

        # 在这里编写处理Webhook请求的代码逻辑
        data = request.json
        print('Received webhook request:', data)
        # 执行 git pull、pnpm install 和 pnpm build 等操作
        # ...

        return '', 200  # 返回成功状态码
    else:
        return '', 403  # 返回拒绝访问状态码

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=3002)
```

这个代码就是合适的，只剩下添加webhook逻辑即可

#### 完整代码

为了方便以后调试，添加了日志输出，并且进行分割，防止日志过大，代码如下：

```py
import os
import time
import hmac
import hashlib
import base64
import urllib
import logging
import shutil
from logging.handlers import TimedRotatingFileHandler
from flask import Flask, request

app = Flask(__name__)

SECRET = 'mySecret'  # 替换为您的签名密钥

def generate_signature(timestamp):
    timestamp_str = str(timestamp)
    secret_enc = SECRET.encode('utf-8')
    string_to_sign = '{}\n{}'.format(timestamp_str, SECRET)
    string_to_sign_enc = string_to_sign.encode('utf-8')
    hmac_code = hmac.new(secret_enc, string_to_sign_enc, digestmod=hashlib.sha256).digest()
    signature = base64.b64encode(hmac_code).decode()
    return signature

# 创建运行日志记录器
run_logger = logging.getLogger('run_logger')
run_logger.setLevel(logging.INFO)

# 创建错误日志记录器
error_logger = logging.getLogger('error_logger')
error_logger.setLevel(logging.ERROR)

# 创建运行日志处理器，每天切割日志文件并保留30天
run_handler = TimedRotatingFileHandler('run.log', when='D', interval=1, backupCount=30)
run_handler.setFormatter(logging.Formatter('%(asctime)s - %(levelname)s - %(message)s'))
run_logger.addHandler(run_handler)

# 创建错误日志处理器，不进行日志文件切割
error_handler = logging.FileHandler('error.log')
error_handler.setFormatter(logging.Formatter('%(asctime)s - %(levelname)s - %(message)s'))
error_logger.addHandler(error_handler)

@app.route('/webhook', methods=['POST'])
def handle_webhook():
    # 验证请求来源是否是Gitee
    if request.headers.get('User-Agent') == 'git-oschina-hook':
        # 验证签名
        timestamp = int(request.headers.get('X-Gitee-Timestamp'))
        actual_signature = request.headers.get('X-Gitee-Token')
        expected_signature = generate_signature(timestamp)
        if actual_signature != expected_signature:
            return 'Validation failed', 403  # 返回拒绝访问状态码

        # 在这里编写处理Webhook请求的代码逻辑
        data = request.json
        run_logger.info('Received webhook request: %s', data)  # 记录运行日志
        try:
            run_logger.info('Inside try block')  # 添加调试语句
            # 执行 git pull、pnpm install 和 pnpm build 等操作
            os.chdir('/test')  # 切换到指定目录
            run_logger.info('Current working directory: %s', os.getcwd())  # 添加调试语句
            dist_directory = '/test/dist'
            if os.path.exists(dist_directory):
                run_logger.info('Directory exists: %s', dist_directory)
                shutil.rmtree(dist_directory)
                run_logger.info('Deleted: %s', dist_directory)
            else:
                run_logger.info('Directory not found: %s', dist_directory)
            os.system('git pull')  # 执行 git pull 命令
            run_logger.info('Executing git pull')  # 添加调试语句
            os.system('pnpm install')  # 执行 pnpm install 命令
            run_logger.info('Executing pnpm install')  # 添加调试语句
            os.system('pnpm docs:build')  # 执行 pnpm build 命令
            run_logger.info('Executing pnpm docs:build')  # 添加调试语句
            run_logger.info('Code execution completed.')  # 记录运行日志
        except Exception as e:
            error_logger.error('Error occurred during code execution: %s', str(e))  # 记录错误日志

        return 'Update success', 200  # 返回成功状态码
    else:
        return 'Update failed', 403  # 返回拒绝访问状态码

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000)
```

使用代码时请自行修改对应的目录路径，并且，配置好服务器端的git，保证可以正常的执行git clone等命令

#### 第二个坑

现在代码写好了，需要正式部署到服务器上，让码云可以正常的访问，这里我使用宝塔面板一键部署，简单高效。毕竟`nohup`还是太简单了，还需要守护进程才可以一直运行。

**注意**

宝塔面板软件商店里有一个Python项目管理器，左侧菜单点击网站，再点击Python项目选项卡，也是Python项目管理。两者的区别就是形式的区别，插件形式和集成形式，但这里推荐用集成的，因为亲测，插件形式部署之后，如果添加域名，会在网站那里添加一个映射网站，如果需要配置ssl，则需要单独在网站设置那里添加ssl，较为麻烦。而集成的这个直接点击设置，然后就可以添加域名和ssl证书，统一管理，方便很多，还不需要下载插件。

附上两种方式的截图：

![集成方式](https://cdn.rumosky.com/usr/uploads/2023/11/2153516543.png)

![插件方式](https://cdn.rumosky.com/usr/uploads/2023/11/3706865963.png)

运行的时候，一定注意用户组，用www用户还是root用户，www用户可能会报权限错误，依赖安装则需要requirements.txt，下面附上我的：

```txt
Flask==3.0.0
requests==2.31.0
```

部署好之后，可以绑定域名和ssl，这样就可以在码云后台绑定域名了，代码默认是有前缀webhook，所以最后的访问地址是：`http://yourdomain/webhook`

### 结语

至此，就可以正常使用了，点击测试或者提交代码，就会自动更新代码，构建前端页面，但是，还没完！

#### 第三个坑

这个是最坑的一个，可能也是个例，不能保证百分百会遇到，那就是我发现，我提交代码之后，或者在码云后台，连续点击两侧测试，则会导致服务器占用100%，连服务器都进不去了，ssh直接崩了，然后需要等待十几分钟，任务执行完毕，就正常了，我知道有人会说是你服务器配置不行，但不是的，经过我的排查，我发现是腾讯云盾这个服务占用过高，反正这个基础版也没啥用，果断卸载，然后再次测试，一点问题都没有，腾讯云，真的坑。