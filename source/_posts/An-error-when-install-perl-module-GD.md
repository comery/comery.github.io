---
title: 'An error when install perl module: GD'
date: 2018-08-15 14:58:12
tags: [perl, circos, modules]
categories: 生物信息
---

### Background

Circos is a very useful data visualization tool for genome or other genetic data. However, installing circos process is not easy for a bioinofmatics beginner  or person without much coding knowledge. Circos depends many perl modules, like:

<!-- more -->

- Carp
- Clone
- Config::General
- Cwd
- Data::Dumper
- Digest::MD5
- File::Basename
- File::Spec::Functions
- File::Temp
- FindBin
- Font::TTF::Font
- GD
- GD::Polyline
- Getopt::Long
- IO::File
- List::MoreUtils
- List::Util
- Math::Bezier
- Math::BigFloat
- Math::Round
- Math::VecStat
- Memoize
- POSIX
- Params::Validate
- Pod::Usage
- Readonly
- Regexp::Common
- SVG
- Set::IntSpan
- Statistics::Basic
- Storable
- Sys::Hostname
- Text::Balanced
- Text::Format
- Time::HiRes 

P.S. recommend to install `Params::Validate` last one.

### Error
fortunately, most of above can be install by cpan, but GD failed lots of time. everytime error like this:

```
Package gdlib was not found in the pkg-config search path.
Perhaps you should add the directory containing `gdlib.pc' to the PKG_CONFIG_PATH environment variable
No package 'gdlib' found
```
### Solve

because GD module depends libgd, libgd is an open source code library for the dynamic creation of images by programmers. you mush install it first.
see more: [libgd](http://libgd.org)

```bash
wget https://github.com/libgd/libgd/releases/download/gd-2.2.5/libgd-2.2.5.tar.gz
tar zxvf libgd-2.2.5.tar.gz
cd libgd-2.2.5
./configure 
make
make install
make installcheck
```
if you are not root, or you just want install for yourself, you need install add -prefix=

```
./configure -prefix=/home/yourname/local
make
make install
make installcheck
```
then, you can use cpan to insall GD module.

