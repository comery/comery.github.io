---
title: Mitochondrioal genome assembly appraches
date: 2018-11-06 20:48:13
tags: [bio-software, mitochondrion, assembly]
categories: 生物信息
---
### Mitochondrioal genome assembly appraches

#### SOAPdenovo-Trans
##### link
https://github.com/aquaskyline/SOAPdenovo-Trans

<!--read more-->

##### example
```shell
SOAPdenovo-Trans-127mer pregraph -s assembly.lib -K 61 -o out -n -i 10
SOAPdenovo-Trans-127mer contig -g out  -e 2 -M 3 -q 5 -Q 2
SOAPdenovo-Trans-127mer map -s assembly.lib -g out -r -f
SOAPdenovo-Trans-127mer scaff -g out  -L 125
```
assembly.lib is like:

```text
max_rd_len=150  
[LIB]  
avg_ins=150  
reverse_seq=0  
asm_flags=3  
map_len=32  
q1=test_1.fq.gz.clean.gz  
q2=test_2.fq.gz.clean.gz
```
#### ORG.asm
##### link
https://pythonhosted.org/ORG.asm/index.html

##### example
```shell
oa index --estimate-length=0.9 butterfly papi_R1.fastq.gz papi_R2.fastq.gz
oa buildgraph --seeds protMitoMachaon butterfly butterfly.mito
oa unfold butterfly butterfly.mito
```

#### NOVOPlasty
##### link
https://github.com/ndierckx/NOVOPlasty
##### usage
```perl
perl NOVOPlasty.pl -c config.txt
```
##### example
example of config.txt:

```text
Project:
-----------------------
Project name          = Test
Type                  = mito
Genome Range          = 12000-22000
K-mer                 = 39
Max memory            = 
Extended log          = 0
Save assembled reads  = no
Seed Input            = Seed.fasta
Reference sequence    = /path/to/reference_file/reference.fasta (optional)
Variance detection    = no
Heteroplasmy          = 
HP exclude list       =
Chloroplast sequence  = /path/to/chloroplast_file/chloroplast.fasta (only for "mito_plant" option)

Dataset 1:
-----------------------
Read Length           = 151
Insert size           = 300
Platform              = illumina
Single/Paired         = PE
Combined reads        =
Forward reads         = /path/to/reads/reads_1.fastq
Reverse reads         = /path/to/reads/reads_2.fastq

Optional:
-----------------------
Insert size auto      = yes
Insert Range          = 1.8
Insert Range strict   = 1.3
Use Quality Scores    = no
```