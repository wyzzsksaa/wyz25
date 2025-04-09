# C

## 常用

%d 或 %i：表示一个有符号十进制整数。

%u：表示一个无符号十进制整数。

%o：表示一个无符号八进制整数。

%x 或 %X：表示一个无符号十六进制整数（%x 使用小写字母，%X 使用大写字母）。

%f：表示一个浮点数，默认为六位小数。

%lf：double型

%e 或 %E：表示科学计数法表示的浮点数（%e 使用小写字母，%E 使用大写字母）。

%g 或 %G：根据数值的大小，自动选择使用浮点数格式或科学计数法格式表示（%g 使用小写字母，%G 使用大写字母）。

%c：表示一个单个字符。

%s：表示一个字符串。

%p：表示一个指针的地址，通常以十六进制形式输出。

%n：不输出任何内容，但将已写入的字符数存储在指定的整数变量中。



%%：输出一个%字符本身。

此外，还有一些用于修饰格式说明符的标志和宽度、精度指定符，例如：



%-d：左对齐整数。

%05d：输出的整数至少占5个字符的位置，不足部分用0填充。

%.2f：输出的浮点数保留2位小数。



**以双下划线（`__`）开头或包含双下划线的标识符** 是 **保留给编译器和标准库实现使用的**

## 算术运算

a++：读取变量然后自增

++a：自增变量然后读取

## 数据运算

用于以不同方式和形式访问数据

-> ：结构体指针的成员访问

*：取值（提取地址）

## 位运算

更加高级，用于修改整数的原始位

二元&：位与

<<：左移

》》：右移

^：位异或

|：位或

~：取补（翻转所有位）

## 布尔运算符

`X ? Y : Z`读作“若X则Y否则Z

## 赋值运算符

<<= ：左移赋值 

》》=：右移赋值

^=：位异或赋值

|=：位或赋值

## 类型限定符

const

volatile：表示会做最坏打算，编译器不会对他做任何优化

register：强制让编译器将这个变量保存在寄存器中，且可以无视它

## 修饰前缀

extern: 使得变量能在其他.c文件中被访问

static（文件）：意思是这个变量只能在当前.c文件中使用，程序其他部分不可访问。要记住文件级别的static和其他位置不同

static（函数）：如果你使用`static`在函数中声明变量，它和文件中的`static`定义类似，但是只能够在该函数中访问。它是一种创建某个函数的持续状态的方法，但事实上它很少用于现代的C语言，因为它们很难和线程一起使用

## char *

char通常用于表示单个字符

char *常用来指向字符串（字符数组）的首地址或用于字符数组的动态分布和处理

## typedef

1.自定义

2.简化复杂的内容

3.typedef int (*compare_cb)(int a,intb);

​	定义了一个名为compare_cb的指向函数的指针

## unsigned

- 声明无符号类型，只能表示非负数
- 输出用%u %lu %llu

## inline

用于向编译器建议将函数内联展开，来减少函数调用的开销（只是个建议，编译器可能会忽略）

inline int max(int a, int b){

​		return (a > b) ? a : b;

}

调用时可能会直接展开

**与宏的区别**：

内联函数是真正的函数，支持类型检查和调试，而宏只是简单的文本替换

内联函数更安全，避免了宏可能带来的副作用

# FILE 结构体 IO函数

stdin:标准输入

stdout:标准输出

stderr:标准错误输出

## fread

int buffer[4];

size_t numRead = fread( buffer , sizeof(int) , 4 , file );

(从一个名为file的文件中读取4个整数分配给数组buffer，第二项为每个读取块的大小)

 ## fopen

FILE *file = fopen（“example.txt” ，“mode”）；

 从example.txt文件中操作

若返回值为NULL，则出错

mode：“r” ：只读。“w”：写入新内容并覆盖原有内容。“a”：从末尾追加内容。“r+” “w+” “a+'':读写

最后得用fclose释放文件

## fgets

一次只读一行

char buf[];

fgets（buf，20，file）；

20：获取的长度

file：文件的指针

## fscanf

int fscanf(FILE *stream, const char *format, ...)

FILE *stream : 指向FILE结构的指针，表示要从中读取的文件流

const char* format：格式化字符串，定义了如何解析输入

