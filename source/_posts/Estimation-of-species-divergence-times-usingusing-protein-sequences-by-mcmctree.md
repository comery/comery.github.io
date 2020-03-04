---
title: >-
  Estimation of species divergence times usingusing protein sequences by
  mcmctree
date: 2019-07-09 10:10:23
tags: [bio-software, phylogenetic tree, mcmctree, codeml]
categories: 生物信息
---

### 1. prepare a phylogenetic tree with fossil evidence time

The first line contains: species number(Tab)fossil record number

>example:
18   1
((((((((((((PET,(((PEFU,PN),PEFC),PEEL)),PEE),PT),PO),PF),PC15),(LB,CC)),SC)'>0.90<1.2833',(CP,SL)'>0.9898<1.3899'),FM),AS),SN);


* control file

```text
seed = -1
seqfile = 71genes.gblocks.protein.linked.fa
treefile = three.fossil.tree
mcmcfile = mcmc.txt
outfile = out.txt
ndata = 1
seqtype = 2 * 0: nucleotides; 1:codons; 2:AAs
usedata = 3 * 0: no data; 1:seq like; 2:normal approximation; 3:out.BV (in.BV)
clock = 2 * 1: global clock; 2: independent rates; 3: correlated rates
RootAge = '<7.0'  * safe constraint on root age, used if no fossil for root.
model = 0 * 0:JC69, 1:K80, 2:F81, 3:F84, 4:HKY85
alpha = 0 * alpha for gamma rates at sites
ncatG = 5 * No. categories in discrete gamma
cleandata = 0 * remove sites with ambiguity data (1:yes, 0:no)?
BDparas = 1 1 0.1 * birth, death, sampling
kappa_gamma = 6 2 * gamma prior for kappa
alpha_gamma = 1 1 * gamma prior for alpha
rgene_gamma = 2 201 * gamma prior for rate for genes
sigma2_gamma = 1 10 1 * gamma prior for sigma^2     (for clock=2 or 3)
finetune = 1: .1 .1 .1 .1 .1 .1 * auto (0 or 1): times, rates, mixing, paras, RateParas, FossilErr
print = 1 * 0: no mcmc sample; 1: everything except branch rates 2: everything
burnin = 2000
sampfreq = 10
nsample = 20000
```

### 2. run mcmctree 

```bahs
mcmctree mcmctree.ctl
```

### 3. modify settings in tmp1.ctl

```text
seqfile = tmp1.txt
treefile = tmp1.trees
outfile = tmp1.out
noisy = 3
seqtype = 2
model = 0
aaRatefile = wag.dat *from /data folder in paml's path
fix_alpha = 0
alpha = 0.5
ncatG = 4
Small_Diff = 0.1e-6
getSE = 2
method = 1
```

### 4. run codeml 

```bash
codeml tmp1.ctl
```

### 5. rename rst2 into in.BV

```bash
mv rst2 in.BV
```

### 6. modify the item of 'usdata' in mcmctree.ctl file

```text
usedata = 2
```

### 7. final run of mcmctree

```bash
mcmctree mcmctree.ctl
```
finally, you will get the tree you want!