  DDS（直接数字频率合成器，Direct Digital Synthesizer）是一种基于数字信号处理技术的频率合成方法，其核心原理是通过数字计算生成高精度、高稳定性的正弦波或其他波形信号。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250330213614.png)

DDS 主要有参考时钟，相位累加器，存储器 ROM，数模转换器以及低通滤波器等 5 个模块构成.

优点

高频率分辨率和快速频率切换（ns 级）。

相位连续，可生成复杂调制信号（如 FSK、PSK）。

集成度高，易于数字化控制。

缺点

输出频率上限较低（受时钟频率限制）。

杂散信号较多（由相位截断和幅度量化误差引起）。

需配合 LPF 滤除高频噪声。

          从原理框图不难看出，DDS主要是基于参考时钟，以参考时钟为基准，对参考时钟进行相位等分,查表，DAC转换得到输出的正弦波。

假设系统时钟为 Fc，输出频率为 Fout。每次转动一个角度 360°/2N， 则可以产生一个频率为 Fc/2N的正弦波的相位递增量。那么只要选择恰当的频率控制字 M，使得 Fout / Fc= M / 2N,就可以得到所需要的输出频率 Fout，

Fout = Fc*M / 2N
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250330213811.png)


相位幅度转换

通过相位累加器，我们已经得到了合成 Fout 频率所对应的相位信息，然后相位幅度转换器把 0°~360°的相位转换成相应相位的幅度值。比如当 DDS 选择为 2V p-p 的输出时，45°对应的幅度值为 0.707V，

这个数值以二进制的形式被送入 DAC。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250330213835.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250330213855.png)



