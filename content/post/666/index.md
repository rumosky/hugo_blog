---
title: "Python虚拟环境pipenv使用说明"
slug: "666"
date: 2023-06-18T21:25:00+08:00
lastmod: 2023-06-18T21:25:00+08:00
categories: ["技术"]
tags: ["python", "pipenv"]
draft: false
---

## 说明

最近需要写好几个Python项目，每个项目的环境都不一样，需要用虚拟环境，记录一下。

### 正文

以前用的都是virtualenv搭建虚拟环境，管理python版本，用 pip进行包的管理。
在虚拟环境激活状态下，可以安装所需的依赖包，安装的依赖包会保存至项目虚拟环境目录下，不会污染系统全局环境

virtualenv的使用存在一定的问题：

由于跨平台的使用不太一致，有时候处理包之间的依赖总存在问题。可能需要手动安装/删除某些特定版本的包存在多个环境（开发环境、测试环境、生产环境）时，依赖包目录文件`requirements.txt`会需要存在多个，无比繁琐要定期更新`requirements.txt`，保持项目环境的一致性，pipenv能够有效管理Python多个环境，各种包，相当于virtualenv和pip的合体，且更加强大。

pipenv主要有以下特性：

* pipenv集成了pip，virtualenv两者的功能，且完善了两者的一些缺陷。

* pipenv会在项目目录下创建`Pipfile`、`Pipfile.lock`文件，管理包之间的依赖关系。

* virtualenv需要将虚拟环境依赖包的导出为`requirements.txt`，一旦依赖包变动，就要重新导出，现在`Pipfile`和`Pipfile.lock`文件可以节省这些步骤，更方便地管理，查看依赖关系是十分方便。

* 各个地方使用了哈希校验，无论安装还是卸载包都十分安全，且会自动公开安全漏洞。

* 通过加载`.env`文件简化开发工作流程。

* 便于docker容器化管理，Pipfile文件支持生成requirements文件，便于项目代码docker化管理，另外，pipfile还支持v–dev环境，可以在调试阶段安装许多调试工具等，而不影响生产环境的环境。

* 无需激活虚拟环境执行代码，只要有pipfile文件即可使用虚拟环境的依赖包执行python脚本，如：通过执行命令`pipenv run python xx.py`

* 支持Python2 和 Python3，在各个平台的命令都是一样的。

#### 安装

在执行安装之前，先说一下，默认是在C盘新建虚拟环境，相应的包存在对应目录，可以在系统内设置环境变量`PIPENV_VENV_IN_PROJECT`为`1`，就可以在当前项目下，生成`.venv`目录，用来存放虚拟环境文件。然后，执行，下面的命令，用来新建一个虚拟环境

```bash
pip install pipenv
```

#### 使用

写在最前面，`pipenv --help`官方说明

```bash
Usage: pipenv [OPTIONS] COMMAND [ARGS]...

Options:
  --where                         Output project home information.
  --venv                          Output virtualenv information.
  --py                            Output Python interpreter information.
  --envs                          Output Environment Variable options.
  --rm                            Remove the virtualenv.
  --bare                          Minimal output.
  --man                           Display manpage.
  --support                       Output diagnostic information for use in
                                  GitHub issues.
  --site-packages / --no-site-packages
                                  Enable site-packages for the virtualenv.
                                  [env var: PIPENV_SITE_PACKAGES]
  --python TEXT                   Specify which version of Python virtualenv
                                  should use.
  --clear                         Clears caches (pipenv, pip).  [env var:
                                  PIPENV_CLEAR]
  -q, --quiet                     Quiet mode.
  -v, --verbose                   Verbose mode.
  --pypi-mirror TEXT              Specify a PyPI mirror.
  --version                       Show the version and exit.
  -h, --help                      Show this message and exit.


Usage Examples:
   Create a new project using Python 3.7, specifically:
   $ pipenv --python 3.7

   Remove project virtualenv (inferred from current directory):
   $ pipenv --rm

   Install all dependencies for a project (including dev):
   $ pipenv install --dev

   Create a lockfile containing pre-releases:
   $ pipenv lock --pre

   Show a graph of your installed dependencies:
   $ pipenv graph

   Check your installed dependencies for security vulnerabilities:
   $ pipenv check

   Install a local setup.py into your virtual environment/Pipfile:
   $ pipenv install -e .

   Use a lower-level pip command:
   $ pipenv run pip freeze

Commands:
  check         Checks for PyUp Safety security vulnerabilities and against
                PEP 508 markers provided in Pipfile.
  clean         Uninstalls all packages not specified in Pipfile.lock.
  graph         Displays currently-installed dependency graph information.
  install       Installs provided packages and adds them to Pipfile, or (if no
                packages are given), installs all packages from Pipfile.
  lock          Generates Pipfile.lock.
  open          View a given module in your editor.
  requirements  Generate a requirements.txt from Pipfile.lock.
  run           Spawns a command installed into the virtualenv.
  scripts       Lists scripts in current environment config.
  shell         Spawns a shell within the virtualenv.
  sync          Installs all packages specified in Pipfile.lock.
  uninstall     Uninstalls a provided package and removes it from Pipfile.
  update        Runs lock, then sync.
  upgrade       Resolves provided packages and adds them to Pipfile, or (if no
                packages are given), merges results to Pipfile.lock
  verify        Verify the hash in Pipfile.lock is up-to-date.
```

使用命令pipenv install，可在当前目录下创建`Pipfile`，`Pipfile.lock`文件，在虚拟环境目录下新增一个虚拟环境

Pipfile文件： 用于保存项目的python版本、依赖包等相关信息 。

