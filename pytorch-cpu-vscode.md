---
title: 'PyTorch cpu VScode'
date: 2020-05-05 20:06:44
tags: []
published: true
hideInList: false
feature: 
isTop: false
---

## 1.创建虚拟环境
conda create -n pytorch #创建个叫pytorch的虚拟环境
source activate pytorch #激活他

## 2.官网
因为没GPU，所以选无cuda版
https://pytorch.org/get-started/locally/
![无cuda](https://StevenJokes.github.io//post-images/1588680447965.jpg)

这里conda使用的是清华源所以把-c去掉
conda install pytorch torchvision cpuonly

## 3.VScode设置
conda install ipykernel
打开settings（CTRL+ALT+p 搜settings)设置为
"python.pythonPath": "C:\\ProgramData\\Anaconda3\\envs\\pytorch\\python.exe",
![](https://StevenJokes.github.io//post-images/1588681064137.jpg)
且在IDE中选择pytorch
![](https://StevenJokes.github.io//post-images/1588680694919.jpg)
