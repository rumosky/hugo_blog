---
title: "Mac安装nvm"
slug: "518"
date: 2023-03-02T23:47:00+08:00
lastmod: 2023-03-02T23:47:00+08:00
categories: ["技术"]
tags: ["nodejs", "nvm", "Apple", "苹果", "Mac"]
draft: false
---

## 说明

刚刚买了Mac mini，配置了一下环境，正好需要安装node，记录一下

### 步骤

本文使用brew安装，确保已经安装了brew包管理工具，若未安装，请参考：
[bspost cid="517"]

这里先记录一下[nvm官网](https://nvm.uihtm.com)，安装执行：

```bash
brew install nvm
```

安装成功会显示如下内容：

```bash
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api/formul
######################################################################## 100.0%
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api/cask.j
######################################################################## 100.0%
==> Fetching nvm
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/nvm-0.39.3
######################################################################## 100.0%
==> Pouring nvm-0.39.3.all.bottle.tar.gz
==> Caveats
Please note that upstream has asked us to make explicit managing
nvm via Homebrew is unsupported by them and you should check any
problems against the standard nvm install method prior to reporting.

You should create NVM's working directory if it doesn't exist:
  mkdir ~/.nvm

Add the following to your shell profile e.g. ~/.profile or ~/.zshrc:
  export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion

You can set $NVM_DIR to any location, but leaving it unchanged from
/opt/homebrew/Cellar/nvm/0.39.3 will destroy any nvm-installed Node installations
upon upgrade/reinstall.

Type `nvm help` for further information.
==> Summary
🍺  /opt/homebrew/Cellar/nvm/0.39.3: 9 files, 190.5KB
==> Running `brew cleanup nvm`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
```

根据上面的提示，将下面的内容添加到`.zprofile`文件

```bash
  export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion
```

添加完执行`source .zprofile`

此时，就安装好了，下面列出常用的命令

```bash
Example:
  nvm install 8.0.0                     Install a specific version number
  nvm use 8.0                           Use the latest available 8.0.x release
  nvm run 6.10.3 app.js                 Run app.js using node 6.10.3
  nvm exec 4.8.3 node app.js            Run `node app.js` with the PATH pointing to node 4.8.3
  nvm alias default 8.1.0               Set default node version on a shell
  nvm alias default node                Always default to the latest available node version on a shell

  nvm install node                      Install the latest available version
  nvm use node                          Use the latest version
  nvm install --lts                     Install the latest LTS version
  nvm use --lts                         Use the latest LTS version

  nvm set-colors cgYmW                  Set text colors to cyan, green, bold yellow, magenta, and white
```

国外的node有些慢，可以替换成国内的地址，修改国内源，执行：

```bash
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node
```

这样设置之后，`nodejs`可以从国内镜像地址下载，但是`npm`不是，需要设置一下才可以

> 参考`nodejs`的设置，很多人可能以为`npm`是这样设置的：`export NVM_NPM_REGISTRY=https://registry.npm.taobao.org`，亲测无效

正确的设置方法是直接执行：

```bash
npm config set registry https://registry.npmmirror.com
```

这样就好了，全局都是这个地址，不需要每个版本的`nodejs`都单独设置

查看`npm`地址命令如下：

```bash
npm config get registry
```

通过这个命令可以验证