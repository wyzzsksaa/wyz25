# vsrc文件夹

存放Verilog或SystemVerilog源代码

示例：

top.v：顶层模块文件，定义整个设计的接口和结构

alu.v：算数逻辑单元的实现

regfile.v：寄存器文件的实现

# csrc文件夹

存放C/C++源代码文件

示例：

main.cpp：仿真的主程序，调用Verilator生成的仿真模型

testbench.cpp：测试平台代码，用于生成测试信号bingyanz

driver.c：硬件驱动代码

# .nxdc

set_pin：

set_frequency：

bind_pin：