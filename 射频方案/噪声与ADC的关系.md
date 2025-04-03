信号链是指一系列接收输入、将信号从一个组件传递到另一个组件并产生输出的信号调节组件。在信号链的过程中会出现一些使信号质量降低的现象，就是噪声。
![](../readme.assets/Pasted%20image%2020250403112126.png)
噪声是任何在信号链中不受欢迎的电气现象。噪声可以分为内部噪声和外部噪声。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112225.png)

噪声的指定

噪声幅度 ：半导体噪声由于随机过程导致瞬时幅度不可预测，但其幅度服从高斯（正态）分布。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112247.png)
噪声谱密度 ：描述噪声在不同频率下的能量分布。噪声频谱密度通常在特定频率（如1kHz、10kHz、100kHz和1MHz）下指定。

**噪声类型**

白噪声源 ：具有均匀的谱密度，在任何给定带宽区间内具有相等的能量。

粉红噪声源 ：谱密度随频率的倒数增加，因此也称为“1/f噪声”。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112303.png)

**阅读噪声规格**

时域规格 ：包括峰峰值（Peak-to-Peak）和均方根值（RMS）。

频域规格 ：包括特定频率点的噪声谱密度。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112319.png)



**估计噪声幅度**

创建噪声谱密度图 ：绘制噪声谱密度曲线以帮助估算噪声幅度。

计算噪声幅度 ：使用积分公式计算特定带宽内的噪声电压。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112333.png)
**白噪声**

热噪声 ：存在于所有被动电阻元件中，由电阻介质中电子的随机布朗运动引起。

散粒噪声 ：当电荷穿越势垒时产生，由通过结点的电流不平滑引起。

雪崩噪声 ：存在于反向击穿模式的PN结中，如齐纳二极管，由载流子通过物理撞击释放额外载流子引起。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112350.png)

**粉红噪声**

闪烁噪声 ：存在于所有类型的晶体管和某些类型的电阻器中，与直流电流相关，由半导体材料中的杂质引起的随机电流波动引起。

爆米花噪声 ：低频调制电流，由电荷载流子的捕获和发射引起，通常与重金属离子污染有关。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112404.png)

ADDA的噪声

除了半导体噪声外，ADDA还有其他噪声和失真来源，包括：

量化噪声 ：与分辨率、差分非线性、带宽有关。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112425.png)

采样抖动 ：在采样时间变化信号时引入噪声。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112438.png)

谐波失真 ：由通道内的非线性行为引起。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112504.png)

模拟噪声 ：有效噪声参考于ADC的输入或DAC的输出。    

**如何选择最佳的****ADDA

确定目标 ：分配信号链中的噪声以实现可接受的信噪比（SNR）。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112525.png)

选择分辨率 ：使用理想数据转换器的经验公式来确定所需的分辨率。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112543.png)

初步选择ADC ：根据所需参数选择初始ADC。

SNR= 6.02N+ 1.76dB        

计算SINAD ：使用计算器输入参数，计算SINAD。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112610.png)

检查噪声分布 ：分析各噪声源的相对水平，确定改进方向。    

减少量化噪声 ：通过选择更高分辨率的ADC来降低量化噪声。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250403112626.png)

重新检查噪声分布 ：验证改进效果。

进行噪声分布权衡 ：在不同噪声源之间进行权衡以保持SINAD不变。



