---
title: compare three methods to parse fasta using python
date: 2019-01-24 23:14:56
tags: [python, fasta, reverse & complement]
categories: 生物信息
---

当我们在写一个比较复杂的脚本时，往往会涉及到很多小的函数，但是实现其功能的写法有很多种，如果能够清楚他们的速度，那么又便于提升我们代码整体的效率。

举一个非常简单而又常用的例子，用python写一个转化fasta文件的反向互补序列的脚本。

实现的方法有很多种，我们这里只讨论3中比较常用的：

1. replace()
2. dictionary
3. transtable()

<!-- more -->

```python
import time

def revcom_base(sequence):
    # make a sequence complement #
    sequence = sequence.upper()
    sequence = sequence.replace('A', 't')
    sequence = sequence.replace('T', 'a')
    sequence = sequence.replace('C', 'g')
    sequence = sequence.replace('G', 'c')
    return sequence.upper()[::-1]

def revcom_dict(s):
    complement = { 'A' : 'T', 'G' : 'C', 'C' : 'G', 'T' : 'A',
                 'a' : 'T', 'g' : 'C', 'c' : 'G', 't' : 'A'}
    t = ''
    for base in s:
        t = complement[base] + t
    return t

def revcom_transtable(sequence):
    # make a sequence complement #
    # replace function of string is too low!
    sequence = sequence.upper()
    transtable = str.maketrans('ATCG', 'TAGC')
    sequence = sequence.translate(transtable)
    return sequence[::-1]

```

以上是三个函数的实现，那我们下载一个数据来测试一下，哪个更快一些，以700K的数据为例；

```python
def read_fasta(fp):
    name, seq = None, []
    for line in fp:
        line = line.rstrip()
        if line.startswith(">"):
            if name: yield (name, ''.join(seq))
            name, seq = line, []
        else:
            seq.append(line)
    if name: yield (name, ''.join(seq))

def run_time(info):
    print("[INFO]: {0} total run time: {1:.2f}".format(info,
                                                       time.time() - t) + "s")

seqs = []
with open('sequence.fasta') as fp:
    for name, seq in read_fasta(fp):
        seqs.append(seq)

# method1
t = time.time()
for i in seqs:
    a = revcom_base(i)
run_time("method1")
t = time.time()

# method2
for i in seqs:
    b = revcom_dict(i)
run_time("method2")
t = time.time()

#method3
for i in seqs:
    c= revcom_transtable(i)
run_time("method3")
```

运行结果：

```text
[INFO]: method1 total run time: 0.01s
[INFO]: method2 total run time: 24.47s
[INFO]: method3 total run time: 0.00s
```
p.s. 开始用了一个23M的数据测试，第二种方法1个小时还没跑完...T_T''  
三种方法的比较十分明了，这其中的主要原因是第二种方法需要单个碱基进行置换，并且是调用str方法连接，这是比较慢的步骤。