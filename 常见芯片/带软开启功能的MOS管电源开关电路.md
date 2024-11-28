# 带软开启功能的MOS管电源开关电路

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128224242.png)

电源开关电路，经常用在各“功能模块”电路的电源通断控制，是常用电路之一。

本文要讲解的电源开关电路，是用MOS管实现的，且带软开启功能，非常经典，是硬件工程师必须掌握的实用电路。

## **一**、电路说明

电源开关电路，尤其是MOS管电源开关电路，经常用在各“功能模块”电路的电源通断控制，如下框图所示。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128224559.png)

在设计时，只要增加一个电容（C1），一个电阻（R2），就可以实现软开启（soft start）功能。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128224703.png)
软开启，是指电源缓慢开启，以限制电源启动时的浪涌电流。

在没有做软开启时，电源电压的上升会比较陡峭。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128224727.png)

加入软开启功能后，电源开关会慢慢打开，电源电压也就会慢慢上升，上升沿会比较平缓。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128224856.png)

浪涌电流可能会令电源系统突然不堪重负而掉电，导致系统不稳定。严重的可能会损坏电路上的元器件。![](../readme.assets/Pasted%20image%2020241128224912.png)

电源上电过快过急，负载瞬间加电，会突然索取非常大的电流。

比如在电源电压是5V，负载是个大容量电容的时候，电源瞬间开启令电压瞬间上升达到5V，电容充电电流会非常大。如果同样的时间内电源电压只上升到2.5V，那么电流就小得多了。下面从数学上分析一下。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128224937.png)

## **二**、原理分析

1、控制电源开关的输入信号 Control 为低电平或高阻时，三极管Q2的基极被拉低到地，为低电平，Q2不导通，进而MOS管Q1的Vgs = 0，MOS管Q1不导通，+5V_OUT 无输出。电阻R4是为了在 Control 为高阻时，将三极管Q2的基极固定在低电平，不让其浮空。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128225449.png)

2、当电源 +5V_IN 刚上电时，要求控制电源开关的输入信号 Control 为低电平或高阻，即关闭三极管Q2，从而关闭MOS管Q1。因 +5V_IN 还不稳定，不能将电源打开向后级电路输出。此时等效电路图如下。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128225511.png)
此时电源 +5V_IN 刚上电，使MOS管G极与S极等电势，即Vgs = 0，令Q1关闭。

3、电源 +5V_IN 上电完成后，MOS管G极与S极两端均为5V，仍然Vgs = 0。

4、此时将 Control 设为高电平（假设高电平为3.3V），则：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128225817.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128225835.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128225848.png)
5、电源打开后，+5V_OUT 输出为5V电压。此时将 Control 设为低电平，三极管Q2关闭，电容C1与G极相连端通过电阻R2放电，电压逐渐上升到5V，起到软关闭的效果。软关闭一般不是我们想要的，过慢地关闭电源，可能出现系统不稳定等异常。过程如下图。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128225904.png)
过慢地开启和关闭电源都可能导致电路系统异常，**这个MOS管电源开关电路及其参数已经过大批量使用验证**，一般情况下可以直接照搬使用。

**三**、电路参数设定说明

调整C1、R2的值，可以修改软启动的时间。值增大，则时间变长。反之亦然。

**如果不想使用软开启功能，直接不上件电容C1即可。**

使用原理图中所标型号的MOS管（WPM2341A-3/TR），通过的电流最好不要超过1.75A，留至少30%的余量，并且要注意散热。

因为下图中该MOS管的数据手册说它超过2.5A会损坏。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241128225940.png)

