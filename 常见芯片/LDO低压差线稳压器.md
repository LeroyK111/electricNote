# LDO性能参数

LDO是用于电压调节的最老和最常用器件，几乎任何一个电路设计中，都可能会使用LDO（低压差线性稳压器）这个器件。虽然LDO稳压器通常是任何给定系统中成本最低的元件之一，但从成本/效益角度来说，它往往是最有价值的元件之一。  

对大多数应用来说，产品Datasheet中的基本参数规格通常都清晰明了。但遗憾的是，Datasheet并不会列出针对每种可能的电路条件的参数，许多主要性能参数并未得到人们的充分理解或至少未被最大限度地加以利用。因此，为了充分发挥LDO优势，工程师必须深入理解关键性能参数及其对特定负载的影响。

## **第一部分：PMOS和NMOS两种主流架构**
低压差稳压器（LDO）是一种广泛应用的稳压解决方案，能够在输入输出电压差较小的条件下将较高的输入电压转化为稳定的低电压输出。作为一种线性稳压器，LDO通过调节通道晶体管实现电压降的优化，类似于自动调节阻值的电子控制可变电阻，以确保在特定输出电流下稳定的输出电压。

LDO提供了一种简单且高效的稳压方式，可在提供恒定负载电流的情况下生成稳固的输出电压。其设计结构对外部元件的依赖性很低，有助于降低系统复杂性与物料成本。

典型LDO结构仅需少量外部组件，通常包括用于输入解耦的电容与稳定输出的电容，而在某些拓扑结构下，还可加入保护二极管，以应对反向电压情况。这种简洁、高效的设计使LDO成为多种应用中稳定电压供应的理想选择。总结起来，LDO包含四个基本功能元件：

- 参考电压源Reference Voltage Source：提供稳定的参考电压，用于与输出电压进行比较；
    
- 反馈网络Feedback Network：从输出电压取样并反馈到误差放大器
    
- 误差放大器Error Amplifier：比较输出电压与参考电压，并根据差值调节控制元件。
    
- 控制元件Control Element：通常是一个功率晶体管（如MOSFET或BJT），调节输入电压，以维持稳定的输出电压。目前主流的有PMOS和NMOS两种。
![](../readme.assets/Pasted%20image%2020241107164225.png)

LDO 稳压器设计通常包含四种不同的通路元件：基于NPN型晶体管的稳压器、基于PNP 型晶体管的稳压器、基于N通道MOSFET的稳压器和基于P通道MOSFET的稳压器。虽然说NPN、PNP晶体管型稳压器压差比MOSFET型更高，但是MOSFET静态电流（Iq）明显低于晶体管稳压器。
![](../readme.assets/Pasted%20image%2020241107164245.png)
所以，综上，PMOS和NMOS两种LDO是现如今的主流。
![](../readme.assets/Pasted%20image%2020241107164301.png)
![](../readme.assets/Pasted%20image%2020241107164320.png)
LDO通常易于设计和使用，但在现代应用中，系统往往包含多个模拟和数字模块，因此选择适合的LDO需要根据系统特性和工作条件综合考量。

## 第二部分：LDO主要性能参数
其实看清楚LDO的本质，可以用一个比喻来理解：想象一只鸭子穿越池塘，水面上看似平静无波，然而水下的双脚却在不停地用力划动。鸭子越用力，越快耗尽体力，不得不停下。电子电路也类似，当稳压器更费力地维持稳定电压时，能耗也随之增加。因此，关键在于选择一种能高效利用电力、保持稳定的稳压器，确保性能始终如一。

判别LDO性能的参数很多，想要用好LDO，必须清晰每个性能参数的意思。

### **压差**

压差是指在输入电压进一步下降，导致LDO不再能进行调节时的输入至输出电压的差值。在此压差条件下，通路元件工作于线性区，类似于一个电阻，其阻值等于漏极到源极的导通电阻RDS（ON）。如今LDO稳压器通常使用PMOS或NMOS FET作为通路元件，依据负载条件，可实现30mV～500mV压差。压差公式为：

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLF7icE61eMIeXHc0zwv95wicqo983wicccc2ELKesficiamIHXiaYx567fut6g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

其中，RDS（ON）包含通路元件的电阻、片内互连电阻、引脚电阻和线焊电阻，并可通过LDO 的压差估算。在压差模式下，可变电阻接近零，LDO无法调节输出电压，输入电压及负载调整率、精度、PSRR 和噪声等参数也不再具备意义。

早期的LDO设计提供大约1.3 V的压差，这意味着对于5 V的输入电压，器件进行调节可实现的最大输出仅为3.7 V左右。然而，在当今更复杂的设计技术和晶圆制造工艺条件下，'低'大致定义为<100mV到300mV左右。

### **裕量电压**

裕量电压是LDO满足其规格要求所需的输入与输出电压差，通常约为400 mV～500 mV，但某些LDO可能需要高达1.5 V的裕量电压。数据手册通常将裕量电压作为指定其他参数的条件。需要注意的是，裕量电压不应与压差混淆，只有在LDO处于压差模式时，这两者才会相等。