```bash
[[source]]
url = "https://pypi.tuna.tsinghua.edu.cn/simple"
verify_ssl = true
name = "pypi"

[packages]
requests = "*"
pyyaml = "*"
Django = "*"

[dev-packages]
pytest = "*"

[requires]
python_version = "3.7"

[scripts]
django = "python manage.py runserver 0.0.0.0:8080"
```

> `source`用来设置仓库地址，即指定镜像源下载虚拟环境所需要的包
> `packages`用来指定项目依赖的包，可以用于生产环境和生成requirements文件
> `dev-packages`用来指定开发环境需要的包，这类包只用于开发过程，不用与生产环境。
> `requires`指定目标Python版本
> `scripts`添加自定义的脚本命令，并通过`pipenv run + 名称`的方式在虚拟环境中执行对应的命令
> `pipenv run django`相当于 执行命令`pipenv run python manage.py runserver 0.0.0.0:8080`

Pipfile.lock文件： 通过hash算法将包的名称和版本，及依赖关系生成哈希值，保证包的完整性，除修改镜像源，非必要情况不对该文件进行修改。

#### 指定python版本创建虚拟环境

使用命令`pipenv install --python 版本号`，可指定python版本创建虚拟环境，pipenv会自动去寻找python安装的目录，然后根据python.exe文件去创建虚拟环境

Windows系统下，基本安装的都是 python3.x版本；而有些Linux系统自带 python2.x 和 python3.x版本，Linux下需要指定默认的python版本，需使用下方的命令：

```bash
pipenv install --two           创建指定python2.x版本的虚拟环境
pipenv install --three         创建指定python3.x版本的虚拟环境
```

#### 安装第三方库

pipenv兼容pip命令，同样使用`pipenv install + 包名`的方式安装第三方库

#### 修改镜像源

如果官方源站安装第三方库的速度很慢，安装失败，可以修改镜像源

pipenv兼容pip命令，所以也可以在命令加上参数

```bash
pipenv install requests -i https://pypi.tuna.tsinghua.edu.cn/simple
```

若想要永久该虚拟环境的镜像源，则需要打开项目目录下的`Pipfile`、`Pipfile.lock`文件，将`source`栏`url = "https://pypi.org/simple"`链接内容修改为需要的镜像源，例如修改为清华的镜像源`url = "https://pypi.tuna.tsinghua.edu.cn/simple"`

#### 安装到dev环境

安装调试工具、性能测试工具、python语法工具，这些内容仅在本地环境有用，生产环境不需要这些。

比如单元测试相关的包`unittest`、`pytest`，只在开发阶段有用，为了和生产环境的包区分开来，可以通过命令`pipenv install --dev + 包名`将其归类到`dev-packages`下

例如安装`pytest`到开发环境

```bash
pipenv install --dev pytest
```

#### pipenv命令

pipenv兼容大部分的pip命令，所以 pip命令能实现的内容，也能通过pipenv命令实现

#### 卸载命令

```bash
pipenv uninstall requests
# 在项目所在虚拟环境中卸载requests包，并在Pipfile文件移除包名

pipenv uninstall --all
# 在项目所在虚拟环境中卸载所有包，并在Pipfile文件移除包名

pipenv uninstall --all --dev
# 在项目所在虚拟环境中卸载所有dev环境的包，并在Pipfile文件移除dev-packages中的所有包名
```

#### 更新命令

```bash
pipenv update requests
# 在项目所在虚拟环境中更新requests包，并在Pipfile.lock文件中更新相应版本信息

pipenv update
# 在项目所在虚拟环境中更新所有包，并在Pipfile.lock文件中更新相应版本信息
pipenv update --outdated
# 在项目所在虚拟环境中查看已过期的包的信息

pipenv lock
# 根据项目所在虚拟环境的Pipfile文件生成/更新Pipfile.lock文件中的依赖包信息
```

#### 查看命令

```bash
pipenv --where
# 查看项目位置

pipenv --venv
# 查看虚拟环境位置

pipenv --py
# 查看虚拟环境python解释器位置

pipenv graph
# 查看依赖包信息
```

#### 激活与退出虚拟环境

使用`pipenv install`命令创建虚拟环境时，创建成功会默认激活虚拟环境

若想退出虚拟环境，可输入`exit 退出（仅适用于黑屏终端，pycharm默认打开项目就加载了虚拟环境，只能修改指定的虚拟环境）

目录下存在`Pipfile`、`Pipfile.lock`文件，已创建过虚拟环境，可通过命令`pipenv shell`进行激活

#### 删除虚拟环境

```bash
pipenv --rm
```

**注意**：该命令无法在pycharm的Terminal终端执行成功，使用pycharm打开项目时，默认激活了项目下的虚拟环境，而使用exit命令退出虚拟环境，pycharm会关闭当前的Terminal终端

删除虚拟环境后，如果目录下仍存在`Pipfile`、`Pipfile.lock`文件，可以通过`pipenv install`重新进行安装虚拟环境，且重新安装的虚拟环境，名称与删除前一致

#### 运行python代码

在虚拟环境已激活的情况下，可直接执行python代码，例如执行demo文件中的`test.py`文件

```bash
python test.py
```

控制台会返回post请求的结果

再比如运行Django项目

```bash
python manage.py runserver 0.0.0.0:8080
```

在虚拟环境未激活的情况下，也可以通过pipenv命令使用虚拟环境运行代码，使用`pipenv run python xxx.py`命令，执行python文件

```bash
pipenv run python test.py
```

黑屏终端返回post请求的结果

再比如运行Django项目（ scripts中已添加自定义的脚本命令）

```bash
pipenv run django
```

#### requirements.txt 文件

`pipenv`可以像`virtualenv`一样使用命令生成`requirements.txt`文件

```bash
pipenv requirements > requirements.txt
```

`pipenv`还可以通过`requirements.txt`文件安装依赖包

```bash
pipenv install -r requirement.txt
```