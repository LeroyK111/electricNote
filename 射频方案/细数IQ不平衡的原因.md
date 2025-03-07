IQ复调制逐渐成为在蜂窝基站、WiMAX、无线点对点 等终端应用中部署发射器信号链的首选架构。IQ复调制逐渐成为在蜂窝基站、WiMAX、无线点对点 等终端应用中部署发射器信号链的首选架构。

它通过同时使用两个正交（相差90度）的载波信号，即同相分量（I，In-phase）和正交分量（Q，Quadrature），来承载信息。这样的设计允许在相同的频谱带宽内传输更多的数据，提高频谱利用率。IQ调制常见于各种调制方式，如QPSK（四相位移键控）、16-QAM（16阶正交振幅调制）等。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250307160141.png)

在模拟调制过程中，IQ信号的增益和相位不匹配会直接影响边带抑制性能，这会导致接收器端的误差矢量幅度(EVM)增大，从而提高比特误差率(BER)。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250307160158.png)


哪些器件或者行为会导致IQ不平衡呢？

IQ不平衡的问题，实际上是无线通信系统在追求理想性能时所面临的现实阻碍。它揭示了一个深刻的矛盾：理论模型假设硬件是完美的，但实际硬件永远存在误差和限制。比如，理想的I和Q信号应该是幅度相等、相位严格正交的，但现实中的混频器、放大器等器件永远无法达到这样的精度。这种理想与现实的张力，是IQ不平衡问题的根本原因。

表格列了举了发射机的IQ不平衡来源。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250307160339.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250307160359.png)

相位不平衡影响分析：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250307160414.png)

DAC的增益误差

量化误差：与ADC一样，DAC也有量化误差，量化误差的精度就影响了幅度的偏差。    

相同器件上的I DAC和Q DAC共用相同的偏置电流电路、满量程调整电阻和基准控制放大器。这些模块中由电压和温度漂移引入的误差在I DAC和Q DAC上 彼此影响。

DAC输出相位误差

DAC输出相位误差是将相同输入信号馈入到I DAC和Q DAC时两个DAC之间的偏差。该偏差来自内部时钟路径的 不匹配以及DAC内核的不匹配。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250307160438.png)

PCB走线长度不匹配

PCB走线在高速电路板设计中作为传输线处理。其每单位 长度电感和电容决定每单位长度的传播延迟。这一延迟取 决于走线宽度、走线厚度、走线形状、走线和基准面的距 离以及板材的介电常数。在理想情况下，从DAC输出到调 制器输入的信号路径上的走线应在I通道和Q通道以及通道 内正极和负极之间保持对称。实际上，由于PCB设计规则 变化和制造限制，走线长度并不完全匹配。这些不匹配会 使一个通道内的信号与另一通道内的信号发生偏斜，导致 IQ相位误差。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250307160456.png)

I和Q通道间的走线不匹配会提高IQ相位误差。

现代高速DAC和IQ调制器能够提供出色的增益和正交精度，但系统内仍存在引起IQ增益和误差不平衡的其他因素。    

对于不平衡因素可使用DAC所提供的增益和正交校正功能可以有效改善边带抑制性能。






