
嵌入式系统是物联网重要技术组成部分，随着物联网的快速发展，嵌入式终端设备对功耗和性能具有更高的要求[1-3]。在嵌入式系统应用领域中，ARM架构的处理器占据着主导地位。ARM架构存在高昂的专利和架构授权费用，且只能做标准化设计，难以针对具体应用需求来实现差异化处理[4]。而开源的RISC-V架构没有专利限制，具备极简、模块化和可定制扩展等优点，能够很好地弥补ARM架构存在的不足，RISC-V架构的处理器可以很好地应用于嵌入式领域[5-7]。 

流水线设计是处理器设计中的关键部分，流水线的结构与级数很大程度上影响处理器整体性能与功耗[8-9]。现有嵌入式微处理器一般采用顺序发射、顺序执行、顺序写回的流水线结构，功耗与性能难以同时兼顾，如ARM的Cortex-M3性能较好、功耗偏高，Cortex-M0性能较差、功耗较低[10]。流水线本质上是一种以面积换性能的手段，流水线的级数越深，吞吐率越高，则性能越高，但寄存器开销大，面积开销大，流水线上的冲突难以解决，需要根据具体的应用场景来选择级数，现代高性能处理器流水线级数越来越深，低功耗处理器流水线级数越来越浅[11]。本文主要面向嵌入式领域对小面积、低功耗、高性能处理器的需求，分析了RISC-V指令集，采用三级流水线结构，利用RISC-V架构定义的WFI休眠指令与时钟门控技术设计一款顺序发射、乱序执行、乱序写回的32位低功耗高性能RISC-V处理器，通用寄存器组的宽度是32位，寻址地址为0~(232-1)，支持RV32IM指令集架构，可以同时兼顾处理器的功耗与性能。

## **1 RISC-V 指令集分析**

RISC-V指令集具有模块化的特点，共6个指令集模块，每个模块使用一个英文字母表示，I表示基本整数指令集，为要求实现的指令集模块，M、A、F、D、C分别表示乘除法、原子、单精度浮点、双精度浮点、压缩指令集，为可选的指令集模块，在具体应用中可以使用模块化的方式进行指令集组合[12]。  

针对小面积、低功耗的嵌入式应用领域，本文实现32位基本整数指令集与乘除法指令集组合成的 RV32IM指令集，共55条指令，在设计过程中按指令执行周期进行分类，8条Load/Store指令与8条乘除法指令为多周期指令，21条整数运算指令、6条CSR指令及8条分支跳转指令为单周期指令，另外有2条存储器屏障（FENCE）指令与2条特殊指令。RISC-V指令编码格式如图1所示，低7位opcode、3位funct3与7位funct7、5位rs1和rs2、5位rd、imm编码位分别表示指令操作码、功能码、源操作数寄存器索引、目的寄存器索引、变长立即数。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426132147.png)

## **2 RISC-V处理器总体结构设计**

本文基于RISC-V指令集分析，设计32位架构的小面积、低功耗、高性能RISC-V处理器，其通用寄存器宽度为32位，寻址范围为232，指令操作数为32位，由IF（取指）、ID（译码）、EX（执行）三级流水线组成，采用顺序发射、乱序执行、乱序写回的方式，提高执行模块的运算速度，平衡各模块的运行速度，从而提高整体效率；利用RISC-V架构定义的WFI指令实现处理器休眠，在处理器休眠状态时，采用时钟门控技术对各个模块进行时钟控制，实现处理器层面低功耗。32位RISC-V处理器整体结构如图2所示，IF单元取出指令存入寄存器（IF/ID），ID单元对IF/ID中的指令进行译码、操作数读取、指令数据相关性的维护及指令发射，并将指令信息存入寄存器（ID/EX），EX单元根据ID/EX中的指令信息完成指令的执行、访存、写回。当执行WFI指令后，处理器停止执行当前指令流，进入空闲状态，Clk-ctrl（时钟控制）模块收到来自EX模块的休眠信号（core_wfi)，关闭所有模块的时钟门控（CG），进入休眠状态，节省动态功耗。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426132229.png)

## **3 RISC-V处理器单元设计**

### 3.1取指单元
取指单元微架构如图3所示，主要由PC生成、AHB总线控制、Mini-Decode、分支预测4个模块组成。AHB总线控制模块根据当前指令的PC值从指令存储器（IMEM）中取出指令，一方面将指令和相应PC值存入IF/ID寄存器，另一方面通过Mini-Decode模块进行部分译码。当前指令为分支跳转指令时，分支预测模块采用静态预测，根据译码得到的指令类型和细节进行分支预测，下条指令的PC值为分支预测跳转地址；当前指令为其他指令时，下条指令的PC值为当前PC+4。PC生成模块生成下条指令的PC值后存入PC寄存器，取指单元每个周期都会生成下一条待取指令的PC值来保证每个周期都能够快速连续不断地从指令存储器中取出指令，从而提高处理器的运行速度。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426132351.png)


### 3.2译码单元
译码单元微架构如图4所示，主要由RegFile（整数通用寄存器组）、Decode（译码）、OITF（多周期指令FIFO）、Issue（发射）4个模块组成。Decode模块根据RISC-V的编码规则对IF/ID寄存器中多周期与单周期指令进行译码，得到指令类型、操作信息等，同时通过译码出的源操作数寄存器索引从RegFile中读取指令操作数，由OITF模块进行数据相关性检查并解除数据相关性后，Issue模块根据Decode模块得到的指令类型生成相应运算所需的信息总线，将指令操作数发射到ID/EX寄存器。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426132537.png)

