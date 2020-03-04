---
title: '[BUG]-diamond of parsering xml result'
date: 2018-08-23 11:16:04
tags: [bio-software, diamond, blast2GO]
categories: 坑坑不息
---

### ERROR
>Parse error for this part of the blast XML document: Error on line 1042356: The entity name must immediately follow the ‘&’ in the entity reference.

When I use diamond replacing blast to search NR database, the result with xml format can't be parsed by blast2GO. Aftering see this [page](https://stackoverflow.com/questions/20230551/getting-a-the-entity-name-must-immediately-follow-the-in-the-entity-referenc), I got it soloved.

<!-- more -->

### & and "\&amp;"
why use "\&amp;" to replace "&"?  
when you type ```&amp;``` in a markdown or html file, you will find it turning into ```&``` directly.<br>If you want type ```&amp;```, you need add a ```\``` in front it, like: ```"\&amp;"```.

### <"xxx"> in line
If your xml has line contain ```"<word>"``` but that is not a xml format tag, so that there is no ```"<\word>"``` paried with it. then error occurs.

### ```<```in line
if there is ```<```in line and ```>``` in somewhere behind in the same line,(for fasta format, that is very common), it also occur error.


