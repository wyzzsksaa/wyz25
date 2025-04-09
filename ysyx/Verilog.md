# 基本概念

端口信号声是要说明端口型号的输入输出属性，信号的数据类型，以及信号的位宽，

输入输出属性有input，output，inout（输入/输出双向端口）

信号的数据类型常用的有wire和reg两种

线网类型只能用连续赋值，不随敏感事件发生而改变寄存器可以随时变，因此在always块内使用的是reg类型

信号的位宽用[n1:n2]表示，n1表示最高有效位，n2表示最低有效位

**多维**：reg [7:0] my_array [0:3];

定义了一个包含4个元素的一维数组，每个元素都是8位宽

**位宽若不做说明的话默认是1位；数据类型若不做说明的话，默认是wire型的**



**向量与数组的区别**：

1. 数组可以是一维二维三维的，向量只能是一维的
2. 向量之间的元素是连续的，数组之间是相互独立的
3. 向量常用与表示数据路径的多比特信号，如总线。数组更常用于存储器建模或需要处理多个相似但独立的数据项时



integer（整数型），real（实数型），time（时间型），realtime（实数时间型）



缓冲门：

![Screenshot 2025-01-16 190921](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-16 190921.png)

## 数字声明

**4‘bz**：数据格式，表示该信号为4bit位宽，

信号共有4种状态**0，1，x，z**分别表示低电平，高电平，不确定态和高阻态，对于没有进行初始化的的信号，一般处于不确定态（x），高阻态表示该信号没有被其他信号驱动，经常用于有多个驱动源的总线型数据上，？是z的另一种表示

**‘b，’B**代表二进制数，‘o,'O代表八进制数，’d，‘D代表十进数，’h，‘H代表十六进制数

**不指明数位的数字**：默认为十进制，若没有指定位宽度，则默认的位宽与仿真器和使用的计算机有关（最小为32位）

23456 // 这是一个32位的十进制数

’hc3 // 这是一个32位的十六进制数

**负数**：例：-6‘d3

## 系统任务与编译指令

**显示信息**：$display用于显示变量，字符串或表达式的主要系统任务

$display(p1,p2,p3,...);	其中p1，p2是双引号括起来的字符串，变量或表达式

![Screenshot 2025-01-15 223834](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-15 223834.png)

**监视信息**：$monitor,格式与$display基本相同，系统函数对其参数列表中的变量值或信号值进行不间断的监视，当其中任何一个发生变化时，显示所有参数数值，且只用调用一次即可在整个仿真过程中生效

**暂停与结束仿真**：$stop $finish



**编译指令**：· define，注意前缀 “`",类似与C中的#define

`include 类似于C语言中的#include

# 模块与端口

![Screenshot 2025-01-15 225237](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-15 225237.png)

## assign语句

称为连续赋值语句

**特点**：

- 只要表达式中的操作数有变化，立即进行计算和赋值（与连续赋值语句对应的另一种语句称为过程赋值语句）
- 复制目标必须是wire型的，wire表示电路间的连线

加减乘除，求幂的操作数可以是实数也可以是整数，求余运算的操作数只能是整数

求余运算结果取第一个操作数的符号

![Screenshot 2025-01-14 203650](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-14 203650.png)

![Screenshot 2025-01-14 203819](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-14 203819.png)

||：逻辑或，&&：逻辑与

![Screenshot 2025-01-14 204134](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-14 204134.png)

![Screenshot 2025-01-14 204256](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-14 204256.png)

算数右移是保存在最高位补充符号位

![Screenshot 2025-01-14 204451](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-14 204451.png)

![Screenshot 2025-01-14 204656](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-14 204656.png)





# 数据流建模

## 延迟

**普通赋值延迟**：

例：assign #10 out = in1 & in2；会产生10个单位时间的延迟

脉冲宽度小于赋值延迟的输入变化不会对输出产生影响

**隐式连续赋值延迟**：

wire #10 out= in1 & in2 ; 与上面等价

**线网声明延迟**：

wire #10 out；assign out = in1 & in2；与上面等价



**操作符的优先级**：

![Screenshot 2025-01-16 202807](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-16 202807.png)

# 行为级建模

行为建模很容易会让初学者偏离描述电路的初衷: 开发者需要看着电路图，心里想象电路的行为，然后转化成事件队列模型的思考方式，最后再用行为建模方式来描述电路的行为，综合器再来根据这样的描述推导出相应的电路。从这个过程来看，这不仅是没有必要的，而且还很容易引入错误

如果开发者心里没有电路图，而是期望通过行为建模方式让综合器生成某种行为的电路，这就已经偏离“描述电路”的本质了。大部分同学非常容易犯这样的错误，把行为建模当作过程式的C语言来写，尝试把任意复杂的行为描述映射到电路，最终综合器只会生成出延迟大，面积大，功耗高的低质量电路

## 结构化过程语句

所有行为语句必须放在initial或always块内部

### initial 语句

initial由begin与end组成一个initial块，从仿真0时刻开始执行，且只执行一次

