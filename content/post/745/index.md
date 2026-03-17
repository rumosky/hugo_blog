---
title: "intel Arc A770跑stable-diffusion绘图AI"
slug: "745"
date: 2024-02-25T00:17:00+08:00
lastmod: 2024-02-25T00:26:28+08:00
categories: ["技术"]
tags: ["显卡", "SD", "Intel", "Arc", "a770"]
draft: false
---

## 说明

本文记录用Intel Arc A770 16G独显运行stable-diffusion-next。

源码地址：https://github.com/vladmandic/automatic

> 这个版本是专门的一个支持oneAPI和DirectML的版本，原版的SD是无法正常跑Intel独显的，运行原版代码时即便Script选择openVINO，设备选择GPU，也无法调用独显，一直使用的是CPU，跑起来非常的慢。

### 步骤

官方安装说明：https://github.com/vladmandic/automatic/wiki/Installation

先下载源码，下载之后分两种情况：

使用oneAPI时运行下面命令：

```bash
.\webui.bat --use-ipex
```

如果使用DirectML/AMD显卡的时候，运行下面命令：（不建议使用这个，这个是微软做的一个DX12的套壳，效率低一些，支持DX12的显卡都可以用，容易爆显存）

```bash
.\webui.bat --use-directml
```

第一次运行时，显示如下内容，有很多报错，最后直接运行不了

