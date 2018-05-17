title: windows之注册表
date: 2016-07-01 06:57:31
categories: python
tags:
---
## 添加注册表项
```bash
Windows Registry Editor Version 5.00

[-HKEY_CLASSES_ROOT\folder\shell\cmd]

;[HKEY_CLASSES_ROOT\folder\shell\cmd]

;@="Open Commond"

;[HKEY_CLASSES_ROOT\folder\shell\cmd\command]

;@="cmd.exe /k cd %1"

```
## 删除注册表项
```bash
[-HKEY_CLASSES_ROOT\Directory\Background\shell\runas]


[-HKEY_CLASSES_ROOT\Drive\shell\runas]
```