一般用于初始化，信号监视，生成仿真波形等

### always 语句

**基本格式**：

always  @（敏感信号条件表）

​		各类顺序语句

例：always  @(posedge CLK)

​		Q = D

- 该语句不是总处于激活状态，当满足激活条件时才能被执行，否则被挂起，挂起时即使操作数有变化，也不执行赋值，赋值目标值保持不变
- 复制目标必须是reg型的

激活条件由敏感信号条件决定，当敏感条件满足时，过程块被激活

敏感条件有两种：

**边沿敏感**：posedge：上升沿，negedge：下降沿

**电平敏感**：(信号名列表) 信号列表中的任一个信号有变化

例：（a，b，c） 当a，b，c中有一个发生变化 (**逗号可以换成or**）

![Screenshot 2025-01-14 210231](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-14 210231.png)

**assign语句与always语句之间的区别**：

![Screenshot 2025-01-14 210518](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-14 210518.png)

## 过程赋值语句

过程赋值语句的更新对象是寄存器，整数，实数或时间变量，

### 阻塞赋值语句

设计组合电路时常用阻塞赋值

用“ = ”作为赋值号

阻塞赋值语句按顺序执行，不会阻塞其后并行块中语句的执行

![Screenshot 2025-01-17 171640](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-17 171640.png)

### 非阻塞赋值语句

允许赋值调度，但不会阻塞 位于同一个顺序块中其后语句的执行

使用“ <= "作为赋值号

设计时序电路时常用非阻塞赋值

非阻塞赋值是在赋值前先完成计算，并且计算过程互不干扰

![Screenshot 2025-01-17 171554](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-17 171554.png)

## 时序控制

三种方法：

### 基于延迟的时序控制

通过表达式指定了语句从开始到执行完成的时间间隔

延迟值可以是**数字，标识符或表达式**，需要在延迟值前加上关键字#

对于过程赋值，提供了三种类型的延迟控制：

**常规延迟控**：

x = 0；

#10 y = 1；x执行后，过10个单位时间执行

**赋值内嵌延迟控制**

y = #5 x + z；等待5个单位时间内执行

**零延迟控制 #0**

同一仿真时刻，位于不同的always和initial块中的过程语句有可能被同时计算，但执行（赋值）顺序不确定，与使用仿真器类型有关，该情况下，零延迟控制语句将在执行时刻相同的多条语句中最后执行，从而避免竞争

### 基于事件的时序控制

事件指某一寄存器或线网变量的值发生了变化。事件可用来触发声明语句或块语句的执行

**常规事件控制**：

事件控制用@来说明

posedge：上升沿，negedge：下降沿

@（posedge clock）q = d

**命名事件控制**：

可声明event（事件）变量，触发该变量，识别该事件是否已经发生

它不能保存任何值，事件的触发用符号->表示，判断用@

![Screenshot 2025-01-17 175657](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-17 175657.png)

**OR事件控制**：

由关键词“or”连接的多个事件名或者信号名组合的列表称为**敏感列表**

![Screenshot 2025-01-18 084845](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-18 084845.png)

且“or”也可以用“，”代替



**@*的用法**：@（*）可把所有输入变量都自动包括进敏感列表



### 电平敏感的时序控制

用wait来表示等待电平敏感的条件为真

always

​	wait （count_enable）#20 count = count + 1；

仿真器连续监视count_enable的值，如果值为0，则不执行后面的语句

## 条件语句

if，使用begin和end将他们组成一个块语句

## 多路分支语句

case，按顺序将它与各个选项进行比较，如果等于第一个，就执行第一个的内容

![Screenshot 2025-01-18 092043](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-18 092043.png)

**带x和z的**：![Screenshot 2025-01-18 092650](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-18 092650.png)

**casez**语句将条件表达式或选项表达式中的z作为无关值，所有值为z的位也可以用“？”代表

**casex**语句将条件表达式或候选项表达式中的x作为无关值

![Screenshot 2025-01-18 093058](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-18 093058.png)

如果输入encoding的值为4‘b10xz，则执行next_state = 3

## 循环

### while循环

用begin和end

### for循环

begin和end

### repeat循环

执行固定次数的循环，循环的次数必须是一个常量，一个变量或者一个信号

且如果重复次数是变量或者信号，循环次数是循环开始执行时的变量或信号的值

### forever循环

表示永久循环，其中不包括任何条件表达式，只执行无限的循环，直到遇到系统任务$finish为止，等价与while（1），如果要退出，可以使用disable

通常情况下，forever循环是和时序控制结构结合使用的

## 顺序块与并行块

**顺序块**：一条接着一条，

**并行块**：由关键字fork与join声明

特点：

1. 并行块内的语句并发执行
2. 语句执行的顺序是由各自语句中的延迟或事件控制决定的
3. 语句中的延迟或事件控制是相对块语句开始执行的时刻而言的

![Screenshot 2025-01-18 100350](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-01-18 100350.png)
