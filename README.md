# PythonNote
Python备忘录-基于Python3

  \* [PythonNote](#pythonnote)

   \* [1、Python读写EXECL](#1python读写execl)

​     \* [1 安装及导入](#1-安装及导入)

​     \* [2 使用](#2-使用)

​     \* [3 详细使用](#3-详细使用)

​      \* [1 读取excel](#1-读取excel)

​      \* [2 读取sheet](#2-读取sheet)

​      \* [3 读取sheet的行](#3-读取sheet的行)

​      \* [4 读取sheet的列](#4-读取sheet的列)

​      \* [5 读取sheet的单元格](#5-读取sheet的单元格)

​      \* [6 写入excel](#6-写入excel)

​      \* [7 带样式写入示例](#7-带样式写入示例)

   \* [2、Python文件增删改查](#2python文件增删改查)

​     \* [1 文件操作 rwa](#1-文件操作-rwa)

​     \* [2 使用](#2-使用-1)

​     \* [3 文件内光标的移动read、seek、tell、truncate](#3-文件内光标的移动readseektelltruncate)

   \* [3、Python读写JSON文件](#3python读写json文件)

​     \* [1 先读后写](#1-先读后写)

​     \* [2 读写一起 移动文件位置指针](#2-读写一起-移动文件位置指针)

​     \* [3 数组类型读写](#3-数组类型读写)


## 1、Python读写EXECL
### 1 安装及导入
```
安装
pip install xlrd

pip install xlwt

pip install xlutils

导入
import xlrd

import xlwt

import xlutils
```

### 2 使用
```
# !/usr/bin/env python
# -*- coding: utf-8 -*-
# 读取excel数据
import xlrd
import os

data = xlrd.open_workbook('share.xls')  # 打开xls文件
table = data.sheets()[0]  # 打开第一张表
nrows = table.nrows  # 获取表的行数
# 删除原文件 创建新文件
# os.remove("ZYTestLocal/zh-Hans.lproj/loc.strings")
# f = file("ZYTestLocal/zh-Hans.lproj/loc.strings", "a+")
for i in range(nrows):  # 循环逐行打印
    if i < 1:  # 跳过第一,二行
        continue
    # 主语言
    en = str(table.row_values(i)[1].encode('utf8'), encoding = "utf-8")
    # 其他语言
    other = str(table.row_values(i)[2].encode('utf8'), encoding = "utf-8")
    if len(en) > 0:
        output = "\"{0}\" = \"{1}\";\n".format(en, other)
        print(output)
```

### 3 详细使用
####  1 读取excel

```
# ----读取excel----
data= xlrd.open_workbook(excel_file)
```

#### 2 读取sheet

```

# ----读取sheet----
# 通过索引顺序获取sheet 
table = data.sheets()[0]
 
# 通过索引顺序获取sheet 
table = data.sheet_by_index(0))
 
# 通过名称获取sheet 
table = data.sheet_by_name("sheet")
 
# 返回book中所有sheet的名字
names = data.sheet_names()
 
# 传入索引或sheet名检查某个sheet是否导入完毕
table.sheet_loaded("sheet")
table.sheet_loaded(0)
 
# sheet名
table.name
 
# sheet列数
table.ncols
 
# sheet行数
table.nrows



```

#### 3 读取sheet的行
```
# 返回由rowx行中所有的单元格对象组成的列表
table.row(rowx)
 
# 获取rowx行第一个单元格的类型
# 0. empty（空的）,1 string（text）, 2 number, 3 date, 4 boolean, 5 error， 6 blank（空白表格）
table.row(rowx)[0].ctype
 
# 获取rowx行第一个单元格的值
table.row(rowx)[0].value
 
# 返回由rowx行中所有的单元格对象组成的列表
table.row_slice(self, rowx, start_colx=0, end_colx=None)
 
# 返回由rowx行中所有单元格的数据类型组成的列表
table.row_types(rowx, start_colx=0, end_colx=None)
 
# 返回由rowx行中所有单元格的数据组成的列表
table.row_values(rowx, start_colx=0, end_colx=None)
 
# 返回rowx行的有效单元格长度
table.row_len(rowx)
```

#### 4 读取sheet的列
```
#返回colx列中所有的单元格对象组成的列表
table.col(colx, start_rowx=0, end_rowx=None)  
 
#返回colx列中所有的单元格对象组成的列表
table.col_slice(colx, start_rowx=0, end_rowx=None)  
 
#返回colx列中所有单元格的数据类型组成的列表
table.col_types(colx, start_rowx=0, end_rowx=None) 
   
#返回colx列中所有单元格的数据组成的列表
table.col_values(colx, start_rowx=0, end_rowx=None)   
```
#### 5 读取sheet的单元格
```
# 返回单元格对象
cell = table.cell(rowx,colx)
# 单元格数据类型
# 0. empty（空的）,1 string（text）, 2 number, 3 date, 4 boolean, 5 error， 6 blank（空白表格）
cell.ctype
# 单元格值
cell.value
# 返回单元格中的数据类型
table.cell_type(rowx,colx)
# 返回单元格中的数据
table.cell_value(rowx,colx)
# 暂时还没有搞懂
table.cell_xf_index(rowx, colx)
```
#### 6 写入excel
```
# 使用xlutils将xlrd读取的对象转为xlwt可操作对象，table即上述xlrd读取的table
workbook = xlutils.copy(table)
 
# 或者如果你只是想创建一张空表
workbook = xlwt.Workbook(encoding = 'utf-8')
 
# 创建一个sheet
worksheet = workbook.add_sheet('sheet')
# 获取一个已存在的sheet
worksheet = workbook.get_sheet('sheet')
 
# 写入一个值，括号内分别为行数、列数、内容
worksheet.write(row, column, "memeda")
 
workbook.save('memeda.xls')
```
#### 7 带样式写入示例
```
workbook = xlwt.Workbook(encoding = 'utf-8')
style = xlwt.XFStyle()
font = xlwt.Font() # 创建字体
font.name = 'Arial'
font.bold = True # 黑体
font.underline = True # 下划线
font.italic = True # 斜体字
font.colour_index = 2 # 颜色为红色
style.font = font
worksheet.write(row, column, "memeda", style)
workbook.save('memeda.xls')
```

## 2、Python文件增删改查
### 1 文件操作 rwa
r、w、a、r+、w+、a+和b
|   模式   |   可做操作   |   若文件不存在   |   是否覆盖   |
| ---- | ---- | ---- | ---- |
|  r    |   只能读   |  报错    |    -  |
|   r+   |    可读可写  |   报错   |   是   |
|  w    |  只能写    |   创建   |   是   |
|  w+    |   可读可写   |   创建   |    是  |
|   a   |   只能写   |   创建   |  否，追加写    |
|   a+   |   可读可写   |   创建   |   否，追加写   |
```
b为二进制模式，可以处理除了文本之外的图片、音频、视频等格式的文件
```
### 2 使用
读文件1：
```
打开文件需要手工关闭，否则文件会一直占据内存
f=open('a.txt','r',encoding='utf-8')
data=f.read()
print(data)
f.close()
```
读文件2
```
无需手动关闭
with open('a.txt','a+',encoding='utf-8') as f:
    f.write('hello')
    print(f.read())
```
### 3 文件内光标的移动read、seek、tell、truncate

除去read( )是以字符进行读取的，seek、tell、truncate是以字节为单位进行的

 - read
read(3)表示读取3个字符

 - seek
 seek(3)表示光标向后移动3个字节，默认是从光标在文件最前面开始，也即seek(3,0)
 seek(3,1) 表示从光标上一次所在的位置向后移动3个字节
 seek(-3,2)表示从文件末尾开始向前移动
 光标的相对移动需要在打开时指定b模式，seek的第二个参数表示移动方式，0为默认方式，即从文件最开头移动，1是相对上一次移动，2是从文件末尾移动

 - truncate
truncate(3)表示截取文件前3个字节，必须以可写的方式打开，w和w+除外

 - tell
tell( )表示显示当前光标所在的字节

## 3、Python读写JSON文件

```
dumps()  将一个python对象编码为json对象
loads() 讲一个json对象解析为python对象
dump() 将python对象写入文件
load()从文件中读取json数据
```

### 1 先读后写
```
#!/usr/bin/python
import json
with open("replayScript.json", "r",encoding='utf-8') as jsonFile:
    data = json.load(jsonFile)

tmp = data["location"]
data["location"] = "NewPath"

with open("replayScript.json", "w") as jsonFile:
    json.dump(data, jsonFile,ensure_ascii=False)
```

### 2 读写一起 移动文件位置指针
```
with open("replayScript.json", "r+",encoding='utf-8') as jsonFile:
    data = json.load(jsonFile)

    tmp = data["location"]
    data["location"] = "NewPath"

    jsonFile.seek(0)  # rewind
    json.dump(data, jsonFile,ensure_ascii=False)
    jsonFile.truncate()
```
### 3 数组类型读写

移除jsonarray最后一个元素
```
#!/usr/bin/python
import json

with open("./config/patchconfig/patch_log.json", 'r+',encoding='utf-8') as f:
    log = json.load(f)
    log.pop(len(log) - 1)
    f.seek(0)
    json.dump(log, f,ensure_ascii=False)
    f.truncate()
```