### **静态电流**

静态电流（Iq）是模拟比较常见的一个概念，顾名思义，它指的是是系统处于待机模式且在轻载或空载条件下所消耗的电流。静态电流适用于大多数集成电路 (IC) 设计，其中放大器、升降压转换器和低压降稳压器 (LDO) 都会影响消耗的静态电流量。当LDO完全运行时，采用以下公式进行计算：

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFJbu7yqtvyt8mFaJ9CpRagEGJfzIJXgLhHWY3ib3qK9f3qmgI5Jusx1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

静态电流看似好像费不了多少电，但对于智能手表或者手机等长期处在待机模式的产品来说，LDO静态电流越小，电池寿命越长。同时，要优化LDO的效率，需尽量降低静态电流以及输入与输出电压的差值。由于输入与输出电压差直接影响效率和功耗，通常选择最低的压差。

### **接地电流**

接地电流 (IGND) 是输入电流与输出电流之差，包含静态电流。低接地电流可以最大化 提高LDO的效率。

对于高性能CMOS LDO，接地电流通常不到负载电流的1%。随着负载电流增加，接地电流也会增大，因为PMOS调节元件的栅极驱动需增强以补偿RON引起的压降。在压差区域内，当驱动级进入饱和时，接地电流也随之增加。对于需要低功耗或小偏置电流的应用，CMOS LDO是理想选择。

### **关断电流**

关断电流指输出禁用时LDO 消耗的输入电流。参考电路和误差放大器在关断模式下都不上电。较高的漏电流会导致关断电流随温度升高而增加。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFZPVwGYV5ribp2roNc0Mia5VBfTJXlibM25Godv4eNkz1ZjG7Az4gnnszA/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

### **效率**

LDO 的效率由接地电流和输入/输出电压确定。若需获得较高的效率，必须最大程度地降低裕量电压和接地电流。此外，还必须最大程度缩小输入和输出之间的电压差。输入至输出电压差是确定效率的内在因素，与负载条件无关。例如，采5 V电源供电时，3.3 V LDO的效率从不会超过66%，但当输入电压降至3.6 V时，其效率将增加到最高91.7%。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFqmU16xHGgAV9TqibSGoe2KictWhJZcXZ2KWkJhFFibfbVFia6LZcoicEWHw/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)


### **噪声**

LDO为电子器件，所以一定会产生噪声。可以使用两个方法查看这些噪声：跨频率查看噪声和查看积分值形式的噪声。要生成不会影响系统性能的干净电源轨，选择低噪声LDO并采取降噪措施非常重要。由于闭环传递函数对抑制参考电压噪声效果有限，多数低噪声LDO需要额外的滤波器来阻挡噪声进入闭环。

除了选择具有低噪声特性的LDO，还可以应用一些技术来确保LDO 具有最低的噪声特性。这些技术涉及到降噪电容器和前馈电容器的使用，或者通过额外的引脚和小电容器来优化输出噪声和PSRR性能，滤除带隙上的噪声。
![](../readme.assets/Pasted%20image%2020241107164759.png)

### **电源抑制比(PSRR)**

PSRR是常见的技术参数，它规定了特定频率的交流元件从LDO输入衰减到输出的

程度。对LDO来说，100 kHz～1 MHz范围内的电源抑制非常重要。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFmiaXE5ncwLpbNkiaaWmAEkV0viaVMVqtCTRmAdEBW41IPfWPVicdbf5qFg/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

电源抑制比 (PSRR) 时常被误认为是单个静态值，但事实并非如此。频率、负载电流、LDO 裕量（输入到输出电压差）、输出电容、输入电压、温度、LDO架构设计、反馈网络和补偿都会影响到PSRR。对于实际应用来说，仅靠调整VIN - VOUT 和输出电容，就可以提高特定应用的PSRR，但影响PSRR的因素并不仅限于这两项，还包括以下参数：
![](../readme.assets/Pasted%20image%2020241107164826.png)
比较LDO的PSRR时，应确保在相同测试条件下进行，特别是考虑裕量电压和负载电流的影响。此外，PSRR应涵盖不同频率，并提供典型的工作性能曲线。输出电容对高频PSRR 有影响，较小电容的阻抗较大电容高，因此比较PSRR时，电容的类型和值必须一致，才能确保有效比较。

### **线性调整率**

线性调整率是指在给定输入电压变化下的输出电压变化。由于线性调整率还取决于通路元件的性能和闭环 DC 增益，在考虑线性调整率时常常不包括压差操作。因此，线性调整率的最小输入电压必须高于压差。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFr7hOywHEFSDfWrSvleloQACxAroHeibU7XG3UgwkmDTHhiaIJE8bcDRw/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

### **负载调整率**

负载调整率是指在给定负载变化下的输出电压变化，这里的负载变化通常是从无负载到满负载。负载调整率体现了通路元件的性能和稳压器的闭环DC增益。闭环DC增益越高，负载调整率越好。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFnNCSS7qQjmramRQGlBSF5ibnDcSAOZ9aJ0Togn133XbW303ftfqbpMQ/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

