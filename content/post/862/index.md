---
title: "poetry install报错"
slug: "862"
date: 2025-12-06T00:02:00+08:00
lastmod: 2025-12-06T00:03:57+08:00
categories: ["技术"]
tags: ["python", "Windows", "poetry"]
draft: false
---

如题，在执行`poetry install`时，有下面的提示

```bash
Error: The current project could not be installed: No file/folder found for package loveapi
If you do not want to install the current project use --no-root.
If you want to use Poetry only for dependency management but not for packaging, you can disable package mode by setting package-mode = false in your pyproject.toml file.
If you did intend to install the current project, you may need to set `packages` in your pyproject.toml file.
```

## 分析

这个错误提示说明 Poetry 在安装当前项目时，找不到对应的包文件或文件夹。这个问题通常是因为 `pyproject.toml` 中的 `packages` 配置不正确，或者项目目录结构不符合 Poetry 的默认期望。

针对这个问题，可以按以下步骤排查和解决：

1. **确认项目目录结构**  
   Poetry 默认会根据项目根目录下和 `pyproject.toml` 中的配置，去寻找包目录。比如，如果你的包名是 `api`，项目目录结构一般是：

   ```
   项目根目录/
   ├── api/
   │   ├── __init__.py
   │   └── ...
   ├── pyproject.toml
   └── ...
   ```

   确保 `api` 文件夹存在，且里面有 `__init__.py` 文件（即使是空文件），否则 Poetry 不会识别它为包。

2. **检查 `pyproject.toml` 中的 `[tool.poetry]` 部分**  
   重点查看是否有 `packages` 配置，比如：

   ```toml
   [tool.poetry]
   name = "api"
   version = "0.1.0"
   description = ""
   authors = ["你的名字 <youremail@example.com>"]

   packages = [
       { include = "api" }
   ]
   ```

   如果没有 `packages` 配置，且项目目录结构不标准，可以添加上述配置，告诉 Poetry 你需要打包的包名。

3. **或者关闭包模式**  
   如果你只是想用 Poetry 管理依赖，不需要安装当前项目包（比如你项目就是一个脚本集合，没有包结构），可以在 `pyproject.toml` 里添加：

   ```toml
   [tool.poetry]
   package-mode = false
   ```

   这样 Poetry 不会尝试把当前项目当成一个包去安装。

4. **使用 `--no-root` 参数**  
   如果你只想安装依赖，但不安装当前项目，可以执行：

   ```bash
   poetry install --no-root
   ```

5. **总结**  
   - 如果你的项目是一个包，确认包目录和 `packages` 配置正确。  
   - 如果不是包项目，考虑关闭包模式或用 `--no-root`。  
   - 保证包目录里有 `__init__.py` 文件。