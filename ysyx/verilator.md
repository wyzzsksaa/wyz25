# 常用参数

**--cc**：转换为C++，与之对应相反的--sc，为SystemC，--cc生成C++仿真，而--sc生成SystemC仿真

**--exe**：直接生成可执行文件，必须有testbench，可以接上-o指定生成可执行文件的路径，若不使用会生成.a的静态链接库

**--build**：自动编译生成的代码并构建可执行文件，保留.o文件，前期最好与--exe一起使用

**-j 0**：并行编译选项，指定使用多少个并行作业来加速编译过程，0表示使用所有可用核心

**--trace<-filetype>**：启用波形跟踪功能（默认生成 VCD 文件），需要执行一遍可执行文件才能得到波形文件其中<-filetype>可以不填，或者使用-fst注意：fst文件只能用gtkwave打开，digital-IDE不支持fst波形的查看其他信息请自己STFW和RTFM

**--Mdir <dir>**：指定生成文件的输出目录（默认 ./obj_dir） 

**-top-module <name>**：指定顶层模块名称（默认自动检测），一般是top.v文件，所以尖括号一般替换为top

**--binary**：等价于--main --exe --build --timing ，一般用于没有输入的模块仿真

**-main**：不驱动输入（相当于输入全接高阻态）

**--timing**：启用时序检查

-top-module <name>：指定顶层模块名称（默认自动检测）



**编译可执行文件**：

make  -C  obj_dir  -f  Valu.mk  Valu

-c：指定工程目录

-f：选择工程文件

Valu：可执行文件名称



# 头文件

<verilated_vcd_c.h> : 生成波形相关

<verilated.h> :仿真库文件

< iostream>: 可替换<stdio.h>，只是后续函数使用的输出函数上有差别



# TestBench

Verilated::ommandArgs(argc, argv)：向仿真器传入参数

Valu *dut = new Valu：初始化电路模型，创建一个对象指针（用来储存波形数据）

Verilated::gotFinish()：在Verilog模块中使用$finish来指示仿真结束时，C++测试平台可以通过调用Verilated::gotFinish()检测这一事件

eval（）：更新电路状态

## 波形图

Verilated::traceEverOn(true)：启用波形记录

dut->trace(m_trace, 5)：指定波形和深度

m_trace->open("wave.vcd")：打开波形文件

$dumpfile：指定输出文件

$dumpvars：确定哪些信号会被记录

​					$dumpvars(0, top)：记录top中的所有信号





**时钟生成**：

`reg clk:`

`initial begin`

​	`clk = 0;`

​	