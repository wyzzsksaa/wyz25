# 规则

![Screenshot 2024-11-21 192053](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2024-11-21 192053.png)

**target**：可以是一个object file（目标文件），也可以是一个可执行文件，还可以是一个标签（label）。对于标签这种特性，在后续的“伪目标”章节中会有叙述

**prerequisites**：生成该target所依赖的文件和/或target。

**recipe**：该target要执行的命令（任意的shell命令）。

反斜杠（ `\` ）是换行符的意思，如果命令太长，你可以使用反斜杠（ `\` ）作为换行符



**实例**：

![Screenshot 2024-11-23 020805](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2024-11-23 020805.png)

此时输入make build即为编译

gcc中-c选项用于告诉编译器只进行编译操作，不进行链接操作

# 成分

![Screenshot 2024-12-27 204940](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2024-12-27 204940.png)

Makefile中，命令前加上@符号表示执行该命令时，不将此命令本身回显到标准输出。有助于保持输出清晰，避免显示每个命令的细节



该图从上往下：

## 变量

### CFLAGS：编译器标志

-g：生成调试信息

-02：优化代码以提高性能

-Wall：启用所有警告信息

-Wextra：启用额外警告

-Isrc：将src目录包含在头文件搜索路径中

-rdynamic：标记所有符号为动态

-DNDEBUG：定义宏NDEBUG，通常用于禁用调试断言

$(OPTFLAGS)：允许传递额外的优化标志

-I：指定头文件的搜索路径



### LIBS

-ldl：链接动态链接器库

$(OPTFLAGS)：允许指定额外的库

### PREFIX

指定安装前缀，默认为/usr/local

?=的作用：只在没有PREFIX设置的平台上运行Makefile时有效

### 自定义

**例**：

**SOURCES**：使用wildcard找到src目录及其子目录中的所有.c文件

需要提供src/* */ *.c 和 src/ *.c以便CNU make能够包含src目录以及其子目录的所有此类文件

**OBJECTS**：将每个.c文件替换成其对应的？.o文件

一旦创建了源文件列表，你可以使用patsubst命令获取*.c文件的SOURCES来创建目标文件的新列表，可以告诉patsubst把所有 %.c 扩展为%.o，并把它们赋值给OBJECTS

**TEST_SRC**：寻找所有用于单元测试的测试源文件，它们存放在不同目录中

**TESTS**：使用相同的patsubst技巧来动态获取所有TEST目标，其中去掉了.c后缀，使整个程序使用相同的名字创建

### TARGET

指定目标库名称

### SO_TARGET

将静态库目标转换为共享库目标



**对于让其他人扩张结构的解释**：

make PREFIX=/tmp install

make OPTFLAGS=-pthread

如果你传入匹配Makefile中相同名称的变量，他们会在构建中生效，可以利用它来修改Makefile的运行方式，第一条命令改变了PREFIX，使它安装到/tmp。第二条设置了OPTFLAGS，为之添加了pthread



## 规则

在没有提供目标时make会默认运行第一个目标，这里它叫all，并且它提供了$(TARGET) tests 作为构建目标。而TARGET变量就是库文件，所以all：会首先构建出库文件，之后，tests目标会构建单元测试

### dev

设置额外的调试标准，然后构建所有内容（这里是all）

### $(TARGET)

构建静态库

**-fPIC**：生成位置无关代码，适用于共享库

**ar rcs $@ $(OBJECTS)**：将当前目标的名称放在这里，并把OBJECTS的内容放在后面。这里$@值为$(TARGET)，它实际上为build/libYOUR_LIBRARY.a。看起来在这一重定向中他做了很多跟踪工作，它也有这个功能，并且你可以通过修改顶部的TARGET，来构建一个全新的库

**ranlib**：更新存档的符号表

最后，在TARGET上运行ranlib来构建这个库

下面两行用于在build/ 和 bin/目录不存在的条件下创建它们

### $(SO_TARGET)

从静态库和对象构建共享库

## 单元测试

如果你拥有一个不是“真实"的目标，只有有个目标或者文件叫这个名字，你需要使用.PHONY：标签来标记它，以便make忽略该文件

## 检查工具/check

### BADFUNCS

该变量名用来标记一些不应该使用的函数

**[ ^_.>a-zA-Z0-9]**：匹配任何非字母，数字，下划线，点或大于号的字符

**(str(n?cpy|n?cat|n?dup|xfrm|str|pbrk|tok_))**：匹配以str开头的一系列函数，包括但不限于strcpy，srtcat，strxfrm，strdup，strstr，strpbrk，strtok等

**stpn?cpy**：匹配 stpcpy 或 stpncpy

**a?sn?printf**：匹配 astpcpy 或 snprintf

**byte_**：匹配 byte_

### @echo

用来告诉make不要打印命令，只需打印输出

### egrep

对源文件运行egrep命令来寻找任何危险的字符串，最后||true是一种方法，用来防止make认为egrep没有找到任何东西是执行失败

  

## 赋值条件

?=：

- **仅当变量未被定义时，才进行赋值。**
- 如果变量已经定义（即使为空），`?=` 不会覆盖它的值。

# 库相关

## LDFLAGS

通常用于传递给编译器（如gcc或g++）的选项，这些选项会被传递给链接器

，主要用于指定库搜索路径（通过-L），链接脚本等

`LDFLAGS += -L/path/to/library -Wl，-rpath，/path/to/library `

而 `-Wl,-rpath,/path/to/library` 则是传递给链接器的一个选项，用于设置运行时库搜索路径

## LDLIBS

主要用于指定程序链接时需要链接的库（通过 -l),这包括标准库、第三方库等

# 自动变量

$@：用于表示当前规则的目标文件（target）

$^：用于表示规则中所有的依赖文件

$<：当前规则的第一个依赖文件   				 $(CC) -c $<

$?：比目标文件更新的的所有依赖文件		 $(CC) -o $@ $?

$+：所有依赖文件（包括重复的）				$(CC) -o $@ $+

$*：目标文件的主文件名（不含拓展名）     $(CC) -c  $ *.c





# 函数

patsubst

wildcard

addprefix：用于在每个路径前添加前缀

shell：$(shell command)：用于执行shell指令

abspath：获取文件路径

call：用来调用用户自定义的函数

$(call function, param1,param2, ...)：function：函数，param，参数