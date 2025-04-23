
如何去搭建一个基带信号模块和链路测试模块，本篇文章演示一下如何进行基带和射频的链路仿真。
基带algorithm design设计模块不能支持RF design直接添加器件仿真，需要将RF design的原理图生成模块构成底层原理图添加到链路仿真。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250423202354.png)
在RF design添加功放，滤波器，混频器等，设置器件的实际值模拟实际器件的真实数据。

添加RF system analysis。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250423202416.png)

右击port端口，可以观测链路的数据，常用的链路级联指标    

CGAIN Cascaded Gain

CND Channel Noise Density 

CNF Cascaded Noise Figure 

CNP Channel Noise Power

CNR Carrier to Noise Ratio 

CP Channel Power 

DCP Desired Channel Power 

ECGAIN Equation based Cascaded Gain 

ECNF Equation based Cascaded Noise Figure 

PNCP Phase Noise Channel Power 

SDRStage Dynamic Range (SIP1DB - TNP) 

SGAIN Stage Gain 

SIP1DB Stage Input 1dB Compression Point 

SNF Stage Noise Figure 

TNP Total Node Power

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250423202642.png)

可以得到每一级的数据参数表格
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250423202658.png)
也可以统计系统的功耗
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250423202720.png)

杂散列表，通过杂散列表调整滤波器的参数。也可以将每一级之间的端口阻抗调整为失配，模拟仿真结果。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250423202742.png)
完成射频链路的设计之后，可以将射频链路添加到algorithm design的RF_Link模块中。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250423202758.png)
即可对整个链路进行信号质量仿真。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250423202823.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250423202835.png)










