---
title: Quicker自托管补全引擎使用说明
categories:
  - 编程
date: 2022-01-22 13:08:50
tags: ['Quicker']
---

# Quicker自托管补全引擎使用说明

## 准备工作

### 1. 修改 Hosts 文件  

1. 向 C:\Windows\System32\drivers\etc\hosts 中添加下面一行记录  
	`127.65.43.21 cc.getquicker.cn`
2. 如果您使用 Clash，还需要将 [cc.getquicker.cn](http://cc.getquicker.cn) 添加到 bypass 中  
![](images/001 image.png)
![](images/002 image_1.png)

### 2. 添加 端口IP 重定向  

在管理员 Powershell 或 cmd 中运行  

```Go
netsh interface portproxy add v4tov4 listenport=80 listenaddress=127.65.43.21 connectport=5000 connectaddress=127.0.0.1  
netsh interface portproxy add v4tov4 listenport=433 listenaddress=127.65.43.21 connectport=5000 connectaddress=127.0.0.1

```

## 使用说明

### 添加要补全的dll

存放在LocalLibs目录中。需同时有dll和xml文件

### 添加命名空间

只进行完上一步的话，只能补全`namespace.class`，如果想要直接补全`class`，需要修改Namespaces.txt文件