### **瞬态响应**

LDO 广泛应用于对负载调节要求较高的领域，如数字IC、DSP、FPGA和CPU等，这些应用需要LDO快速响应，以保持电压稳定。瞬态响应是LDO的关键性能参数，包含负载瞬态响应和线路瞬态响应。

负载瞬态响应是指负载电流变化时输出电压的变化，受输出电容、ESR、LDO控制环路带宽和负载电流变化速率的影响。变化速率较慢时，LDO可跟踪变化，但过快时可能导致异常行为，如过大振铃。

线路瞬态响应是指输入电压变化时输出电压的变化，受LDO控制环路带宽和输入电压变化速率的影响。输入电压变化缓慢时，可能隐藏振铃或异常行为。

瞬态响应主要取决于闭环传递函数的带宽，为了优化响应，闭环带宽应尽可能高，同时保持足够的相位余量确保系统稳定性。

以上参数在实际应用中都非常重要，不过，如果想要评估一款高性能LDO芯片主要有3个标准：一是电源对噪声的抑制比，二是LDO自身输出噪声及对负载变化的瞬态相应能力，三是在高精密传感器应用中，温度漂移是否足够小。

## 第三部分：厂商的产品情况

知道以上参数以后，我们就可以从厂商实际产品中进一步消化这些参数，毕竟厂商宣传的参数肯定是最关键的那几个。

虽然LDO不是什么高性能的IC，但LDO芯片市场竞争异常激烈。主要厂商包括TI、ADI、ST、ONsemi、Infineon、Microchip、Diodes、Rhom等。

TI的LDO产品非常丰富，可以帮助应对几乎所有的稳压器设计挑战，为敏感的模拟系统供电到延长电池寿命。解决方案包括业界首款智能AC/DC线性稳压器，以及低噪声、宽输入电压范围（VIN）、小封装尺寸和低静态电流 (Iq) 等多种功能，整体特点是低噪声、低Iq、高功率密度。TI的分类方法是电压范围，除了低噪声、低Iq、小型化等，TI还单门将汽车分为一类。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFfKC8Umy7JGLnHzFViaIF5Lde5j52XibSmAwjWTAZiaAgB6gV9aXtS4OLw/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

ST的LDO主要分为三个系列：高PSRR LDO稳压器、低 Iq LDO 稳压器、超低压差 LDO 稳压器。其产品特性为低压差、低静态电流（低 IQ）、快速瞬态响应、低噪音、良好的纹波抑制。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFRtGEtkwapefcMKT1uaFc9BvVkQ4nDX5BiaZLDqoZFL8NVGEmtF7xicgQ/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

英飞凌提供多种类型的线性稳压器 （LDO），包括高精度电压器跟随器、可变线性稳压器、超低静态电流稳压器 (LDO) 和高性能稳压器。与开关电源不同，低静态电流稳压器不能与降压或升压转换器一起使用。降压和升压转换器能够分别对电压 “降压” 或 “升压” 电压。它们不能与线性稳压器配合使用，因为输入电压必须始终大于输出电压。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFjiaXu4IeD0icHPv6x4XeEhzJjHfVyibopaHaHpKG1NxPWRzAibPYdegsFQ/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

瑞萨是低压差稳压器 (LDO) 主要供应商，提供配备世界顶尖单 LDO、双 LDO 的低功耗手持应用，具有高达 3MHz 的超高 PSRR、低输入电压范围、紧凑封装，以及超低噪音和静态电流等特点。瑞萨电子的高电流 LDO 最高支持 3A 电流，拥有业内领先的快速负载瞬态响应、全负载和全结温最密集电压输出精度和同级最低遗失电压。此外，瑞萨电子的低成本线性稳压器还可从电信和数据通信中常用的中间配电电压产生低压偏置电源。这类器件可用作启动或连续低功耗稳压器。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLFbfAacK2nSB6B5KcYOiaOjDVoZ0bJMF2vgmrjZv9MFkaUKtmrglkmYmA/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

ADI制造各种各样的高性能低压降 (LDO) 线性稳压器。这些 LDO 线性稳压器提供极低压差、快速瞬态响应和出色的线路和负载调整，并具有在有线/无线和音频系统、FPGA/DSP/µC 电源以及 RF 和仪器仪表领域的最终应用中增加性能价值的功能。我们各式俱全的 LDO 线性稳压器具有各种各样的杰出特性，可满足任何设计中需求，无论是需要低噪声、高 PSRR 还是紧凑封装。

![图片](https://mmbiz.qpic.cn/mmbiz_png/4uXLf4TOmvZGsXptel554sYMabH2dCLF5eJlicuHEGyGownDlWJ9cQzicLIxH9gVz2tFIHYvdKXlQ2WMkdGcqnIw/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

最近几年，也诞生了越来越多的精品国产LDO。比如，纳芯微、思瑞浦、矽力杰、力芯微电子、蓝箭电子、立锜科技、特瑞士、友台半导体、思旺电子等。
























