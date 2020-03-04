---
title: 进化树也能顾盼生姿
date: 2017-08-02 10:22:38
tags: [visulization, ITOL, phylogenetic tree]
categories: 生物信息
---

最近需要画一张包含进化树和不同基因GC含量的图，研究了一番，觉得用热力图表示GC含量比较合适一点。可以完成这项工作的工具其实有很多，今天主要分享两种比较常见和好用的工具，其一是ITOL网页版工具，其二是R里的ggtree包（Y叔出品，值得安利）。

### ITOL
[ITOL](http://itol.embl.de)是一款非常强大的进化树注释，修饰，展示的工具，文章的摘要如下：
>Interactive Tree Of Life (http://itol.embl.de) is a web-based tool for the display, manipulation and annotation of phylogenetic trees. It is freely available and open to everyone. The current version was completely redesigned and rewritten, utilizing current web technologies for speedy and streamlined processing. Numerous new features were introduced and several new data types are now supported. Trees with up to 100,000 leaves can now be efficiently displayed. Full interactive control over precise positioning of various annotation features and an unlimited number of datasets allow the easy creation of complex tree visualizations. iTOL 3 is the first tool which supports direct visualization of the recently proposed phylogenetic placements format. Finally, iTOL's account system has been redesigned to simplify the management of trees in user-defined workspaces and projects, as it is heavily used and currently handles already more than 500,000 trees from more than 10,000 individual users.

 