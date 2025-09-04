
# 一、ADC 是什么？

ADC 的英文全拼是 Analog to Digital Converter，中文为模数转换器，它可以将连续模拟输入信号转换为离散的数字信号，并以一序列 1 和 0 的形式进行传送。这些输入信号被量化为数字量后，再进行传输或进一步后续处理时，就不易受噪声干扰。

**模拟信号：连续变化的物理量所表达的信息，如温度、湿度、压力、长度、电流、电压、光强、音色等。

**数字信号：自变量和因变量都是离散的数据信息，通常容易被 MCU/DSP/CPU 进行后续处理的二进制数来表达。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250513191130.png)

从模拟到数字的变换就像从真实世界进入到像素世界，我们日常生活中常讲到的数码相机、手机上的摄像头模组内，就包含一个成像专用的 ADC，将图像中每个像素单元的模拟光强度值转换成数字量。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250513191145.png)

# 二、我们为什么需要 ADC？

现实世界中，我们被温度、湿度、光、声等物理量包围，作为有着感知能力的生物体，我们能够非常自然地获取模拟信号，并与这些物理量达成默契，但是对于 CPU、MCU 等各类电子设备来说，这些信号却很难被理解。

在数字化社会中，一切事物都被赋予了可量化的期待，对数据的读取、处理、传输和存储，成为了人类认识事物的基本逻辑。

因此，我们需要将现实世界中的模拟信号转换为机器能够理解的数字表达。现实世界和数字世界的“窗户纸”将由模数转换器（ADC）来捅破。

# 三、ADC 有哪些架构？工作原理是什么？

ADC 架构有：并行比较型（Flash），逐次逼近型（Successive Approximation Register），积分型（Integrating），增量型（Delta-Sigma），流水线型（Pipeline）等。

## 并行比较型（Flash）

下图是并行比较型 ADC 的拓扑原理图，采样输入信号和设置好的比较电平直接比较得到输出。

下图中假设有 n 个比较器，最下面的是第 1 个，满量程输入电平是 Vfsr，作为参考电压，由 n+1 个等值电阻将其均分为 n 个阶梯，那么第 X 个比较器负向输入电压为 Vfsr·X/(n+1)，如果从第 m 个比较器开始以上的比较器输出都是 0，以下的输出都是 1，那么输入信号电压为：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250513191228.png)

## 逐次逼近型（SAR）

一个 n 位分辨率的 SAR 型 ADC，第一阶段，输入信号先和设定好的比较电平输入比较器作比较，比较电平设置为 ADC 满量程的一半 Vfsr·2-1，输出第一位二进制结果 B1，将 B1 存入寄存器，第二阶段，输入比较器的比较电平根据第一次的比较结果设置为 Vfsr·2-1+(2·B1-1)Vfsr·2-2，此处的 B1 及后面公式中的 B2, B3, Bn-1, Bn 均作为十进制数参与计算，比较后输出第二位结果 B2，同样存入寄存器，进入第三阶段，比较电平设置为 Vfsr·2-1+(2·B1-1)Vfsr·2-2+(2·B2-1)Vfsr·2-3，得到第三位结果 B3，直至第 n 阶段，比较电平设置为 Vfsr·2-1+(2·B1-1)Vfsr·2-2+(2·B2-1)Vfsr·2-3+…+(2·Bn-1-1)Vfsr·2-n，得到最后一位结果 Bn，由最高位 B1 至最低位 Bn 组成的 n 位二进制数即为该 n 位 ADC 的输出结果，转化为 10 进制数 D，那输入信号的电平测量值等于 Vfsr·D·2-n。

例如下图是一个 6bit 的 SAR 型 ADC 的转化流程，输入信号先和 Vfsr/2 比较得到最高位 1，之后再和 Vfsr/2+Vfsr/4 比较得到第二位 1，继续下去，得到二进制结果 110101，根据上文的公式 Vfsr·D·2-n 得出输入电平为 53·Vfsr/64，理论误差小于 Vfsr/64。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250513191255.png)

## 积分型（Integrating）

下图是单斜率积分型 ADC 的拓扑原理图，通过积分器从 0 电平积分到达采样信号电平的时间计算得到采样电平。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250513191315.png)

采样开始时，积分器开始积分，同时计数器开始对输入的时钟信号 Clk 计数，假设该时钟频率为 f，积分电流为 Vref/R，经过时间 t 后 A 点电压超过输入信号的电压值，比较器输出从 1 跳变至 0，计数器停止计数，得到计数值 k，通过下方公式计算得到输入电压。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250513191335.png)

## 增量型（Delta-Sigma）

增量型 ADC 的拓扑原理图如下，先看积分器，如果输出小于 0，比较器输出 1，否则输出 -1，比较器输出 1 时，乘法器输出 Vref，否则输出 -Vref，所以当积分器输出大于 0 时，将有 Vin-Vref 输入到积分器中进行下一次比较，否则输入 Vin+Vref，记录每一次比较器的输出，统计输出 -1 的次数 X 和总比较次数 m，通过下方公式来计算输入电平，总的比较次数越高，分辨率越高。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250513191359.png)

## 流水线型（Pipeline）

流水线型 ADC 通常由多个相同结构的子单元组成，每个子单元包含一个 ADC，一个反向DAC，一个减法器，一个固定增益的放大器构成，子单元中的 ADC 多为 Flash 型，也有 SAR 型。

如下图，假如一个 X 阶的理想化流水线 ADC，子单元中的 ADC 的精度为 n bit，该子单元满量程为 Vfsr，假设该子单元 m 输入信号 Vin 被该子单元内 ADC 量化的结果为 Am·Vfsr，那么该单元可输出的结果最小值 Amin=0，最大值 Amax=(2­n-1)/2n，将 Vin 和该量化结果通过 DAC 转化为模拟信号后送入减法器会得到一个小于等于 Vfsr·2­-n 的差值 Vin-Am·Vfsr，该差值通过子单元内增益为 2n 的放大器放大后得到电平为 2n·(Vin-Am·Vfsr) 的模拟信号输出该单元，再作为输入进入下一级子单元 m+1，经过同样的流程得到量化结果 Am+1·Vfsr，每一级将输入信号和量化信号的差值放大后送至下一级再做量化，经过 X 阶最终会产生一个 X·n 位精度的量化结果，由以下公式计算，

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250513191426.png)

## 具体比较

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250513191445.png)

