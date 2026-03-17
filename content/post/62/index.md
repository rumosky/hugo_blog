---
title: "VScode配置C/C++环境"
slug: "62"
date: 2020-03-03T22:11:00+08:00
lastmod: 2020-03-03T22:11:00+08:00
categories: ["技术"]
tags: ["Windows", "vscode", "C"]
draft: false
---

## 说明

Dev C++停止更新很久，codeblocks开源但也许久没有发布新版，加上这两个都没有代码提示，语法高亮也很丑，所以便使用VScode开发，特此记录。

### 步骤

1.安装MinGW

```bash
# 官网在线安装
https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/installer/mingw-w64-install.exe/download

# 官网离线安装
https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/
```

设置如下图：

![MinGW下载设置](https://cdn.rumosky.com/usr/uploads/2020/03/86230954.png)

> 建议安装在C盘根目录，不要安装在文件夹名有空格的目录下，本文安装目录为`C:\MinGW\`

**2021.1.6 更新**

上述下载网站版本过旧，且国内网速过慢，建议使用下述地址，版本新网速快

下载地址：[mingw](https://nuwen.net/mingw.html "mingw")

2.下载安装好后请将此目录添加为系统环境变量`C:\MinGW\mingw64\bin`

3.打开VScode，安装扩展`Code Runner`和`C/C++`

4.新建一个文件夹，此文件夹为以后C/C++程序文件夹

> 新建的这个文件夹即工作目录，配置好以后，可以在此目录下继续新建文件夹，不需要再次配置

5.在新建文件夹内新建`.vscode`，内含`launch.json`，`tasks.json`，`c_cpp_properties.json`三个文件，文件内容如下：

#### launch.json

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch", // 配置名称，将会在启动配置的下拉菜单中显示
            "type": "cppdbg", // 配置类型，这里只能为cppdbg
            "request": "launch", // 请求配置类型，可以为launch（启动）或attach（附加）
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
            "args": [], // 程序调试时传递给程序的命令行参数，一般设为空即可
            "stopAtEntry": false, // 设为true时程序将暂停在程序入口处，一般设置为false
            "cwd": "${workspaceFolder}", // 调试程序时的工作目录，一般为${workspaceFolder}
            "environment": [],
            "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台
            "MIMode": "gdb",
            "miDebuggerPath": "C:/MinGW/mingw64/bin/gdb.exe", // miDebugger的路径，注意这里要与MinGw的路径对应
            "preLaunchTask": "gcc", // 调试会话开始前执行的任务，一般为编译程序，c++为g++, c为gcc
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": false
                }
            ]
        }
    ]
}
```

#### tasks.json（C语言）

```json
{
    "version": "2.0.0",
    "command": "gcc",
    "args": [
        "-g",
        "${file}",
        "-o",
        "${fileBasenameNoExtension}.exe"
    ]
}
```

#### tasks.json（C++）

```json
{
    "version": "2.0.0",
    "command": "g++",
    "args": [
        "-g",
        "${file}",
        "-o",
        "${fileBasenameNoExtension}.exe"
    ], // 编译命令参数
    "problemMatcher": {
        "owner": "cpp",
        "fileLocation": [
            "relative",
            "${workspaceFolder}"
        ],
        "pattern": {
            "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
            "file": 1,
            "line": 2,
            "column": 3,
            "severity": 4,
            "message": 5
        }
    }
}
```

#### c_cpp_properties.json

```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "C:/MinGW/mingw64/include/**",
                "C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++",
                "C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/x86_64-w64-mingw32",
                "C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/backward",
                "C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include",
                "C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include-fixed",
                "C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/../../../../x86_64-w64-mingw32/include"
            ],
            "browse": {
                "limitSymbolsToIncludedHeaders": true,
                "databaseFilename": ""
            }
        }
    ],
    "version": 4
}
```

##### includePath获取方法

1.打开`cmd`，输入`gcc -v -E -x c++ -`，结果如下：

```bash
Using built-in specs.
COLLECT_GCC=C:\MinGW\mingw64\bin\gcc.exe
Target: x86_64-w64-mingw32
Configured with: ../../../src/gcc-8.1.0/configure --host=x86_64-w64-mingw32 --build=x86_64-w64-mingw32 --target=x86_64-w64-mingw32 --prefix=/mingw64 --with-sysroot=/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64 --enable-shared --enable-static --disable-multilib --enable-languages=c,c++,fortran,lto --enable-libstdcxx-time=yes --enable-threads=win32 --enable-libgomp --enable-libatomic --enable-lto --enable-graphite --enable-checking=release --enable-fully-dynamic-string --enable-version-specific-runtime-libs --disable-libstdcxx-pch --disable-libstdcxx-debug --enable-bootstrap --disable-rpath --disable-win32-registry --disable-nls --disable-werror --disable-symvers --with-gnu-as --with-gnu-ld --with-arch=nocona --with-tune=core2 --with-libiconv --with-system-zlib --with-gmp=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-mpfr=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-mpc=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-isl=/c/mingw810/prerequisites/x86_64-w64-mingw32-static --with-pkgversion='x86_64-win32-seh-rev0, Built by MinGW-W64 project' --with-bugurl=https://sourceforge.net/projects/mingw-w64 CFLAGS='-O2 -pipe -fno-ident -I/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64/opt/include -I/c/mingw810/prerequisites/x86_64-zlib-static/include -I/c/mingw810/prerequisites/x86_64-w64-mingw32-static/include' CXXFLAGS='-O2 -pipe -fno-ident -I/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64/opt/include -I/c/mingw810/prerequisites/x86_64-zlib-static/include -I/c/mingw810/prerequisites/x86_64-w64-mingw32-static/include' CPPFLAGS=' -I/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64/opt/include -I/c/mingw810/prerequisites/x86_64-zlib-static/include -I/c/mingw810/prerequisites/x86_64-w64-mingw32-static/include' LDFLAGS='-pipe -fno-ident -L/c/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64/opt/lib -L/c/mingw810/prerequisites/x86_64-zlib-static/lib -L/c/mingw810/prerequisites/x86_64-w64-mingw32-static/lib '
Thread model: win32
gcc version 8.1.0 (x86_64-win32-seh-rev0, Built by MinGW-W64 project)
COLLECT_GCC_OPTIONS='-v' '-E' '-mtune=core2' '-march=nocona'
 C:/MinGW/mingw64/bin/../libexec/gcc/x86_64-w64-mingw32/8.1.0/cc1plus.exe -E -quiet -v -iprefix C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/ -U_REENTRANT - -mtune=core2 -march=nocona
ignoring duplicate directory "C:/MinGW/mingw64/lib/gcc/../../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++"
ignoring duplicate directory "C:/MinGW/mingw64/lib/gcc/../../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/x86_64-w64-mingw32"
ignoring duplicate directory "C:/MinGW/mingw64/lib/gcc/../../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/backward"
ignoring duplicate directory "C:/MinGW/mingw64/lib/gcc/../../lib/gcc/x86_64-w64-mingw32/8.1.0/include"
ignoring nonexistent directory "C:/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64C:/msys64/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/../../../../include"
ignoring duplicate directory "C:/MinGW/mingw64/lib/gcc/../../lib/gcc/x86_64-w64-mingw32/8.1.0/include-fixed"
ignoring duplicate directory "C:/MinGW/mingw64/lib/gcc/../../lib/gcc/x86_64-w64-mingw32/8.1.0/../../../../x86_64-w64-mingw32/include"
ignoring nonexistent directory "C:/mingw810/x86_64-810-win32-seh-rt_v6-rev0/mingw64/mingw/include"
#include "..." search starts here:
#include <...> search starts here:
 C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++
 C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/x86_64-w64-mingw32
 C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/backward
 C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include
 C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include-fixed
 C:/MinGW/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/../../../../x86_64-w64-mingw32/include
End of search list.
```

2.将第19-24行得到的路径替换到`c_cpp_properties.json`文件includepath中的第7-12行

> 注意，includepath中第一行为安装目录下include目录，其余为刚刚cmd中获取到的目录，书写注意格式。

### 运行程序

在工作目录下新建`welcome.c`文件，内容如下：

```c
#include<stdio.h>

int main() {
    printf("welcome to 124.221.171.144!");
    return 0;
}
```

点击右上角的  图标运行代码，结果如下：

![运行结果](https://cdn.rumosky.com/usr/uploads/2020/03/4232031270.png)

#### 写在最后

建议配置文件内容均使用C++版本，也就是命令使用`g++`而不是`gcc`，C语言可以在C++环境下运行，相反则不行。