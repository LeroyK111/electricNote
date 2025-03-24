
RISC-V是加州大学伯克利分校设计推出的第五代开源指令集[1]，可以免费应用于商业或者学术领域，近年来受到国内外开源爱好者的关注和重视[2]。开源处理器Rocket项目是RISC- V 开源项目的一个典型代表，其采用台积电 40 nm 工艺的性能明显优于相同工艺下的ARM Cortex-A5[3]。本文基于开源处理器Rocket和开源项目 Si-Five Blocks，添加了Cordic（Coordinate Rotation Digital Computer）协处理和FFT(Fast Fourier Transform）独立加速器，提出了一种软硬件分离的FPGA平台验证方法。该方法具有以下优点：基于ITCM以及常用外设UART和SPI，方便易用；软硬件分离，提高了平台验证速度和利于软硬件问题定位。

## **1** **系统架构**

验证的系统架构主要由Rocket Core、ITCM、DTCM、SoC总线、BootRom、SPI、UART、FFT 独立加速器和Cordic算法协处理器等部分构成，具体结构如图1所示。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219175753.png)

内存的映射关系如表1所列。通用模块 UART、SPI 等不再详细介绍，下面主要介绍两个异构加速器模块：Cordic算法协处理以及FFT 独立加速器。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219175819.png)

### **1.1** **Cordic 算法协处理**
Cordic算法于1959年提出，主要用于计算三角函数。该算法是一种对目标值进行无线逼近的迭代算法，迭代的次数越多越接近目标值，迭代过程中仅进行除2和加减运算，因此很适合硬件电路实现[4]。cordic算法处理单元通过RoCC接口协议与CPU进行连接，译码时依据操作码判断指令是否为协处理器指令，如果是自定义指令，CPU 通过RoCC接口将指令发送给协处理器[5]。Cordic 协处理采用16次循环的反馈结构，由象限预处理单元（PRE-DEAL)、循环单元（LOOP-UNIT）以及符号后处理单元（POST-DEA L）三部分组成，具体电路结构如图2所示。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219180312.png)

图2 中FSM（Finite State Machine）为有限状态机，输入／输出信号的定义如表2所列。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219180330.png)

Cordic算法处理单元利用了 RISC- V 指令集架构灵活的可扩展性，自定义指令接口custom0（Rocket Core中，默认实现了3个自定义指令：custom0、customl、custom2[6]）完成三角函数的计算，具体使用方法如下：  

**custom0 rd，rs1，rs2，funct**  

custom0为使用Rocket Core中 RoCC的首个接口，rd 为目标寄存器，rs1和rs2为源寄存器，funct为额外的编码空间，便于实现更多的指令（funct为指令的25~31位，因此可以编码出128条指令）。三角函数指令在软件中的具体定义如下：

ROCC_INSTRUCTION(CORDIC_CUSTOM_CODE,y,x,0,SIN_FUNCT_CODE)  

ROCC_INSTRUCTION(CORDIC_CUSTOM_CODE,y,x,0,COS_FUNCT_CODE)

其中，CORDIC_CUSTOM_CODE值为0表示使用自定义指令接口custom0,x为源操作数（即输入的角度值，如30°），y为目的操作数即计算完成的三角函数值，SIN_ FUNCT_CODE和 COS_FUNCT_CODE为funct来实现sin或者cos函数的指令。  

输入三角值采用二进制数（16位）进行量化，范围为0~360°，因此可以得出1deg=65536/360。三角函数值的结果使用二进制（17位）补码表示，其中最高位为符号位，低16位为函数值大小。表3举例给出了输入三角值和期望的函数计算结果。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219180354.png)


### **1.2** **FFT 独立加速器**
利用计算机高效、快速计算离散傅里叶变换的方法，统称为FFT 快速傅里叶变换。FFT 于 1965 年提出，用来加速多项式的乘法，因此对于数字信号的处理具有重要意义[7]。本文FFT独立加速器模块运用按时间抽取的FFT算法，具体算法流程如图3所示。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219180949.png)

FFT 独立加速器计算过程如下：加速器通过MMIO （内存映射IO）接收到启动信号后，按照离散傅里叶变换的运算规律主动通过主设备接口从存储中读取数据到内部RAM中进行蝶形运算，然后将运算后的结果写回到内部RAM中，为下一轮计算使用。重复上述计算过程，直至完成全部轮次计算，然后以中断的形式通知CPU读取结果数据。独立加速器的结构图如图4所示。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219181008.png)


