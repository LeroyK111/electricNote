
在做射频通信的时候，经常会听到用到几个名词，信噪比，解调信噪比，0.5%的误码率的解调信噪比是多少。

到底什么是信噪比SNR，什么是EB/N0，什么又是BER，相信很多人看到这三个难兄难弟都很头大，傻傻分不清楚。本文讲讲他们之间的关系是什么。

什么是信噪比SNR。

SNR即Signal-to-Noise Ratio，是信号功率与噪声功率的比值。

SNR是一个关键的性能指标，用于衡量信号质量的好坏。信噪比越高，信号质量越好，信息传输的可靠性越高。这是一个通用量，发射和接收都用它来标量系统的信号质量，可以通过频谱仪等仪器直接实际测量。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250427152441.png)

SNR=10lg(S/N)

S指信号，N是噪声。

SNR对射频工程师来说是一个非常重要的指标，它可以标定我们射频模块或者系统的失真量    

比如说星座图，星座图的失真量是可以换算成SNR

在前面的文章也讲过

EVM≈1/SNR


![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250427152503.png)

Eb/N0：Ratio of bit energy to noise power spectral density，每个二进制bit能量与噪声功率谱密度的比值.这个参数主要用于衡量数字通信系统的性能.

从上述定义看，两个都是衡量信号功率和噪声功率的比值，SNR不区分信号类别，模拟数字都可。Eb/N0特指数字的信噪比。

从计算上有什么不同？

Eb：一个比特的平均能量，N0：噪声功率谱密度。那么信号速率为RBbps/s的信号能量就为Eb*Rb，Rb所占的信号带宽是B    

SNR=(Eb/N0)*(Rb/B)

取对数就是SNR=Eb/N0+Rb/B

Rb和B之间的关系是编码效率的关系，拿升余弦滤波器来说，滚降系数为α

B=Rb*（1+α）

SNR=Eb/N0-10log（1+α）

这里计算SNR是指简单的单载波调制，对于扩频，OFDM计算需要根据情况计算。

对于接收机的灵敏度计算公式来说，

F=-174dBm/Hz+NF+10log(BW)+解调信噪比，这里的解调信噪比，对于模拟调制就是SNR，也就是常听得到的信纳德。对与数字调制这里就是Eb/N0。

也就是说，对于数字调制来说SNR并不等于Eb/N0。

那么Eb/N0怎么确定？

这里就要提到BER，BER误码率 (BER: Bit Error Rate)，误码率是衡量一段时间内信息有没有错误的单位。BER通常不是单个出现的，它的出现一般都伴随着Eb/N0。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250427152533.png)


通常我们在说一个灵敏度的时候都对应着一个指标，-116dBm@BER≤0.5%这样。

也就是灵敏度并不是要求完全没有错误的情况下的指标，而是一个能容忍多少误码情况下的指标，即此时对应的Eb/N0。

从上面的仿真图也可以看出，不同的BER对应着不同的Eb/N0。

以前也有朋友问我，接收机的灵敏度的解调信噪比怎么计算和确定，解调信噪比的确定最终是与BER的指标对应。

误码率与Eb/N0的关系如下，pB指误码率，M指调制阶数，n是比特数

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250427152555.png)