...：可变参数列表，根据格式化字符串来提供相应的指针，指向将要存储读取数据的位置

返回成功匹配和赋值的输入项的数量，如果到达文件末尾或发生读取错误，且没有成功匹配任何项，返回EOF

## rewind

void rewind（FILE  *stream）；

stream是一个指向FILE结构体的指针，rewind函数会将stream移向文件的开头。

## fwrite

size_t written = fwrite（buffer ， sizeof（char），1 , file);

buffer：指向要写入文件的数据

第二项，第三项，第四项同fread

## fflush

int fflush(FILE *stream);

stream为指向FILE的指针

fflush用于刷新文件流的缓冲区

若刷新成功，则返回0；不成功返回NULL

## fprintf

用于将格式化的输出写入到指定写入到指定的文件流，与printf类似，但printf默认是将输出打印到标准输出（通常是屏幕），而fprintf可以指定任意的文件流作为输出目标

int fprintf(FILE *stream, const char *format, ... );

FILE *stream : 指定输出的目标文件，如果是标准输出，可用stdout，如果为标准错误输出，可用strerr，若是指定文件，则先fopen

 const char *format ：格式化字符串

返回成功写入的字符数（不包括终止空字符），如果发生错误，则返回负数

## sprintf

将数据输出到标准输出（通常是终端）

```
int sprintf(char *str, const char *format, ...);
```

str：目标字符串的指针

format：格式化字符串，指定输出的格式

成功时返回写入的字符数（不包括终止符\0)，失败时返回负值

## fgetc

用于从文件流中读取单个字符，通常被用来读取文本文件的内容，每次调用时返回文件中的下一个字符

int fgetc(FILE *stream);

  如果到达文件末尾或者发生读取错误，则返回EOF

## feof

用于检测文件流是否已经到达了文件的末尾，通常与读取文件内容的函数（如fgets，fgetc等）一起使用，以确保在尝试从文件中读取数据之前或之后没有越界访问

int feof(FILE *stream)

如果文件位置指示器位于文件末尾，则返回非零值（通常是1）

若不在末尾则返回0

## ferror

用于检测在文件流上是否发生了读写错误，当对文件进行操作时（如读入，写入），如果发生错误，相关文件流会设置一个错误标志，ferror函数可以检查这个标志，以确定是否有错误发生

int ferror(FILE *stream)

如果没有错误，则返回0

如果指定的文件流中设置错误指示器，则返回非零值（通常是1）

# 头文件

## string.h

C语言中关于字符串的函数包括但不限于：

strlen: 计算字符串长度。

strcpy: 复制字符串。

strncpy：用于将指定长度的字符串复制到字符数组中

- char *strncpy（char *dest，const char *src，size_t n）；

- dest：指向目标字符数组的指针
- src：源字符串的指针
- n ： 要复制的字符数

strcat: 连接字符串。

strcmp: 比较字符串。

strchr: 查找字符在字符串中的位置（从左）

strrchr：同上（从右）

strstr: 查找子字符串在字符串中的位置。

strdup:将一个字符串拷贝到新的内存地址中，并返回该地址的指针，调用了malloc

**memcpy**：void* memcpy（void* dest，const void* src，size_t n)

​			用于从源地址src复制n个字节的数据到目标地址dest

​			它假设源和目标区域不重叠；如果确实存在重叠，则行为是未定义的

**memmove**: void* memcpy（void* dest，const void* src，size_t n)

​			功能类似于memcpy，但解决了重叠问题

​			即使有重复部分，memmove也能保证正确复制数据

**memset**: void* memset(void* s,int c, size_t n);

​			将指定值从c（转换为无符号字符）填充到指针s指向的前n个字节中

​			常用于初始化或清除内存区域，例如将一个数组的所有元素设置为0

## ctype.h

- isalphe:检查提供的字符是否是字母

- isblank:检查提供的字符是否是

    **toupper**：将小写字母转换为相应的大写字母，

    **tolower**：相反

## stdlib.h

- **rand()**:用于生成伪随机数，返回一个0~max的整数，max由<stdlib.h>定义，通常为32767.若每次程序运行时不加以干预，将生成相同的序列
- **srand()**:调用srand并传给他一个种子值时，随机数生成器会被重置，若使用相同种子值，随后的rand生成序列相同