SoC总线采用 Tilelink，该总线是一个芯片级（chip-scale）的互联标准。Tilelink 允许多个master主设备，以一致性存储器映射的方式访问存储器或者其他slave 从设备，其设计目的是为SoC提供低延迟和高吞吐率传输的片上互连方式，来连接通用多处理器、协处理器、加速器等各种设备[8]。FFT计算单元是以Tilelink 总线方式挂载到Rocket Core上，分配的基地址如表1所列。操作 FFT 计算单元同其他常用外设（UART、SPI），具体操作如下：  

FFT_REG(FFT_REG_ADDR)=(uint64_t)addr;  

FFT_REG(FFT_REG_SIZE)=count_point;  

FFT_REG(FFT_REG_RUN)=action;

其中，FFT_REG_ADDR表示设备的基地址，为表1中的 FFT:0x10020000;FFT_REG_SIZE为FFT运算的点数；FFT_REG_RUN为启动信号。

## **2** **软硬件分离验证**

为满足软硬件分离验证，SoC需要的模块为BootRom、ITCM以及外设 UART 和 SPI。UART 串口功能为满足 PC串口助手同应用软件（FFT 和 Cordic）的指令和数据交互。BootRom中的启动代码负责通过SPI接口将SD卡中的可执行文件拷贝到ITCM中运行。因此，SoC中的BootRom和测试软件应用程序需特定设计。
### **2.1** **BootRom 设计**

BootRom主要完成一些基本配置，包括设置全局指针、堆栈指针，初始化BSS段等。此外，还需要添加SPI底层驱动和SD卡操作协议来支持将外部的可执行程序拷贝到ITCM中。主要应用的SD卡操作协议指令如表4所列。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219181200.png)

进行拷贝（应用程序由SD卡拷贝到ITCM）的伪代码如下：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219181352.png)
其中，SPI_REG_ADDR为表1中的SPI设备基地址0x1001 4000,RXFIFO为接收数据寄存器的偏移地址，*p为拷贝目的地址（即ITCM起始地址0x800 0000），最后在启动代码中添加跳转指令将PC指针指向ITCM的起始地址，具体指令如下：  

li s1,0x8000000  

jr sl  

将启动代码、SPI底层驱动、SD卡操作协议等软件以硬编码的形式固化到 BootRom 中。BootRom与Rocket Core、Cordic协处理器单元、FFT独立加速器一起编译生成SoC网表文件，供FPGA平台验证使用。

### **2.2** **Cordic和FFT软件测试程序设计**


Cordic算法处理单元为协处理器，软件中需要添加协处理指令来对该加速器进行测试；FFT为独立加速器作为设备挂载到Tilelink总线，因此需要在测试软件中通过操作寄存器来对FFT单元进行测试。Cordic和FFT加速器模块具体的软件测试指令上文已给出，这里不再赘述。  

由于测试软件从SD卡中拷贝到ITCM中运行，因此需要在链接脚本和启动文件中添加必要的支持，其中链接脚本中要添加ITCM和DTCM对应的物理地址，启动时将程序的数据段拷贝到DTCM，具体添加脚本代码如下：  

```
ITCM (rxai! w):ORIGIN=0x80000000,LENGTH=64k 

DTCM(wxa! ri):ORIGIN=0x40000000,LENGTH=64k
```

选择RISC-V对应的工具链编译软件测试程序，然后将生成的可执行二进制bin文件通过 dd 工具烧录到 SD 卡中。


### **2.3** **VC707 开发板平台验证**
测试程序通过串口UART获取上位机（PC机中的串口助手）发送过来的指令或者数据。当处理器接收到指令并译码之后，判断这条指令是否为自定义指令，若为协处理器指令便将指令发送给Cordic算法处理单元，当完成计算之后，将结果再通过串口反馈到上位机中。FFT 独立加速器则是计算完成后发送中断给 CPU，然后发送结果数据到UART接口上。平台验证如图5所示。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219181456.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219181505.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219181650.png)

图6给出了对Cordic协处理器的验证，对比表3中的结果不难看出计算输出结果的正确性；图7给出了对FFT 独立加速器的验证，当输入为6时，加速器完成了2的6次方（即64点）的快速傅里叶变换。上述结果表明：软硬件分离后（BootRom仅固化通用性代码，即将SD卡的可执行二进制文件拷贝到ITCM中，加速器软件测试程序则单独编译后存储于SD卡中）可实现对异构SoC加速器的平台验证工作。



