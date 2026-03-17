---
title: "Python处理excel查找相同列内容并输出对应行数据"
slug: "657"
date: 2023-05-18T11:42:00+08:00
lastmod: 2023-05-18T11:42:00+08:00
categories: ["技术"]
tags: ["python"]
draft: false
---

## 前言

前两天帮同学处理了一个excel，可以用Python实现，记录一下。

### 正文

说明：有一个excel工作簿，含有两个表，sheet1和sheet2，这两个表都有基金名称这个字段，sheet1内容不重复，sheet2有重复，以sheet1基金名称为模板，在sheet2中查找，最后导出sheet2中含有sheet1内容的数据。

其实用vlookup，然后再去重就好了，但正好好久没有写python了，实现一下。

> 思路是这样的，先把sheet1和sheet2的基金名称存在列表里，然后，对比两个列表，查找出相同项的序号，这个序号是列表里面的，再把序号转换成真实的excel行号，最后，根据行号从sheet2中获取对应的行数据，写入新的excel文件，然后去除空行即可。

头一次用openpyxl模块，对于函数不是很熟悉，应该是有更简单的方式，不需要放在两个列表里面，这里就不再讨论

源代码：

```py
import openpyxl as xl

# 加载源文件
wb = xl.load_workbook("source.xlsx")

# 选择当前工作表
source_sheet = wb["Sheet1"]

# 定义源数据列
source = []

# 遍历源数据表，将对应的列内容添加至数组
for i in range(1, source_sheet.max_row):
    source.append(source_sheet.cell(row=i + 1, column=4).value)

# 选择目标数据表
target_sheet = wb["Sheet2"]

# 定义目标数据列
target = []

# 遍历目标数据表，将对应的列内容添加至数组
for i in range(1, target_sheet.max_row):
    target.append(target_sheet.cell(row=i + 1, column=3).value)

# 定义匹配结果的行号
result_index = []

# 遍历源数据和目标数据，将相等的结果序号，存入刚才定义的列表
for i in range(len(target)):
    for j in range(len(source)):
        if source[j] == target[i]:
            result_index.append(i)

# 数组序号转换为真实的excel行号
result_index = [(i + 2) for i in result_index]

# 定义过滤后的字典（map）
data = {}

# 定义需要查找的列序号
column = ["A", "B", "C", "D", "E", "F", "G"]

# 根据获取到的行号，遍历取出相应的行数据
for i in result_index:
    for j in column:
        index = j + str(i)  # 拼接出单元格序号
        # 根据单元格序号获取对应的值，并将值存入字典
        data.update({index: target_sheet[index].value})

# 创建新excel表
wb2 = xl.Workbook()
sheet = wb2.active

# 重命名新表为result
sheet.title = "result"

# 将获取到的结果数据写入excel文件
for key in data:
    sheet[key] = data[key]

wb2.save("result.xlsx")

# 加载结果数据表
ws = wb2["result"]

# 过滤excel中的空行
rows_to_delete = []
for row in ws.iter_rows(min_row=1, max_row=ws.max_row):
    if all([cell.value is None for cell in row]):
        rows_to_delete.append(row)

for row in rows_to_delete:
    ws.delete_rows(row[0].row, 1)

# 保存结果文件
wb2.save("result.xlsx")

```

### 最后

附上原始数据表：[source.xlsx](https://cdn.rumosky.com/usr/uploads/2023/05/4129191976.xlsx)

代码仅供参考，可以根据个人情况自行调整。