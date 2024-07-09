# MCU的AI潜力



## M85内核MCU夺冠 

  

MLCommons在榜单表示，MLPerf Tiny基准测试套件主要针对小于100kB的“微小”神经网络的推理用例，以公平且可重复的方式进行测试，处理来自传感器（包括音频和视觉）的数据，以最小的外形尺寸为低功耗设备提供端点智能。

  

MLperf Tiny v1.2结果包括博世（Bosch）、Kai Jiang（个人）、高通（Qualcomm）、瑞萨（Renesas）、意法半导体（ST）、Skymizer和Syntiant提交的91项整体性能结果，包括18项能量测量。测试基准包括深度自动编码器（Deep Auto-encoder）、DSCNN（依赖敏感卷积神经网络）、MobileNetV1 0.25x、ResNet-V1四项。

  

榜单显示，高通骁龙Gen 3跑分在本次结果中每项基准都是第一，对TinyML来说，智能手机的确是最为典型的例子。其次，则是单板计算机RISC-V核心的Skymizer TinkerV-linux-andes_nn,RISC-V AndesCore AX45MP Single core @1.0GHz，其上核心为Andes的处理器，在深度自动编码器上性能强劲，不过，ResNet-V1处理能力处于中等水平。

  

如果只看MCU的话，瑞萨EK-RA8D1,32 Bit Arm Cortex-M85 @480MHz综合性能第一，在深度自动编码器和ResNet-V1处理能力上都很出色。同时纵观v0.5~v1.2整个榜单，这款MCU的表现都非常亮眼。瑞萨RA8系列作为全球首款Cortex-M85的MCU，依靠M85加入Arm Helium技术，对比过去的DSP+FPU，可见实力强劲。

  

紧接着，ST NUCLEO-H7A3ZI-Q,32-Bit Arm Cortex-M7 @280MHz；ST NUCLEO-L4R5ZI,32-Bit Arm Cortex-M4 @120MHz；ST NUCLEO-U575ZI-Q,Arm Cortex-M33 @160MHz位列二~四。很明显，同属ST的产品跑分，是按照M7、M4、M33排序，这本身也和Arm的分级相符了。

  

博世提交的开发板既包括英飞凌的CY8CPROTO-062-4343W，包括ST的DISCO-F746NG和NUCLEO系列，也包括瑞萨的RH850/F1KM-S4 R7F701649。这些主要就看是什么Arm内核还有DSP、FPU了。

![](../readme.assets/Pasted%20image%2020240422194602.png)
不过，整体来说，由于芯片等级落差颇大，单从推论时间（毫秒ms）及能耗（微焦耳uJ 比较可能会有点不公平，而且MCU、MPU和SoC放在一起“大乱斗”，就更不公平了，都不是一个功耗等级，所以整体还是一个参考，还是要按照实际应用来看。

## **TinyML在十年内攻下MCU**

AI以及大模型的出现，使得千行百业正在发生变化，它不仅要改变服务器端，也会改变边缘端。

数据中心功耗和负载已经发展得很可怕了，加之物联网兴起，不可能每做一次任务，就要问一次服务器怎么做，每个点的设备总归是要有自己的想法。所以TinyML就是把AI应用带到边缘设备（如智能手机、可穿戴、汽车和物联网设备等）上的关键。

AI让边缘更智能，边缘让AI无处不在，对MCU来说，TinyML就是正在发生的变革。

TinyML最大的优点就是可移植性。在具有小电池和低功耗的廉价MCU上运行意味着，使用 TinyML，人们可以很容易地将ML以便宜的价格集成到几乎任何东西中。
![](../readme.assets/Pasted%20image%2020240422194650.png)
从工作机制来看，TinyML 算法的工作机制与传统机器学习模型几乎完全相同，通常在用户计算机或云中完成模型的训练。

不过，TinyML真正发挥之处在于训练后的处理，通常称为“深度压缩”（deep compression）。
![](../readme.assets/Pasted%20image%2020240422194702.png)
TinyML主要拥有两种部署方式：一是直接部署框架，比如说谷歌的TensorFlow LiteMicro、EdgeImpulse等，第二种就是对MCU提供机器学习优化库，提供函数优化运算加速能力，比如说 Arm的CMSIS NN、ST的STM32Cube.AI等。

处理器推进方，有两股势力：一是Arm自己推进自己的Cortex-M系列，如设计出矢量扩展技术Arm Helium，推出为MCU设计的microNPU Ethos U55；二是许多厂商对开源的RISC-V进行CNN推理加速器研究，如蜂鸟E203、GAP-8、Pulpissimo等。

意法半导体微控制器和数字IC事业部总裁Remi El-Ouazzane曾在采访中表示，TinyML将在未来10年成为MCU市场的最大推动力。未来五年内，该公司5亿个MCU将运行某种形式的TinyML 或AI工作负载。

另据统计显示，世界上有超过2500亿个嵌入式设备在运行，预计每年增长20%，而在其中，目前有将近30亿台支持设备会支持TensorsFlow Lite。

厂商近几年结论，便印证上了上述结论，MCU界“六大天王”ST、NXP、Microchip、Renesas、TI、Infineon都在加大布局边缘AI：
- ST在2019年发布STM32Cube.AI工具，并在2021年收购NanoEdge AI Studio，降低边缘AI开发门槛，在今年使用NVIDIA TAO 工具套件拓展STM32边缘AI生态；
    
- NXP在2018年就推出机器学习软件eIQ®机器学习(ML)软件，并不断加大在AI/ML上的投入；
    
- Microchip在2020年就将Cartesiam（现已被ST收购）、Edge Impulse和Motion Gestures的软件和解决方案接口引入其设计环境；
    
- Renesas在2022年完成对美国从事机器学习模型开发的初创企业Reality AI（以TinyML为业务）的收购；
    
- TI最近几年推出的MCU均在边缘AI领域具有优势，包括高集成可扩展的边缘AI处理器组合；
    
- Infineon 2023年5月收购瑞典的TinyML和AutoML领域初创公司Imagimob AB。
    
尽管不断给模型增加参数量，让大模型越来越大是现在AI从业者的坚定方向，不过对机器学习算法而言，设计出内存、计算和能源效率更高的算法发展，也是一个新的趋势。

目前，TinyML仍处于起步阶段，在该方向上的专家也很少。不过，可以预见一拨新趋势，正在款款走来。