- **strdup**:复制字符串，同时会将原来的字符串复制到新建的内存

- **calloc**：用于动态内存分布，且会将自动分配的内存区域初始化为0

  void *array = （int *）calloc（num,size);

  num:分配的元素数量。size：每个元素大小

**atoi**：用于将字符串转换成整数（int型）

​		int a = atoi（const char *str);

​		返回转换后的整数值，如果字符串不能被转换成整数或者字符串为空，则返回数字0需要注意的是，atoi不区分无效转换和数字0的情况



## assert.h

- assert():会判断传入的表达式的值，若为0（即条件为假）调用abort函数，终止程序运行；若非0（真）不进行任何操作。

## time.h

time_t:是一个数据类型，通常用来表示时间

time_t time(time_t *timer);

若timer = NULL，则time返回当前时间

## errno.h

### errno

全局变量，为int，用于存储最近一次系统调用或库函数调用调用失败时的错误代码，每个代码都对应一个特定的错误类型。

### perror

该函数能将errno的值映射为对应的错误信息

### strerror

用于将错误代码（通常是全局变量errno的值）转换为对应的错误信息字符串

printf（“Error : %s\n",strerror(errno));

## stddef.h

ptrdiff_t：一种类型定义，是一种有符号整数类型，用于表示两个指针之间的差值当从一个指针减去另一个指针时（这两个指针指向同一个数组中的元素或位于该数组的一个假想的“过去末尾”位置），结果将是一个ptrdiff_t类型的值

输出为%td

## unistd.h

主要提供了一系列与Unix操作系统相关的函数声明和宏定义，为程序提供了访问底层操作系统的接口

**sleep**：

会暂停调用它的线程或进程的执行一段时间，它接受一个以秒为单位的参数

unsigned int sleep ( unsigneed int seconds)

如果调用被信号中断，则返回剩余的睡眠时间，否则返回0

**usleep**:

接受毫秒作为参数

int usleep(useconds_t usec);

成功返回0，失败时返回-1并设置错误号（errno）

## regex.h

由于支持正则表达式的操作

### 数据类型

**regex_t：**用于储存编译后的正则表达式

**regmatch_t:** 存储匹配的子字符串的起始和结束位置

```
typedef struct {
    regoff_t rm_so;  // 子字符串的起始偏移量
    regoff_t rm_eo;  // 子字符串的结束偏移量
} regmatch_t;
```

### 函数

**regcomp（）：**

用于编译正则表达式

```
int regcomp(regex_t *preg, const char *regex, int cflags);
```

- `preg`：指向 `regex_t` 类型的指针，用于存储编译后的正则表达式。
- `regex`：要编译的正则表达式字符串。
- `cflags`：编译标志，用于控制编译行为（如 `REG_EXTENDED`、`REG_ICASE` 等）

REG_EXTENDED：使用拓展正则表达式语法

REG_ICASE：忽略大小写

REG_NOSUB：不存储匹配的子表达式

REG_NEWLINE:允许^与$匹配换行符



**regexec（）：**

用于执行正则表达式匹配

```
int regexec(const regex_t *preg, const char *string, size_t nmatch, regmatch_t pmatch[], int eflags);
```

- `preg`：编译后的正则表达式。
- `string`：要匹配的字符串。
- `nmatch`：`pmatch` 数组的大小。
- `pmatch`：用于存储匹配结果的数组。
- `eflags`：执行标志，用于控制匹配行为（如 `REG_NOTBOL`、`REG_NOTEOL` 等）。

返回0表示匹配成功，REG_NOMATCH表示没有匹配，其他值表示错误

REG_NOTBOL：字符串开头不被视为行的开头（^不会匹配字符串的开头）

REG_NOTEOL：字符串结尾不被视为行的结尾 （$不会匹配字符串的结尾）



**regfree（）：**

用于释放 `regcomp()` 编译的正则表达式

# 预处理

`#` 是一个特殊的操作符，它用于将宏参数转换成字符串字面量。这个过程通常被称为“字符串化”



##_ _ VA_ARGS_ _ :意思是将剩余的所有额外参数放到这里

_ _ FILE _ _ 和 _ _ LINE _ _ :获取当前的fine：line用于调试信息

