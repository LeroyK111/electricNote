
接收机的主要作用是把搭载在载波上的信号尽可能的无失真，无干扰的提取出来，不管是什么架构，最终一步都需要经过ADC进行模拟信号数字化提取。

而射频架构的区别在于,ADC拿一个什么样信号在处理。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250302223133.png)

超外差架构，给ADC提供了一个频率低，干扰少，动态范围大的信号。

零中频架构，给ADC提供一个带宽窄，杂散少的信号。

直接采样架构，ADC从射频端直接采样，ADC得到一个高带宽，低延迟的信号。

从设计处理的角度来说，接收机射频前端为ADC服务。设计接收机在于ADC想得到一个什么样的信号或者说想处理一个什么样能力的信号。其他的都是属于ADC采样后算法能力范围内的折中。

说白了就是ADC采样后的能力够了，射频前端怎么省功耗，怎么省成本怎么来。ADC的能力不够，前端补齐。

1.接收机的噪声系数和ADC的噪声系数

接收机的灵敏度计算公式如下

![](../readme.assets/Pasted%20image%2020250302223230.png)

一个接收机定下了接收灵敏度，也就定下了接收机的最小噪声系数是多少

那么ADC的噪声系数对接收机的噪声系数有什么影响呢？

从噪声系数的角度来理解，首先ADC的噪声系数可以有以下推导过程

假设ADC的输入的最大信号为v(t) = V0 sin 2πft

那么满量程功率为P=（V0 /√2）2/R=V02/2R

那么功率用dBm表示即为P（dBm）=10log（V02/2R）+30

计算ADC的噪声需要利用ADC的SNR来计算，可以得到ADC噪声电平

SNR=20log（VFS/Vnoise）那么Vnoise=Vfs*10-SNR/20

噪声因子F等于信号的功率除以噪声的N=KBT

F=(Vnoise)2/KBTR=（VFS*10-SNR/20）2/KBTR=(VFS2/R)*10-SNR/10*(1/KBT)    

NF=10log（F）=PFS-SNR+174-10logB
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250302223243.png)
假设一个16为的ADC，它的SNR为84dB，输入的电压的峰峰值为3V，采样率为80Mbps，按照采样带宽遵循第一奈奎斯特准则，带宽为40MHz

那么这个ADC的NF=10log(1.52/100)+30-84+174-10log(40*10^6)

=13.5-84+174-66=37.5dB

一般情况下，假设一个接收机的NF是4dB，从接收机噪声系数的角度来设计，假设射频前端的噪声系数是3.5，那么ADC折合到系统中的噪声系数贡献只能有0.5dB
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250302223302.png)

根据噪声系数级联公式，通过仿真可知，射频前端的增益至少要40dB，才能满足系统噪声系数小于4dB的要求。

所以一个接收机系统的最小增益从ADC噪声系数的角度来计算取决于ADC的噪声系数和接收机的灵敏度。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250302223343.png)


当然如果从ADC的动态范围的角度也可以计算出最小增益是多少，这里我们就不展开讲了。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250302223458.png)
上文根据ADC的噪声系数可以推出接收机的最小增益

但是根据IIP3的级联公式可知，增益越大，IIP3越小，IIP3越小意味着接收机容易产生互调干扰，最终影响ADC的动态范围能力。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250302223519.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250302223543.png)


