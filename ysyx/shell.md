# Linux 的常用操作

## ls

- -l :列出每个文件的属性

  开头为d:目录，-：普通文件

- -a：列出隐藏文件
- -al：全（ 简写为ll ）

## touch

创建文件

## cd

cd -   : 回到上两个目录

## cat

看文件

### head

只看文件的开头

head --lines=2 文件名：只看前两行

### tail

只看文件的尾部

同上

## less

看文件的全文

按q退出

## file

可获取单一文件的信息

## where

查文件位置

## echo

打印

h="hello"

echo $h :hello

## mv

mv a b : 将a重命名为b

mv souce /path/to/destation:移动文件

## cp

复制文件

cp source destination

-r：递归，用于目录的复制

## rm

移除

-r或-R：递归，使得rm递归删除目录及其内容，如果指定的是一个目录，则该目录中的所有文件和子目录都会被删除，然后是该目录本身

-f：强制删，会忽视不存在的文件，并且不会确认

# shell脚本

#! ：让操作系统知道用哪个程序来解释和运行脚本

#!/bin/bash : 表示使用Bash shell 解释器

#!/usr/bin/python3 : 表示使用Python 3解释器

#!/usr/bin/env python3 : 表示使用Python 3解释器



set -e : 使得脚本在遇到任何未被捕获的错误（即命令返回非零退出状态）时立即终止执行



