_ _ func_ _ 和 _ _ FUNCTION_ _ 代表当前的函数的函数名，类型为字符串常量

**...** : 可变参数列表

**#ifndef**：是“如果没有被定义”的意思

**#undef**：用于取消一个宏

**NDEBUG**:通常用于控制是否启用断言（assert）

- 当NDEBUG没有被定义时，断言语句会被编译进程序中，并在运行中检查条件是否为真
- 当NDEBUG被定义时，不会被编译进最终程序中，这通常用于生产环境以提高性能，因为断言语句会在每次遇到时执行额外的检查

# 类型大小

**stdint.h**为定长的整数类型定义了一些typedef，同时也有一些用于这些类型的宏（更好移植）。这比老的limits.h更加易于使用，因为它是不变的。这些类型如下：



int8_t ：8位符号整数

uint8_t：8位无符号整数

int64_t：64位符号整数

uint64_t：64位无符号整数

当用于对类型大小有要求的特定平台时，可以使用这些类型。如果你怕麻烦，不想处理平台相关类型的今后潜在的扩展的话，也可以使用这些类型

(u)int(BITS)_t，其中u代表unsigned，BITS是所占位的大小。这些模式串返回了这些类型的最大（或最小）值



INT(N)_MAX：N位符号整数的最大正数，例如INT16_MAX

INT(N)_MIN：N位符号整数的最小负值

UINT(N)_MAX：N位无符号整数的最大正值，最小值为0

这里的N应为特定整数，如8,16,32,64,128



- 在`stdint.h`中，对于`size_t`类型和足够存放指针的整数也有一些宏定义，以及其它便捷类型的宏定义

int_least(N)_t：至少N位的整数

uint_least(N)_t：至少N位的无符号整数



INT_LEAST(N) _ MAX ：int_least(N)_t 类型的最大值

INT_LEAST(N) _ MIN ：int_least(N)_t 类型的最小值

UINT_LEAST(N) _ MAX ：uint_least(N)_t类型的最大值



int_fast(N)_t：与int_least(N) _ t类似，但是是至少N位的“最快”整数

uint_fast(N)_t：

INT_FAST(N)_MAX：int_fast(N) _ t的最大值

**最快**：满足最小宽度要求的前提下，选择该平台上执行效率最高的类型



intptr_t：足够存放指针的符号整数

uintptr_t：足够存放指针的无符号整数

INTPTR_MAX：上的最大值

INTPTR_MIN：上的最小值

UINTPTR_MAX：上的最大值



intmax_t：系统中可能的最大尺寸的整数类型

uintmax_t：系统中可能的最大尺寸的无符号整数类型

INTMAX_MAX

INTMAX_MIN

UINTMAX_MAX



PTRDIFF_MAX：ptrdiff_tde 的最大值

PTRDIFF_MIN：最小值

SIZE_MAX：size_t的最大值

# 作用域，栈

- 不要隐藏某个变量，就像上面`scope_demo`中对`count`所做的一样。这可能会产生一些隐蔽的bug，你认为你改变了某个变量但实际上没有。
- 避免过多的全局变量，尤其是跨越多个文件。如果必须的话，要使用读写器函数，就像`get_age`。这并不适用于常量，因为它们是只读的。我是说对于`THE_SIZE`这种变量，如果你希望别人能够修改它，就应该使用读写器函数。
- 在你不清楚的情况下，应该把它放在堆上。不要依赖于栈的语义，或者指定区域，而是要直接使用`malloc`创建它。
- 不要使用函数级的静态变量，就像`update_ratio`。它们并不有用，而且当你想要使你的代码运行在多线程环境时，会有很大的隐患。对于良好的全局变量，它们也非常难于寻找。
- 避免复用函数参数，因为你搞不清楚仅仅想要复用它还是希望修改它的调用者版本。



**关于 栈，堆**：

- 如果你从malloc获取了一块内存，并且把指针放在了栈上，那么当函数退出时，指针会被弹出而丢失。
- 如果你在栈上存放了大量数据（比如大结构体和数组），那么会产生“栈溢出”并且程序会中止。这种情况下应该通过`malloc`放在堆上。
- 如果你获取了指向栈上变量的指针，并且将它用于传参或从函数返回，接收它的函数会产生“段错误”。因为实际的数据被弹出而消失，指针也会指向被释放的内存

