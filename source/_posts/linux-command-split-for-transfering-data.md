---
title: linux command split for transfering data
date: 2020-01-16 15:04:06
tags: [linux, split, transfer, bam]
categories: 常用工具
---

科研合作经常面临的一个问题是，需要及时的共享数据，对于不涉及太大数据的人来说，一个百度云盘或者谷歌drive就搞定了，但是对于做生物信息分析的，很多时候需要分享的是bam，fastq一类的文件，小则几百G，大到上T的数据，想想都头疼。不过这个问题也不难解决，搭建一个ftp就可以实现数据传输，尤其是和其他国家的合作者。

最近就遇到这个情况，当然这里并不是分享如何搭建FTP了，而是讲讲数据传输时候需要注意的问题。传输的过程中最常遇到的问题应该是网络的不稳定导致的数据传输中断，尤其是单个文件过大，所以对大文件切分是十分有必要的。

起初我要传一个很大的bam文件，是四个样本merge之后的，大概86G，同事帮我传的时候说请将每个文件拆分成单个文件不超过8G，我想了想，那把每个样品的bam文件拆开吧，但是这样也超过8G了呀，难道需要把单个bam文件转成sam文件再拆分（因为bam文件是二进制的，如果要按行操作，肯定是想着转成sam操作），好复杂啊，有没有什么更好的方式呢？随即我想到了linux的split命令，不过我只用它拆分过文本文件，加`-l`参数可以按照指定的行数拆分，难道也可以按照字节数拆分？

输入命令一查，还真是如此：

```shell
SPLIT(1)                  BSD General Commands Manual                 SPLIT(1)

NAME
     split -- split a file into pieces

SYNOPSIS
     split [-a suffix_length] [-b byte_count[k|m]] [-l line_count] [-p pattern] [file [name]]

DESCRIPTION
     The split utility reads the given file and breaks it up into files of 1000 lines each.  If file is a single dash (`-') or absent, split
     reads from the standard input.

     The options are as follows:

     -a suffix_length
             Use suffix_length letters to form the suffix of the file name.

     -b byte_count[k|m]
             Create smaller files byte_count bytes in length.  If ``k'' is appended to the number, the file is split into byte_count kilobyte
             pieces.  If ``m'' is appended to the number, the file is split into byte_count megabyte pieces.

     -l line_count
             Create smaller files n lines in length.

     -p pattern
             The file is split whenever an input line matches pattern, which is interpreted as an extended regular expression.  The matching
             line will be the first line of the next output file.  This option is incompatible with the -b and -l options.

     If additional arguments are specified, the first is used as the name of the input file which is to be split.  If a second additional
     argument is specified, it is used as a prefix for the names of the files into which the file is split.  In this case, each file into
     which the file is split is named by the prefix followed by a lexically ordered suffix using suffix_length characters in the range
     ``a-z''.  If -a is not specified, two letters are used as the suffix.
     If the name argument is not specified, the file is split into lexically ordered files named with the prefix ``x'' and with suffixes as
     above.

```

划重点，```-b```参数就可以对二进制文件按照字节来拆分，所以要拆分成大小为8G的文件就应该是： ```split -b 8G file.bam```希望以上内容对大家的日常工作有所帮助。