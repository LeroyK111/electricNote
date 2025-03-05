
在科技产品系统开发应用中，不少微电子设计和半导体厂商推出的RISC-V指令集形成的软件核、SoC平台、芯片系列崭露头角。特别是RISC-V 指令集微控制处理器软件核及其器件，一改ARM系列软件核及其器件的停滞退势，以更加优异的性能快速增长。以下就RISC-V 指令集及其软件核、SoC平台、芯片系列、通用微控制处理器的开发应用展开详细阐述。

## **1 RISC-V指令集及其优势**

计算机中央处理器 CPU（Computer Processor Unit）及其计算机／微计算机控制处理器的指令集主要有两类：复杂指令集计算机 CISC（Complex Instruction Set Computer）和精简指令集计算机RISC（Reduced Instruction Set Computer）。典型实用代表，Intel的x86/64系列处理器属于CISC，ARM系列微控制处理器属于RISC。近年来崛起的第五代RISC- V及其微控制处理器件，性价比更高，大有替代ARM-RISC的趋势。  

Intel-x86/64的指令集规范是封闭的，ARM-RISC不封闭但需要授权使用，只有RISC-V开源、开放和免费，而且RISC-V具有全套开源免费的编译器、开发工具和软件集成开发环境IDE（Integrated Development Environment）。  

RISC-V指令集以模块化组织，每个模块用一个英文字母表示，必须具备基本整数指令子集I以实现完整的软件编译器，其他指令子集可选，代表有M/A/F/D/C。如某款RISC-V处理内核是RV32IMAC即代表实现了I/M/A/C指令集。部分指令集描述如表1所列。  

表1 RISC-V 部分指令集描述
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250305183304.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250305183601.png)
RISC-V的优势对其拓展应用影响深远，尤其适合对生态依赖性比较小的封闭或半封闭产品、嵌入式或新兴的物联网、包含嵌入式人工智能等应用的边缘计算领域以及需要定制化的场景。RISC-V能够为物联网行业带来显著的灵活性和成本优势，未来20年，物联网和边缘计算领域的处理器内核年出货量预计会达到万亿颗的规模。中国拥有全球最大的市场空间，RISC-V大有可为。  

RISC-V_ISA免费开源，由国际基金会共同管理与维护，但其实现核（Core）及其片上系统 SoC（System on Chip）平台、系列芯片，有开源免费的，更多则不是开源免费的，可以申报专利保护。

## 2 RISC-V_ISA核汇集
RISC-V指令集规范推出以后，很多研究机构和半导体厂商推出了不少各种RISC-V_ISA软件核，打造了典型的SoC承载芯片。其中，有32位的，也有64位的；有用于研究和教学目的开源免费软件核，也有以赢利为目的的商用软件核。  

主流 RISC-V_ISA核及其典型应用如表2所列。

表2 主流 RISC-VISA 软件核及典型应用列表
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250305183703.png)

其他RISC-V_ISA软件核及其典型应用，如表3所列。


表3 其他RISC-V_ISA 软件核及典型应用列表