OITF模块本质上是一个深度为2个表项的FIFO，微架构如图5所示，每条多周期指令发射时，OITF分配一个表项存储源操作数寄存器索引与目的寄存器索引，在指令写回后移除相应表项，指令在发射前与OITF中的各个表项进行源操作数寄存器索引和目的寄存器索引的对比，检查此指令与正在执行的多周期指令之间的写后读（RAW）、写后写（WAW）等数据相关性，产生相关性则停止发射直到相关多周期指令执行完毕。多周期指令译码流程如图6所示，通过opcode与funct7将指令分组，由funct3译码出具体指令。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426132630.png)

### **3.3** **执行单元**

执行单元由ALU（算术逻辑运算）、AHB总线控制、WB（写回）、Commit（交付）4个模块组成，微架构如图7所示。ALU模块完成被派遣指令的执行，包含 Regular ALU、MDV、LSU三个功能子单元，分别进行普通指令运算（逻辑运算、加减法、移位等）、乘除法指令运算及访存地址生成，其中MDV单元的乘除法指令运算采用多周期乘除法器，降低并行化开销，进一步实现处理器功耗与性能的平衡；Commit模块判定指令是否在处理器中被执行，包含分支预测解析和异常处理两个子单元，分别在分支预测错误与接收到异常信号时发出流水线冲刷请求；AHB总线控制模块根据ALU模块生成的访存地址从 DMEM （数据存储器）中读出或写入数据；WB模块将ALU模块执行结果写回Regfile，并经过一个二路选择器前递给ALU模块来解决单周期指令之间产生 RAW 相关性造成的数据冲突，加快数据处理速度，WB包含两级写回仲裁单元（WB1与WB2)，分别用来仲裁多周期指令之间及多周期与单周期指令之间的写回顺序。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426132813.png)
执行单元采用乱序执行、乱序写回的结构，加快执行速度，提升处理器整体性能。多周期指令正在执行时，后序指令（多周期指令取指后取出的指令）完成译码后可以与多周期指令在相应功能单元（Regular ALU、LSU、MDV）中并行执行，程序流程图如图8所示。通过OITF模块判断后序指令与多周期指令间是否发生数据相关性，若不发生相关性则通过发射单元将后序指令发送到执行单元，当后序指令所需的运算单元处于空闲状态时，后序指令可以与多周期指令在相应运算单元中并行执行，ALU模块中最多可以有三条指令并行执行。当指令执行完、进行写回时，多周期指令由WB1判断指令的发射顺序，先发射的指令写回优先级更高，判定依据为OITF的读指针；单周期指令由WB2模块根据多周期指令是否执行完来决定写回顺序，多周期指令仍在执行则先写回单周期指令，多周期指令执行完毕则优先写回多周期指令，比重排序缓存（ROB）和使用物理寄存器组的乱序写回方法功耗更低。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426132838.png)


## **4 验证与分析**

### **4.1** **指令集验证**

本文使用硬件描述语言Verilog HDL将处理器结构以代码来实现，通过riscv-test（测试处理器是否符合RISC-V指令集架构定义的测试程序）中的指令集架构测试用例来对处理器进行功能验证，验证流程如图9所示。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426132936.png)

使用GCC工具链对汇编语言编写的测试程序进行编译，生成反汇编文件及Verilog中readmemh函数可读入的二进制文件，创建由Verilog编写的TestBench测试平台，通过TestBench使用二进制文件中的内容初始化指令存储器，在Synopsys的VCS环境下进行仿真验证。图10为ADDI 指令的仿真波形图，立即数为-1，操作数rs1为88，写回结果为87，ADDI指令功能正确，所有指令仿真结果表明，本文设计的处理器中RV32IM所有指令均通过测试。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426133005.png)


### **4.2** 功耗与性能分析

使用Synopsys的Design Compiler工具，采用SIMC 110 nm工艺对本文设计的处理器进行逻辑综合，并通过跑分程序（CoreMark）对处理器进行跑分测试，综合跑分测试结果与其他处理器对比如表1所列。本文设计的处理器相比ARM Cortex-M0及蜂鸟E203，功耗面积略高但性能有较大提升；相比ARM Cortex-M3性能偏低，但面积小了53%，功耗降低了0.54 mW；相较参考文献[3]性能略低，但本设计将指令分为多周期指令与单周期指令，分别在ID单元与EX单元解决数据相关性，能效比高，且采用了静态预测、多周期乘除法器、时钟门控技术，处理器整体功耗低。

表1 不同处理器功耗性能比较
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250426133103.png)
本文针对嵌入式应用需求设计了一款32位小面积、低功耗、高性能RISC-V处理器。采用顺序发射、乱序执行、乱序写回的三级流水线结构实现了处理器功耗与性能的折中，通过WFI指令与时钟门控技术设计一种处理器休眠模式，实现处理器层面低功耗。处理器在VCS环境下通过TestBench测试平台对RV32IM指令集中55条指令进行仿真验证，功能正确；采用SIMC 110 nm工艺在DC环境下对32位RISC-V处理器进行逻辑综合与跑分测试，处理器面积开销为20.5k个逻辑门，功耗为0.21 mW；指令执行速度为2.54 CoreMark/MHz。结果表明，本设计同时兼顾了处理器低功耗与高性能的需求，可以很好地应用于人工智能、物联网等领域。

