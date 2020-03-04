---
title: circos-text-incomplete
date: 2018-07-29 23:31:54
tags: [bio-software, circos, plot]
categories: 生物信息
---
circos 图可以通过plot中的text功能增加基因名称或者注释文字，这是一个非常常用的功能，最近我在画一个线粒体基因组的circos图的时候，发现一直有基因名称显示不全，调试了很多版本和参数，都没有解决。后来通过仔细阅读官网的例子，发现有一个参数: label_snuggle , 需要将这个参数设置为'yes'这样的话可以在相近的文字有可能重叠的时候将他们分开，通过连线的方式将文字向外延伸。
比如下面的两个例子。

### example 1

![example1](http://ogj9k5cjf.bkt.clouddn.com/github/png/example1.png)


### example 2
![example2](http://ogj9k5cjf.bkt.clouddn.com/github/png/example2.png)
配置文件中增加：
```
label_snuggle         = yes
max_snuggle_distance  = 1r
snuggle_tolerance     = 0.25r
snuggle_sampling      = 2
```

当然还有另外一种方法，那就是减小文字大小，当文字足够小就不会相互重叠了。

顺便安利一波自己写的脚本，可以根据注释好的genebank文件直接画circos图，包含基因结构，GC含量，以及深度信息（如果有给出深度信息）。[链接地址](https://github.com/comery/Bioinformatics-Perl/tree/master/mito2circos)