---
title: "python比较两个表相同内容并输出"
slug: "663"
date: 2023-05-18T15:56:00+08:00
lastmod: 2023-05-18T15:56:00+08:00
categories: ["技术"]
tags: []
draft: false
---

## 前言

根据之前的一个小片段，修改了一下需求，记录一下

### 正文

这个代码用来比较两个sheet相同的内容，保存到新的excel中

源代码：

```py
import openpyxl as xl

# 打开原始文件
wb = xl.load_workbook("source.xlsx")
sheet1 = wb["Sheet1"]
sheet2 = wb["Sheet2"]

# 新建目标文件
wb_new = xl.Workbook()
sheet_new = wb_new.active

# 写入标题行
header = []
for cell in sheet2[1]:
    header.append(cell.value)
sheet_new.append(header)

# 遍历sheet1的所有行并比较与sheet2的字段是否相同
for row1 in sheet1.iter_rows(min_row=2, values_only=True):
    for row2 in sheet2.iter_rows(min_row=2, values_only=True):
        if row1[0] == row2[0]:  # 比较字段是否相同
            values = []
            for cell in row2:
                values.append(cell)
            sheet_new.append(values)

# 保存新文件
wb_new.save("result.xlsx")

```