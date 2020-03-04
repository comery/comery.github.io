---
title: Get Wikipedia page using python
date: 2018-08-26 19:05:02
tags: [python, wikipedia, crawler]
categories: 常用工具
---

### **background**

I got a long list of animals, about 9000, and I need to figure out where each animal lives, specially whether lives in Africa. Unfortunately, there is no one database containing this kind information, all website I can got from internet is something like common knowledge other than a scientific list. Finally, I realized I forget a powful tool - wikipedia, but how to make it? I can not search them one by one, it must kill me. wait, I can code to solve this problem, come on, I am a bioinformatican.

<!-- more -->

### **python module - wikipedia**

**[Wikipedia](https://github.com/goldsmith/Wikipedia)** is a Python library that makes it easy to access and parse data from Wikipedia.  
you can install by pip:  

```shell
pip install wikipedia
```

Read the docs at https://wikipedia.readthedocs.org/en/latest/.
**[Quickstart](https://wikipedia.readthedocs.io/en/latest/quickstart.html)**  
**[Full API](https://wikipedia.readthedocs.io/en/latest/code.html)**
### **achievement**

I post my codes here:

```python
import wikipedia
import sys
import os
import subprocess

if len(sys.argv) < 3:
    print("Usage: python " + sys.argv[0] + "item_list " + "outdir")
    exit()

file = sys.argv[1]
outdir = sys.argv[2]
if not os.path.exists(outdir):
    os.mkdir(outdir)
log = open("search_wiki.log",'a')
log.write("#item\tsuggestion_item\turl\n")

with open(file) as fh:
    for item in fh.readlines():
        item = item.rstrip()
        if item[0] != "#":
            re_item = item.replace(' ','_')
            print(item)
            try :
                wikipedia.summary(item)
            except wikipedia.exceptions.PageError:
                log.write(item + "\tNA\n")
                pass
            except wikipedia.exceptions.DisambiguationError:
                log.write(item + "\tNA\n")
                pass
            else:
                page = wikipedia.page(item)
                url = page.url
                log.write(item + "\t" + url + "\n")
                cmd = "wget "  + " -O " + outdir + "/" + re_item +".html " + " -q " + url
                subprocess.call(cmd , shell=True)
                subprocess.call("sleep 0.5", shell=True)
```