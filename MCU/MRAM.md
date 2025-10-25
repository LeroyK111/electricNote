# 被“吹爆”的MRAM，走向MCU

由于嵌入式闪存（eFlash）在28nm触达极限，限制了MCU制程进一步向前发展，厂商纷纷瞄准了新型存储，包括磁存储器（MRAM/STT-MRAM/SOT-MRAM）、相变存储器（PCM/PCRAM）、阻变存储器（RRAM/ReRAM）和铁电存储器（FRAM/FeRAM）。

而在其中，MRAM一直被业界所追捧，华为、台积电、三星、英特尔、新思科技等巨头都曾经耕耘过MRAM。无独有偶，两年前来自Coughlin Associates 的Tom Coughlin和来自Objective Analysis的Jim Handy在一份报告中盛赞MRAM并看好其前景，他们给出的理由是MRAM类型丰富，应用前景广阔，综合优势明显。

早在2018年，业界曾流传过这样一句话：“MRAM进驻MCU，28nm下将无闪存？”这两年，MRAM不负众望，厂商一个接一个地发布带有MRAM的MCU。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025131634.png)



对于MCU来说，嵌入式闪存（eFlash）存储器是一种常用的内置存储器 （NVM）。由于片上内存容量增加和eFlash达到28nm以下的极限，这种内存空间正在迅速变化。

为了让MCU继续突破制程，同时加快MCU的NVM的传输速度，大厂纷纷选择不同路线来突破MCU制程。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025131818.png)

英飞凌（Infineon）选择阻变存储器RRAM，推出台积电28nm工艺的AURIX TC4x系列MCU。

意法半导体（ST）选择相变存储器PCM，推出三星28nm FD-SOI ePCM工艺的xMemory Stellar系列MCU，同时推动升级至18nm FD-SOI工艺。

德州仪器（TI）选择铁电存储器FRAM，其MSP430系列便拥有FRAM的相关产品。

瑞萨（Renasas）和恩智浦（NXP）选择磁传感器MRAM，NXP推出了全球首款区域控制器16nm FinFET+MRAM MCU S32K5，瑞萨则针对MRAM推出了台积电22nm ULL工艺的RA8P1、RA8T2、RA8M2、RA8D2。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025131952.png)



并不存在孰强孰弱的问题，毕竟没有任何一种存储技术是“六边形战士，每种新型存储都有其优势之处。

不过，从磁性软盘、磁带时代，磁存储器就已经渗入了我们的生活，MRAM更像一个“全能手”，什么都能做一些，所以也就被大家所看好了。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025132027.png)

## 看懂eMRAM

MRAM有多强？它有着介于SRAM和DRAM两种易失性存储技术之间的速度和面积，同时拥有读写次数无限、写入速度快、功耗低、面积小、泄露小、容量高、抗辐射性和逻辑芯片整合度高的特点。目前MRAM实验室耐温可达-40℃~150℃，覆盖了车规芯片的-40℃~120℃。

从原理来看，与传统的RAM不同，MRAM不以电荷或电流存储数据，而是由自旋电子特性由铁磁性和非磁性材料组成，形成磁隧道结MTJ (Magnetic tunnel junction）。即使断开电源，MTJ也能保持极化，保留存储的数据。现在，MTJ存在各种各样的结构，这正是MRAM的复杂所在。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025132107.png)


MRAM分为三代：第一代为MRAM，叫做磁场驱动型MRAM；第二代为STT-RAM（自旋转移扭矩），通过通入垂直于隧道结的电流使得磁矩发生翻转；第三代MRAM技术分为两种，分别为通过在重金属层中通入面内电流使得磁矩发生翻转的叫自旋轨道矩MRAM（SOT-MRAM）和通过施加电压改变磁各向异性使得磁矩发生翻转的叫做压控磁各向异性MRAM（VCMA-MRAM或MeRAM）。

目前第二代STT-MRAM占据主导地位。STT-MRAM在速度、面积、写入次数和功耗方面能够达到较好的折中，当今最普通的STT-MRAM存储器内部组合方式是1T-1MTJ（one transistor,one magnetic tunnel junction)，1T-1MTJ拥有面积小，制造成本低和与CMOS工艺融合性好等优点。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025132135.png)
而在最近，台积电也攻克了第三代SOT-MRAM产业化的关键问题：尽管SOT-MRAM的理论优势明显，但要实现产业化应用，必须解决一个关键技术瓶颈：自旋轨道耦合材料的热稳定性问题。研究团队的突破性方案是：在钨层中插入超薄钴层，形成复合结构。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025132224.png)
那么将eMRAM（嵌入式磁存储器）应用到MCU中，有什么优势？

根据新思科技介绍，与PCRAM和ReRAM相比，eMRAM具有更低的温度灵敏度，提供更好的生产级良率，并提供更长的耐用性（在多年的多个读/写周期中保留数据）。它允许字级擦除和程序作，使其成为节能的NVM解决方案。

尽管eMRAM的制造成本高于ReRAM，但其更高的可靠性和更低的可变性导致了面积高效和稳健的设计，从而抵消了较高的晶圆成本。单个芯片可以通过eMRAM拥有更多内存，或者使用eMRAM的设计可以在相同数量的内存下更小、更节能。eMRAM已经在22nm的领先代工厂投入生产，现在转向FinFET节点。

此外，MRAM在极端环境下依然能保持出色的可靠性，并展现出卓越的功耗、性能与面积（PPA）综合指标。MRAM最初为满足航空航天领域的严苛要求而研发，其通过可调磁层设计实现了存储密度的最大化。得益于其出众的功耗控制及性能表现，MRAM成为追求极致可靠性和数据完整性的应用的理想之选。

