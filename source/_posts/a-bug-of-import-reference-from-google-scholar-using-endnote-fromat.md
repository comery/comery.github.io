---
title: A bug of import reference from google scholar using endnote fromat
date: 2018-12-16 14:15:12
tags: [paper, endnote]
categories: 文献阅读
---
发现一个endnote导入文件的bug,我们首先来看一下一般的导入操作：

### 用endnote导入google scholar的参考文献，导出格式为Endnote格式
以一篇文章刚发表在bioRxiv上的文章为例：

```text
MitoZ: A toolkit for mitochondrial genome assembly, annotation and visualization
```
<!-- more -->

#### step 1, 打开google scholar,搜索然后下载：

![step1](https://comery-1258590047.cos.ap-guangzhou.myqcloud.com/20181216/endnote_googlescholar.png)

如果是单个引文下载，下载的文件后缀名为“.ris”， 如果是多个引文下载，下载文件名后缀为“.enw”，很奇怪的是，虽然名为.ris格式，但是用Endnote 导入时，选择```Reference Manager(RIS)```是没有反应的，所以只能选择```EndNote Import``` 格式


#### step 2, 打开Endnote软件导入

<img src="https://comery-1258590047.cos.ap-guangzhou.myqcloud.com/20181216/endnote_googlescholar1.png" height="300" width="500" >

OK，导入成功后，EndNote这里已经出现了这个文献，然后点击选择，会发现 ```anutors``` 字段和```journal```字段是混在一起的，用```%J```连接。

<img src="https://comery-1258590047.cos.ap-guangzhou.myqcloud.com/20181216/endnote_googlescholar11.png" height="300" width="300" >

很明显，如果不仔细检查，你之后加入到文章中的文献就会有问题，

![step3](https://comery-1258590047.cos.ap-guangzhou.myqcloud.com/20181216/endnote_googlescholar2.png)

### 解决方案，用RefMan 替代EndNote格式

<img src="https://comery-1258590047.cos.ap-guangzhou.myqcloud.com/20181216/endnote_googlescholar3.png" height="500" width="500" >

这样下载的文件格式也是RIS格式，而且在导入的时候可以直接使用```Reference Manager(RIS)```格式，比较一下，发现这次没有问题。

<div align="center">

<img src="https://comery-1258590047.cos.ap-guangzhou.myqcloud.com/20181216/endnote_googlescholar6.png" height="200" width="200" >

<img src="https://comery-1258590047.cos.ap-guangzhou.myqcloud.com/20181216/endnote_googlescholar7.png" height="200" width="200" >

</div>

![step7](https://comery-1258590047.cos.ap-guangzhou.myqcloud.com/20181216/endnote_googlescholar5.png)
