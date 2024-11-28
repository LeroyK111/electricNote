# ADC是如何影响EVM指标的

EVM是量化系统中所有信号综合损害的简单指标。采用数字调制的器件经常定义这个指标，可通过同相(I)和正交(Q)矢量图（也称为星座图）来表示。
![](../readme.assets/Pasted%20image%2020241128231016.png)

一般来说，计算EVM的方式是针对每个接收信号找到理想星座位置。通过计算接收信号的位置与其最接近的理想星座位置之间的所有误差矢量幅度的均方根(rms)，可得出器件的EVM值。

EVM与给定系统的误码率(BER)密切相关。当接收信号远离目标星座点时，它们落入另一星座点判定边界内的概率会随之而增加。这会使BER变大。进而造成丢包误码的情况出现。

说到对EVM的影响，工程师第一个想到的就是功放。

那么ADC对EVM有没有影响呢？有影响是怎么产生的？

要了解ADC对EVM的影响，需要先知道他们之间的工作原理和联系。
![](../readme.assets/Pasted%20image%2020241128231028.png)信号经过前端射频链路的放大、抑制、变频，然后送给ADC采样，给ADC的是一个模拟量，ADC输出的是一个数字量。

ADC把模拟量变为数字量的过程中会出现采样误差。
![](../readme.assets/Pasted%20image%2020241128231040.png)

## 静态指标

静态指标主要反映ADC在静止状态下的性能，常见的静态指标包括：

分辨率：表示ADC能够分辨的最小电压变化，通常以位数（如8位、10位、12位等）表示。分辨率越高，ADC能表示的电压级别越多，量化噪声越小。

失调误差（Offset Error）：指ADC输出的数字值与实际输入信号的偏差。

增益误差（Gain Error）：表示ADC输出的增益与理想增益之间的差异。

微分非线性（DNL）：描述相邻输出码之间的实际电压差与理想电压差（1 LSB）之间的偏差。DNL越接近0，表示线性度越好。    

积分非线性（INL）：描述ADC输出的实际量化值与理想量化值之间的最大偏差。INL是DNL的累加。

单调性：确保ADC的输出随着输入信号的增加而单调增加，避免出现丢码现象。
![](../readme.assets/Pasted%20image%2020241128231106.png)
总结一句话就是：模拟转数字的过程中由于位数有限的原因产生了误差。

上述的静态指标误差会影响动态指标，最终影响了ADC的动态范围。

## 动态指标

动态指标描述的是ADC性能随着信号频率变化而变化的特征，包括：

信噪比（SNR, Signal-to-Noise Ratio）：信号功率与噪声功率的比，或者说信号均方根电压与噪声均方根电压的比值。

总谐波失真（THD, Total Harmonic Distortion）：信号的均方根值与谐波的平方和根的平均值之比（通常算高5个谐波）。

信噪失真比（SNDR, Signal-to-Noise-and-Distortion Ratio）或SINAD（Signal-to-Noise and Distortion Ratio）：信号+噪声+谐波的功率与谐波+噪声的功率比值。SINAD很好地反映了ADC的整体动态性能，因为它包括所有构成噪声和失真的成分。

无杂散动态范围（SFDR, Spurious-free Dynamic Range）：信号的均方根值与最差杂散信号的均方根值的比值，无论它在频谱的哪个位置。    

有效位数（ENOB, Effective Number Of Bits）：综合考虑了ADC的非理想特性后，实际能够提供的位数。

THD+N（Total Harmonic Distortion plus Noise）：信号的均方根值与谐波的平方和根加上所有噪声分量（不包括直流电流）的平均值的比值。

SNR就是SNR=6.02N+1.76

需要明白SNR与SINDA之间的关系，SINAD所有构成噪声和失真的成分，所以SINDA比SNR差

ENOB=SINDA-1.76/6.02

这也就是我们常说的有效位数和实际位数之间的关系
![](../readme.assets/Pasted%20image%2020241128231128.png)
在EVM的计算公式中EVM和SNR之间可以相互转换。在某些情况下，

SNR≈-20log10(EVM)

ADC除了有效位数的SNR，同时ADC是一个时钟输入参考器件，会引入类似相位噪声一样的噪声-称之为ADC的抖动

采样时钟抖动 (Tj) 是由时钟源 (Tjclk) 和内部 ADC 孔径抖动 (Tjapt) 产生的抖动的组合：
![](../readme.assets/Pasted%20image%2020241128231140.png)
如果参考时钟的相位噪声小于SNR，那么

EVM≈10^(-SNR/20)
![](../readme.assets/Pasted%20image%2020241128231154.png)
![](../readme.assets/Pasted%20image%2020241128231209.png)