|   |   |
|---|---|
|Ride Code|［日］东京工大，Verilog编写，2路超标量乱序执行，RV32IM,6级流水|
|Z-scale|［英加］不列颠哥伦比亚大学UBC,Verilog编写，单发标量，RV32IM,3级流水|
|YARVI|[RISC-V爱好者］Tommy Thorn，实现了RV321,Verilog编写|
|Sodor|［美］加州大学伯克利BSD，针对教学，32位，Chisel编写，支持5种处理器；单周期处理器、2级流水线处理器、3级流水线处理器、5级流水线处理器、可执行微码的处理器|
|RicoRV32|[RISC-V爱好者］Clifforf wolf,RV32IMC，侧重求面积和频率优化，重构了Trap/interrupt/interrupt return 简化中断控制，Verilog语言编写，以小巧闻名，可以在Xilinx7-Series FPGA上轻松实现|
|玄铁 910|［中国﹣阿里］平头哥，3发射8执行乱序架构，2.5gHz主频，7.1Coremark/MHz单核性能，16核可组合，增强计算、存储，5G、人工智能以及自动驾驶等领域应用|

常见 RISC-V_ISA 软件核及典型应用列表如表4所列。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250305183831.png)

## 3 通用RISC-V指令集微控制处理器在开发中的应用

###  **3.1 片上RISC-V应用系统开发**

网上下载或者特别购买一些商用的RISC-V_ISA软核及其常规“外设／接口”软核，就可以在通用的 FPGA平台上构造起灵活的RISC-V片上微控制处理系统，进而展开特定应用领域的微控制处理器系统开发。“外设／接口”软核，也可以是自行开发的。  

选用较多的典型免费开源RISC-V_ISA软核是赛昉SiFive的FE310和芯来Nuclei的 HE203（蜂鸟 E203），得到其 Verilog 源码后，在主流的FPGA_SoC开发板平台上（如 Artix-7 35T Arty FPGA 评估套件、Xilinx XC7A100T 评估开发板等）以相应的 IDE 工具（如 Xilinx 的Vivado开发套件、Altera的QuartusII等）做适当的代码适应调整、接口环境配置等，就可以直接转换编译和适配得到SoC架构文件，下载到FPGA开发板上，相应的微控制处理器SoC平台就形成了，这样的RISC-V微控制处理器FPGA_SoC平台就可以像通用RISC-V_ISA 微控制处理器芯片一样投入嵌入式应用系统开发使用。  

在FPGA_SoC开发板平台上，移植并具体适配FE310 或 HE203，既可通过make命令行方式，也可使用Vivado_IDE或 QuartusII_IDE的图形化用户界面GUI (Graphical User Interface）方式。GUI方式虽然易用，但需要完成相关环境设置，事实上两种方式原理是一样的：首先编译生成位流文件，进而生成具体FPGA适配文件。编译前需要做的主要工作有（特别是软核成型的原始 FPGA开发板与具体的FPGA 有差异时）:

①检查时钟和复信定义，调整变更系统文件如system. Org；  

②因地调整输入／输出（I/O）引脚，适时更换I/O缓冲，适当增加上下拉；  

③增设层次top文件，预设脏数据处理，应对综合布线布局后I/O资源增多，只引出需要的I/O；  

④检查并指定协议lience支持的脚本文件。

适配时需要做的工作：  

①检查承载的闪存Flash型号，及时因地调整；  

②指定添加必要的依赖文件，减少并消除过程出错。

在旧有 FPGA 开发板而非推荐的 FPGA 上做移植和适配时尤其需要注意上述问题，特别是非 Xillix公司的FPGA，如换用了Altera的FPGA。  

FE310或HE203已经非常成熟，有很多做好的不同FPGA适配文件可以直接下载使用，如果移植适配时有问题，可以直接拿来作对比。商用RISC-V_ISA软核，移植适配时有问题，厂家还会手把手地进行服务指导。运用FPGA进行 RISC-V片上嵌入式系统开发应用，灵活自主，已经成为了一种时尚的产品系统开发选择。


### **3.2 嵌入式RISC - V应用系统开发**

在通用RISC-V_ISA微控制处理器芯片或 FPGA_SoC平台上进行嵌入式应用系统开发，首选RISC-V_ISA 厂商提供的IDE（如Nuclei Studio、Freedom Studio等），其次可以选用一些通用的第三方IDE（如RT-Thread Studio、Arduino Studio等）。这些IDE集成有交叉编译工具链riscv-gun-toolchain、模拟调试器qemu、调试支持OpenOCD（Open On - Chip Debugger），也可以依托常用的IDE环境 Eclipse，添加 riscy - gun- toolchain、qemu、OpenOCD和用到软核相应的SDK（Software Development Kit），如 Freedom E SDK、HBird E SDK等，得到自定义的Studio_IDE。事实上，软核RISC-V_ISA厂商或第三方提供的IDE，也是经过这种自定义做起来的，它们更成熟和易于使用。  

在实际工程项目开发前，在IDE环境中，需要做的主要工作有：安装特定仿真调试器的驱动并做选定；指定工具链位置（主要是riscv -gun - toolchain和OpenOCD）；配置编译、连接选项；设置特定的板级支持包BSP(Board Support Package)；设置所用包含文件路径；设置并指定相应闪存定位链接文件。  

IDE建立好后，RISC-V_ISA 芯片或 FPGA_SoC平台上的嵌入式应用系统开发就与传统的 ARM芯片或SoC平台一样了，如RV-MDK、IAR等。RISC-V_ISA全套开源免费的编译器、开发工具和IDE，相比ARM等传统架构的编译器和IDE而言，还颇有差距，毕竟是后起之秀，又处于技术的“百家争鸣”时期，没有经过统一和优化。  

RISC-V_ISA独特的性能使得应用其微控制器芯片或FPGA_SoC平台进行嵌入式应用系统设计十分便利。以往复杂的系统控制，若选型运用得当，常常可以事半功倍。图1是德国电机精密控制厂家Trinamic 运用32 位RISC-V软核和其专有电机驱动控制软核设计的Rocinate系列risc-v_isa微控器的结构框图，除了常规应用，还可使得精密伺服控制非常易于实现，它内部既集成了输出驱动，也集成了电流检测保护，还全部置于 RISC-V内核的控制下，可以直接驱动控制伺服电机、步进电机、无刷直流电机、有刷电机、音圈电机等，实现驳接、驾驶、搬运、机器人手臂等自动控制。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250305183936.png)
图2给出了选用沁恒RISC-V3A_ISA微控制器CH56x实现快速U盘读写的流程图。CH56X芯片集成有片内外设﹣5 Gbps的通用串行总线 USB3.0和百兆的嵌入式多媒体卡 EMMC（Embedded Multi Media Card），硬件中断实现进出栈，双层128位并行直接存储器访问DMA（Direct Memory Access）架构，软硬件结合，可以实现54 Mbps以上的U盘读写速度。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250305184237.png)

RISC-V_ISA及其芯片或FPGA_SoC平台突破了相应传统科技领域的知识产权限制，是现代主流ARM微控制器软件核及其芯片替代的“不二”选择，为CPU“自主控制”和“普适通用”矛盾的有效解决指出了光明前景。了解RISC-V_ISA及其典型运用，搜集常用RISC-V_ISA_SoC平台，研究RISC-V_ISA芯片序列，熟悉现代常见通用RISC-V_ISA芯片或FPGA_SoC平台，是嵌入式应用产品系统低投入、高产出的便捷途径。RISC-V_ISA 能够为物联网行业带来显著的灵活性和成本优势，其芯片或FPGA_SoC开发应用，尽管存在着前进中的通用性、易用性方面的不少差异，但本土化应用生态优秀，加上新兴的物联网、人工智能等应用的边缘计算领域及定制化的广泛场景，一定会拥有广阔的发展空间。