```bash
Using python: "D:\Tools\Python310\python.exe"
Creating VENV: D:\projects\oneAPI\venv
Using VENV: D:\projects\oneAPI\venv
21:23:17-328839 INFO     Starting SD.Next
21:23:17-331343 INFO     Logger: file="D:\projects\oneAPI\sdnext.log" level=INFO size=65 mode=append
21:23:17-331591 INFO     Python 3.10.11 on Windows
21:23:17-337596 WARNING  Not a git repository, all git operations are disabled
21:23:17-436994 INFO     Version: app=sd.next version=unknown
21:23:17-445213 INFO     Platform: arch=AMD64 cpu=Intel64 Family 6 Model 191 Stepping 2, GenuineIntel system=Windows
                         release=Windows-10-10.0.22631-SP0 python=3.10.11
21:23:17-447214 INFO     Intel OneAPI Toolkit detected
21:23:17-448272 INFO     Installing package: openvino==2023.3.0
21:23:56-097415 INFO     Installing package: nncf==2.7.0
21:25:06-724489 INFO     Installing package: onnxruntime-openvino
21:25:51-434494 INFO     Installing package:
                         https://github.com/Nuullll/intel-extension-for-pytorch/releases/download/v2.1.10%2Bxpu/torch-2.
                         1.0a0+cxx11.abi-cp310-cp310-win_amd64.whl
                         https://github.com/Nuullll/intel-extension-for-pytorch/releases/download/v2.1.10%2Bxpu/torchvis
                         ion-0.16.0a0+cxx11.abi-cp310-cp310-win_amd64.whl
                         https://github.com/Nuullll/intel-extension-for-pytorch/releases/download/v2.1.10%2Bxpu/intel_ex
                         tension_for_pytorch-2.1.10+xpu-cp310-cp310-win_amd64.whl
21:48:13-444031 ERROR    Error running pip: install --upgrade
                         https://github.com/Nuullll/intel-extension-for-pytorch/releases/download/v2.1.10%2Bxpu/torch-2.
                         1.0a0+cxx11.abi-cp310-cp310-win_amd64.whl
                         https://github.com/Nuullll/intel-extension-for-pytorch/releases/download/v2.1.10%2Bxpu/torchvis
                         ion-0.16.0a0+cxx11.abi-cp310-cp310-win_amd64.whl
                         https://github.com/Nuullll/intel-extension-for-pytorch/releases/download/v2.1.10%2Bxpu/intel_ex
                         tension_for_pytorch-2.1.10+xpu-cp310-cp310-win_amd64.whl
21:48:13-475903 INFO     Installing package: onnxruntime
21:49:16-700022 INFO     Startup: quick launch
21:49:16-702021 INFO     Verifying requirements
21:49:16-704688 INFO     Installing package: patch-ng
21:50:18-756981 INFO     Installing package: anyio
21:51:20-712473 INFO     Installing package: addict
21:52:21-941029 INFO     Installing package: astunparse
21:53:53-561538 INFO     Installing package: blendmodes
21:55:25-709061 INFO     Installing package: clean-fid
21:57:41-560906 INFO     Installing package: filetype
21:59:13-110764 INFO     Installing package: GitPython
22:00:45-352688 INFO     Installing package: httpcore
22:02:17-167470 INFO     Installing package: inflection
22:03:18-574170 INFO     Installing package: jsonmerge
22:04:20-113806 INFO     Installing package: kornia
22:05:53-026983 INFO     Installing package: lark
22:07:24-776749 INFO     Installing package: lpips
22:08:56-262614 INFO     Installing package: omegaconf
22:09:59-887428 INFO     Installing package: open-clip-torch
22:11:05-909365 INFO     Installing package: optimum
22:12:29-071462 INFO     Installing package: piexif
22:13:31-398533 INFO     Installing package: resize-right
22:14:33-779426 INFO     Installing package: tensordict==0.1.2
22:15:36-442087 INFO     Installing package: toml
22:16:38-630454 INFO     Installing package: torchdiffeq
22:17:41-080749 INFO     Installing package: voluptuous
22:18:43-332142 INFO     Installing package: yapf
22:19:46-587471 INFO     Installing package: scikit-image
22:20:53-801146 INFO     Installing package: fasteners
22:21:56-445067 INFO     Installing package: dctorch
22:22:58-843256 INFO     Installing package: pymatting
22:24:09-835886 INFO     Installing package: peft
22:25:13-388347 INFO     Installing package: orjson
22:26:16-242105 INFO     Installing package: httpx==0.24.1
22:27:19-299837 INFO     Installing package: compel==2.0.2
22:28:25-715719 INFO     Installing package: torchsde==0.2.6
22:29:28-366735 INFO     Installing package: clip-interrogator==0.6.0
22:30:31-323249 WARNING  Package version mismatch: tqdm 4.66.2 required 4.66.1
22:30:31-324803 INFO     Installing package: tqdm==4.66.1
22:31:34-238382 INFO     Installing package: opencv-contrib-python-headless==4.9.0.80
22:32:42-022352 INFO     Installing package: einops==0.4.1
22:33:44-676732 INFO     Installing package: gradio==3.43.2
22:34:57-186024 INFO     Installing package: numexpr==2.8.8
22:36:00-191676 WARNING  Package version mismatch: protobuf 4.25.3 required 3.20.3
22:36:00-192594 INFO     Installing package: protobuf==3.20.3
22:37:03-611931 INFO     Installing package: pytorch_lightning==1.9.4
22:38:09-389775 WARNING  Package version mismatch: transformers 4.38.1 required 4.37.2
22:38:09-391845 INFO     Installing package: transformers==4.37.2
22:39:56-229606 INFO     Installing package: tomesd==0.1.3
22:41:29-162150 WARNING  Package version mismatch: urllib3 2.2.1 required 1.26.18
22:41:29-163222 INFO     Installing package: urllib3==1.26.18
22:43:02-394965 WARNING  Package version mismatch: timm 0.9.16 required 0.9.12
22:43:02-395965 INFO     Installing package: timm==0.9.12
22:44:37-451715 WARNING  Package version mismatch: pydantic 2.6.2 required 1.10.13
22:44:37-452720 INFO     Installing package: pydantic==1.10.13
22:46:11-286492 INFO     Verifying packages
22:46:11-288608 INFO     Installing package: git+https://github.com/openai/CLIP.git
22:47:27-820466 INFO     Installing package:
                         git+https://github.com/patrickvonplaten/invisible-watermark.git@remove_onnxruntime_depedency
22:51:39-356793 INFO     Installing package: pi-heif
22:52:42-965922 INFO     Installing package: tensorflow==2.13.0
22:55:21-898312 ERROR    Required path not found: path=D:\projects\oneAPI\modules\k-diffusion\k_diffusion\sampling.py
                         item=k_diffusion
22:55:21-899820 INFO     Extensions: disabled=[]
22:55:21-902933 INFO     Extensions: enabled=['Lora', 'sd-extension-chainner', 'sd-extension-system-info',
                         'sd-webui-agent-scheduler', 'sd-webui-controlnet', 'stable-diffusion-webui-images-browser',
                         'stable-diffusion-webui-rembg'] extensions-builtin
22:55:21-911068 INFO     Extensions: enabled=[] extensions
22:55:21-930814 INFO     Command line args: ['--use-ipex'] use_ipex=True
╭───────────────────────────────────────── Traceback (most recent call last) ──────────────────────────────────────────╮
│ D:\projects\oneAPI\launch.py:263 in <module>                                                                         │
│                                                                                                                      │
│   262 if __name__ == "__main__":                                                                                     │
│ ❱ 263     main()                                                                                                     │
│   264                                                                                                                │
│                                                                                                                      │
│ D:\projects\oneAPI\launch.py:240 in main                                                                             │
│                                                                                                                      │
│   239                                                                                                                │
│ ❱ 240     uv, instance = start_server(immediate=True, server=None)                                                   │
│   241     while True:                                                                                                │
│                                                                                                                      │
│ D:\projects\oneAPI\launch.py:168 in start_server                                                                     │
│                                                                                                                      │
│   167     get_custom_args()                                                                                          │
│ ❱ 168     module_spec.loader.exec_module(server)                                                                     │
│   169     uvicorn = None                                                                                             │
│ in exec_module:883                                                                                                   │
│ in _call_with_frames_removed:241                                                                                     │
│                                                                                                                      │
│ D:\projects\oneAPI\webui.py:11 in <module>                                                                           │
│                                                                                                                      │
│    10 from threading import Thread                                                                                   │
│ ❱  11 import modules.loader                                                                                          │
│    12 import torch # pylint: disable=wrong-import-order                                                              │
│                                                                                                                      │
│ D:\projects\oneAPI\modules\loader.py:48 in <module>                                                                  │
│                                                                                                                      │
│   47                                                                                                                 │
│ ❱ 48 from fastapi import FastAPI # pylint: disable=W0611,C0411                                                       │
│   49 import gradio # pylint: disable=W0611,C0411                                                                     │
│                                                                                                                      │
│ D:\projects\oneAPI\venv\lib\site-packages\fastapi\__init__.py:7 in <module>                                          │
│                                                                                                                      │
│    6                                                                                                                 │
│ ❱  7 from .applications import FastAPI as FastAPI                                                                    │
│    8 from .background import BackgroundTasks as BackgroundTasks                                                      │
│                                                                                                                      │
│ D:\projects\oneAPI\venv\lib\site-packages\fastapi\applications.py:16 in <module>                                     │
│                                                                                                                      │
│     15                                                                                                               │
│ ❱   16 from fastapi import routing                                                                                   │
│     17 from fastapi.datastructures import Default, DefaultPlaceholder                                                │
│                                                                                                                      │
│ D:\projects\oneAPI\venv\lib\site-packages\fastapi\routing.py:22 in <module>                                          │
│                                                                                                                      │
│     21                                                                                                               │
│ ❱   22 from fastapi import params                                                                                    │
│     23 from fastapi._compat import (                                                                                 │
│                                                                                                                      │
│ D:\projects\oneAPI\venv\lib\site-packages\fastapi\params.py:5 in <module>                                            │
│                                                                                                                      │
│     4                                                                                                                │
│ ❱   5 from fastapi.openapi.models import Example                                                                     │
│     6 from pydantic.fields import FieldInfo                                                                          │
│                                                                                                                      │
│ D:\projects\oneAPI\venv\lib\site-packages\fastapi\openapi\models.py:4 in <module>                                    │
│                                                                                                                      │
│     3                                                                                                                │
│ ❱   4 from fastapi._compat import (                                                                                  │
│     5     PYDANTIC_V2,                                                                                               │
│                                                                                                                      │
│ D:\projects\oneAPI\venv\lib\site-packages\fastapi\_compat.py:20 in <module>                                          │
│                                                                                                                      │
│    19                                                                                                                │
│ ❱  20 from fastapi.exceptions import RequestErrorModel                                                               │
│    21 from fastapi.types import IncEx, ModelNameMap, UnionType                                                       │
│                                                                                                                      │
│ D:\projects\oneAPI\venv\lib\site-packages\fastapi\exceptions.py:6 in <module>                                        │
│                                                                                                                      │
│     5 from starlette.exceptions import WebSocketException as StarletteWebSocketException                             │
│ ❱   6 from typing_extensions import Annotated, Doc  # type: ignore [attr-defined]                                    │
│     7                                                                                                                │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
ImportError: cannot import name 'Doc' from 'typing_extensions' (D:\projects\oneAPI\venv\lib\site-packages\typing_extensions.py)
请按任意键继续. . .
```

上面的这个错误，没有找到问题，重启一下，然后又报错：

```py
No module named 'intel_extension_for_pytorch'
```

这个时候，在Intel官网下载：https://intel.github.io/intel-extension-for-pytorch/index.html#installation

然后在页面上选择对应的系统版本就可以看到安装命令，然后执行即可，安装好之后，再次重启SD

结果如下图：
![SDnext-run](https://cdn.rumosky.com/usr/uploads/2024/02/3878216114.png)

点击`URL`即可访问web界面，界面如下：

![SDnext](https://cdn.rumosky.com/usr/uploads/2024/02/2954395830.png)

现在就可以填入提示词等信息跑图了