# 可变参数

如va_list, va_start, va_arg, va_end这类，允许函数接受不定数量和类型的参数。这些工具定义在<stdarg.h>中

va_list: 这是一个类型，用来声明一个变量，该变量保存遍历可变参数列表所需的信息

va_start: 这个宏初始化va_list变量，以便开始访问可变参数列表

va_arg：这个宏根据指定的类型从va_list中取出下一个参数

va_end：这个宏清理va_list，应该在所有参数都被访问后调用

# APR

## apr_errno.h

定义了一系列的错误码和宏，用于跨平台的错误处理

APR_SUCCESS：操作成功完成

APR_EBADF：无效的文件描述符

APR_ENOENT：没有这样的文件或目录

APR_EINVAL：无效参数

APR_EOF：到达文件末尾

APR__TIMEUP：操作超时

## apr_file_io.h

提供了一组跨平台的API，使得开发者可以方便的进行文件操作，而无需担心底层操作系统之间的差异

**打开文件**：apr_file_open（）用于打开一个现有文件

**创建文件**：apr_file_open() 同样可以用来创建新文件，只指定适当的标志（如APR_CREATE）

**读取文件**：apr_file_read（）从文件中读取数据到缓冲区

**写入文件**：apr_file_write() 将数据从缓冲区写入文件

**删除文件**：apr_file_remove()

**重命名文件**：apr_file_rename()



## apr_thread_proc.h

提供了与线程和进程管理相关的函数声明，包含以下几类主要功能：

### 线程管理

**创建线程**：apr_thread_create() 创建一个新的线程

**销毁线程**：apr_thread_exit() 用于从线程内部退出

**等待线程结束**：apr_thread_join() 等待另一个线程结束

**获取线程ID**：apr_thread_self()获取当前线程的标识符

**获取/设置线程属性**：

apr_threadattr_create() 创建线程属性对象

apr_threadattr_detach_set(), 设置是否分离线程

apr_threadattr_stacksize _set() 设置线程堆栈大小

### 进程管理

**创建进程**：apr_proc_create() 创建一个新的线程

**销毁进程**：apr_proc_destroy() 用于从线程内部退出

**等待进程结束**：apr_proc_wait() 等待另一个线程结束

**执行程序**：apr_proc_exec() 在当前进程中执行新的程序

# 动态库

**注意**：

- 每次调用dlopen或dlsym后都应该检查返回值是否为NULL，以确保操作成功，可以通过dlerror函数来获取详细的错误信息
- dlopen和dlclose在多线程环境中是安全的，但dlsym不保证线程安全，因此同一时间内不应该从多个线程同时调用同一个句柄上的dlsym

## dlopen

打开一个共享对象的文件，并将它链接到调用者的地址空间

void *dlopen(const char *filename, int flag);

filename：要加载的共享库文件的路径，如果传递的是NULL，则返回当前进程的全局符号表，

flag：制定如何加载库的行为标志，常用的有：

- RTLD_LAZY：延迟绑定，只有首次引用符号时才解析
- RTLD_NOW：立即绑定，加载时即解析所有未定义符号
- PTLD_GLOBAL：使库中的符号对后续加载的其他库可见

成功时返回指向共享对象的句柄；失败时返回NULL

## dlsym

根据提供的句柄和符号名称获取函数或变量的地址

void *dlsym(void *handle, const char *symbol)

handle：由dlopen返回的共享对象句柄

symbol：要查找的符号名（通常是函数或变量的名字）

成功时返回符号的地址，失败时返回NULL

## dlclose

关闭一个之前通过dlopen打开的共享对象

int dlclose(void *handle)

handle：由dlopen返回的共享对象

成功时返回0；如果有错误发生，则返回非零值

# C框架

bin/：放置可运行程序的地方，这里通常是空的，Makefile会在这里生成程序

build/：当值库和其他构建组件的地方，通常也是空的，Makefile会在这里生成这些东西

src/：放置源码的地方，通常是 .c 和 .h文件

tests/：放置自动化测试的地方

# GCC

_ _attribute _ _ ((used))：告诉编译器不要优化这个变量，即使它没被显式使用
