---
title: 更改fasta序列每一行显示的碱基数目
date: 2017-04-14 16:44:23
tags: [perl, fasta, wapper]
categories: 生物信息
---

##### 一个Perl小程序

``` perl
#!/usr/bin/perl -w
use strict;
die "perl $0 <*.fa> <number of one line>" unless (@ARGV == 2) ;
my $genome=shift;
my $num = shift;
open (IN,$genome) or die $!;
$/=">";<IN>;$/="\n";
while (<IN>) {
    chomp;
    my $id = $_;
    print ">$id\n";
    $/=">";
    my $seq=<IN>;
    chomp $seq;
    $seq =~ s/\n//g;
    1 while $seq =~ s/(\w{$num})(\w+)$/$1\n$2/;
    print "$seq\n";
    $/="\n";
}
close IN;
```