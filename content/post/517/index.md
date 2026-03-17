---
title: "MacOS配置homebrew"
slug: "517"
date: 2023-03-02T23:22:32+08:00
lastmod: 2023-03-02T23:22:32+08:00
categories: ["技术"]
tags: ["Apple", "苹果", "Mac", "homebrew"]
draft: false
---

## 说明

前两天买的Mac mini到货了，正好需要配置环境，折腾了一下，特此记录。

### 官网安装

首先，这里是官网：[Homebrew][1]，用官网的脚本安装，执行

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

如果网络没问题，会直接一路安装成功，但如果提示：

```bash
curl: Failed to connect to raw.githubusercontent.com port 443: Connection refused
```

需要修改`/etc/hosts`，在这个网站[IP address][2]，检测一下`raw.githubusercontent.com`，会得到这个域名的A解析，将对应的IP和域名，添加到hosts文件的最后，然后就可以顺利执行脚本了

### 国内脚本

上面的那个官网的，速度不是很理想，可以手动替换源地址，阿里，清华，中科大，腾讯都有。直接百度即可

```bash
# 科大
https://mirrors.ustc.edu.cn/help/brew.git.html

# 阿里
https://developer.aliyun.com/mirror/homebrew/

# 腾讯
https://mirrors.cloud.tencent.com/homebrew/

# 清华
https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/
```

脚本地址：[HomebrewCN][3]

这个脚本，很简单，可以自主选择要使用的国内源，很方便，安装完之后，根据提示运行命令即可

安装脚本命令

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

### 问题

下面记录遇到的几种问题以及解决办法

1，执行`brew update`遇到如下报错：

```bash
Error: Failed to download https://formulae.brew.sh/api/formula.jws.json!
Failed to download https://formulae.brew.sh/api/cask.jws.json!
==> Downloading https://formulae.brew.sh/api/formula.jws.json
                                                                           0.7%curl: (28) Operation too slow. Less than 100 bytes/sec transferred the last 5 seconds

Warning: formula.jws.json: update failed, falling back to cached version.
==> Downloading https://formulae.brew.sh/api/formula.jws.json
#                                                                          2.2%curl: (28) Operation too slow. Less than 100 bytes/sec transferred the last 5 seconds

Warning: formula.jws.json: update failed, falling back to cached version.
==> Downloading https://formulae.brew.sh/api/formula.jws.json
curl: (28) Operation too slow. Less than 100 bytes/sec transferred the last 5 seconds

Error: Failure while executing; `/usr/bin/env /opt/homebrew/Library/Homebrew/shims/shared/curl --disable --cookie /dev/null --globoff --user-agent Homebrew/4.0.4-86-g7c15dce\ \(Macintosh\;\ arm64\ Mac\ OS\ X\ 13.2.1\)\ curl/7.86.0 --header Accept-Language:\ en --fail --progress-bar --location --remote-time --output /Users/rumosky/Library/Caches/Homebrew/api/formula.jws.json --compressed --speed-limit 100 --speed-time 5 --progress-bar https://formulae.brew.sh/api/formula.jws.json` exited with 28. Here's the output:
curl: (28) Operation too slow. Less than 100 bytes/sec transferred the last 5 seconds
```

执行这个命令即可

```bash
export HOMEBREW_NO_INSTALL_FROM_API=1
```

2，执行`brew doctor`时，遇到下面的提示：

```
Please note that these warnings are just used to help the Homebrew maintainers
with debugging if you file an issue. If everything you use Homebrew for is
working fine: please don't worry or file an issue; just ignore this. Thanks!

Warning: Suspicious https://github.com/Homebrew/brew git origin remote found.
The current git origin is:
  https://mirrors.aliyun.com/homebrew/brew.git

With a non-standard origin, Homebrew won't update properly.
You can solve this by setting the origin remote:
  git -C "/opt/homebrew" remote set-url origin https://github.com/Homebrew/brew

Warning: Suspicious https://github.com/Homebrew/homebrew-core git origin remote found.
The current git origin is:
  https://mirrors.aliyun.com/homebrew/homebrew-core.git

With a non-standard origin, Homebrew won't update properly.
You can solve this by setting the origin remote:
  git -C "/opt/homebrew/Library/Taps/homebrew/homebrew-core" remote set-url origin https://github.com/Homebrew/homebrew-core
```

这个忽略就好，不用管。

3，执行`brew update`，遇到下面的提示：

```bash
Warning: No remote 'origin' in /opt/homebrew/Library/Taps/homebrew/homebrew-cask, skipping update!
Warning: No remote 'origin' in /opt/homebrew/Library/Taps/homebrew/homebrew-core, skipping update!
Warning: No remote 'origin' in /opt/homebrew/Library/Taps/homebrew/homebrew-services, skipping update!
Already up-to-date.
```

再执行`brew -v`命令看看是不是有两个warning，这说明你的homebrew-core、homebrew-cask和homebrew-services目录被git认为不是一个安全的目录，需要手动添加

```bash
git config --global --add safe.directory 你的homebrew-core路径
git config --global --add safe.directory 你的homebrew-cask路径
git config --global --add safe.directory 你的homebrew-services路径
```

## 最后

阿里的文档很旧，内容没有更新，腾讯直接就没有文档，建议用清华和科大的，文档很详细，我最后用的是清华的，按照文档一步一步来，一点问题都没有，最后附上我的配置文件`.zprofile`

```bash
eval "$(/opt/homebrew/bin/brew shellenv)"
# Set PATH, MANPATH, etc., for Homebrew.
export HOMEBREW_INSTALL_FROM_API=1
export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
eval "$(/opt/homebrew/bin/brew shellenv)"
```

优先使用官方的脚本，觉得慢，就替换成国内的，实在不行，再用国内的脚本。

  [1]: https://brew.sh
  [2]: https://www.ipaddress.com
  [3]: https://gitee.com/cunkai/HomebrewCN