在现代交通工具中，MRAM应用尤为突出，如支持空中下载（OTA）软件更新的智能汽车。此外，MRAM还有助于在不牺牲性能的前提下，降低高级微控制器（MCU）和AI加速器的能耗。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025132326.png)
当然，MRAM也并非没有缺点，它还面临很多的挑战，比如真实器件材料体系复杂、开关比低，CMOS工艺要完全匹配等，再比如动态功耗、能量延迟效率和可靠性方面的瓶颈。

此外，MRAM对强磁场较为敏感，这在一定程度上限制了其应用范围。因此，在特定环境下，可能需要通过实施物理隔离措施或采用屏蔽技术来降低潜在风险。

虽然eMRAM具有吸引人的优势，但设计人员应使用可靠、经过硅验证的解决方案，并无缝集成内置自检（BIST）和错误代码纠正（ECC）支持。

MCU设计人员在将解决方案集成到SoC中时必须考虑eMRAM磁抗扰度，包括测试MRAM的灵敏度水平，以高斯（B场）或奥斯特（H场）报告，并将此规格告知其客户。芯片附近任何可能变得磁性的元件（例如电感线圈）都会影响eMRAM性能，因此系统设计人员必须通过使这些元件与eMRAM保持足够的距离来防止受到磁场干扰。

## 瑞萨和恩智浦疯狂加码

恩智浦这几年对MCU制程迭代的“执念很深”，因为随着软件定义汽车（SDV）大行其道，市场对于区域控制器的需求越来越大。今年3月恩智浦推出的S32K5就是一个很典型的应用MRAM的案例。

S32K5配置非常强大，兼具高性能、低功耗的特点，可集成多个不同ECU到单一系统或模块，优化成本与性能。其亮点有三：第一，强大的异构计算能力，通过单核、多核或锁步内核配置的Cortex-M7@200MHz和Cortex R52@800MHz内核、DSP、eIQ NPU，不仅可以加速AI/ML，还可通过低功耗子系统延长电池寿、通过16FF技术和高效设计实现被动冷却；第二，创新的“核心到引脚”（core-to-pin）资源隔离，将系统资源安排到隔离环境中，如果安全层面出现问题可以将核重启，该功能适用于内存、TMA通道、外设以及输入输出（IO）；第三，支持最高2.5Gbps的10BaseT1S以太网加速和CAN加速，大幅度增强了SK32K5的确定性、时间敏感网络（TSN）。

S32K5为首个16nm FinFET+ MRAM的汽车MCU，并且MRAM容量高达41MB。对于16nm FinFET，恩智浦半导体资深副总裁兼汽车微控制器总经理Manuel Alves认为16nm是当下区域控制器的理想之选，因为集中化趋势对算力和存储的需求非常大；对于MRAM，Manuel Alves认为这种新型存储介质具备独特的优势，一是写入和编程速度极快，比闪存快10倍，可快速运行，二是耐久性强，能实现100万次写入，不仅可存代码，还能用于数据存储，灵活性高，便于数据收集和跨区域存储。

S32K5作为一款区域控制器，可在两种不同场景下：一是集成所有实时控制功能，中央处理器仅负责应用运行，此为区域控制器的极端情况；二是中央计算集成整体功能，具备强大实时计算能力，区域控制器轻量化，成为连接终端节点与中央计算的接口，即区域聚合器或网关。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025132410.png)

恩智浦不同，瑞萨的视角则更偏向于边缘AI一些。不光是用MRAM那么简单，而是疯狂堆料、堆算力，让自己的MCU拥有极致的性能，极致的跑分。

今年6月，瑞萨就在官网悄然上线了“全球最强MCU”RA8P1系列MCU。其采用22nm ULL 工艺制造，搭载0.5/1MB MRAM（可选 4/8MB 闪存），对比起来，RT1170采用的则是28nm FD - SOI制程工艺。根据瑞萨的说法，相较于闪存，MRAM具备更快的写入速度、更高的耐用性和更强的数据保持能力。

自2023年推出全球首款Cortex-M85 MCU后，这款MCU不光用上了最强的Arm M核1GHz Cortex-M85，同时还集成了Ethos-U55 NPU，属于是把堆料做到极致了。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025132530.png)

瑞萨彼时也同步推出了RA8D2，这款产品同样是搭载了MRAM。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025132642.png)

今年10月，瑞萨继续用惊人的速度迭代产品，推出1GHz RA8T2 Cortex-M85微控制器，集成MRAM与EtherCAT赋能工业电机控制微控制器。该产品集成1MB MRAM、2MB带 ECC校验的SRAM，同时为双内核分别配置256KB（M85）和 128KB（M33）紧耦合内存（TCM），并支持SiP封装扩展至8MB外部闪存。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025132942.png)

紧接着，瑞萨电子超高算力RA8系列新增两款MCU产品，搭载1GHz双核7300 CoreMark跑分及嵌入式MRAM技术——RA8M2和RA8D2微控制器（MCU）产品。

瑞萨表示，RA8M2和RA8D2搭载嵌入式MRAM，相较闪存技术具备多重优势——高耐用性与更强的数据保持能力、更快的写入速度、无需擦除操作、支持字节寻址，同时具备更低的漏电流和制造成本。对于要求更高的应用，还提供单个封装中带有4或8MB外部闪存的SIP选项。此外，RA8M2和RA8D2两款MCU均包含千兆以太网接口和双端口TSN交换机，可满足工业网络应用场景的需求。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025133426.png)
可以说，最近的瑞萨不光死磕M85，同时还死磕MRAM，目前来看，瑞萨的RA8系列可谓是几乎覆盖了各个领域。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251025133748.png)










