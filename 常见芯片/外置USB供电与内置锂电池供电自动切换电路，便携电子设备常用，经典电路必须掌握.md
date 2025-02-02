
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202190450.png)

很多内置有锂电池的便携电子设备，比如手机，通常采用这样的供电方式：

- 1、没有插入USB电源时，使用内置的锂电池供电。
    
- 2、当插入USB电源时，切换为由外置的USB电源供电，并对锂电池进行充电。
    

下图电路就是实现上述的功能，它来自一款电子书阅读器（Kindle同类产品）：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202190512.png)
这是已量产的电路，成熟稳定，实物电路板如下图所示，几个关键的元器件做了标注：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202190544.png)
本文要讲解的是“外置USB供电与内置锂电池供电的自动切换电路”，所以先把上述电路中不相关的电路隐藏。

也就是隐藏锂电池充电管理、电源滤波等电路：

![](../readme.assets/640%203.gif)

隐藏后变成这样：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202190620.png)
这一下子，电路变得好简单，实现电源切换的功能，竟然只需要一个二极管、一个MOS管、一个电阻！

## 一、电路说明

将上述的“外置USB供电与内置锂电池供电自动切换电路”整理一下，弄好看点：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202190703.png)




## 二、原理分析
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202190729.png)


2、当拔掉USB电源时：

VBUS的电压会从5V开始往下降，电阻R155起到给VBUS放电的作用。

VBUS的电压需要快速下降，因为如果下降慢了，会导致MOS管Q4打开变慢，也就不能很快地切换为电池VBAT供电。

如下图，假设VBUS缓慢下降到4.9V，即MOS管Q4的g极为4.9V。VBUS通过二极管D9降低了约0.3V，变为4.6V，即MOS管的Vgs电压为：

4.9V - 4.6V = 0.3V > 0

MOS管仍然不导通，VOUT的供电没有切换为VBAT。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202190956.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202191142.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202191202.png)

## 三、性能提升  

在拔掉USB电源的瞬间，有没有可能MOS管Q4来不及打开，导致VBAT的电压没有及时切过来？

是有可能的。

MOS管Q4没有快速打开，VBAT供电不能及时续上来，会导致VOUT电压下降过多，VOUT的负载电路就可能工作异常。如果电路的负载较重，拉取的电流较大，尤其容易出现在供电电源切换时VOUT电压下降过多的问题。

怎么办呢？

- 1、可以加快MOS管打开导通的速度。方法是减小VBUS的滤波电容的容值，减小电阻R155的阻值，这都是让VBUS快速掉电，从而让Vgs快点到达令MOS管完全打开的电压。
- 2、在VOUT增加滤波电容，但是效果不怎么明显。
- 3、这是重点！可以给MOS管并联一个肖特基二极管D1，如下图所示：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202191225.png)

该肖特基二极管D1的正向导通压降约为0.3V，比MOS管的体二极管要小。在MOS管完全打开之前，VBAT通过肖特基二极管D1对VOUT进行供电，可以缓解VOUT电压下降过多的问题。

**这个方法非常实用，该电路与方法已经被申请了实用新型专利。**其实很多再普通不过的电路都被申请了实用新型专利，尽管这些电路被大众长期使用在先，具体就不展开了。

## 四、应用案例
除了上述的电子书阅读器有应用之外，还有大量的产品使用了这个切换电路。

比如MicroPython领域著名的**01Studio公司**，其出品的多款开发板都有这个切换电路。

以其中的一款型号为“pyWiFi-ESP32”的开发板举例，其电源部分的电路图如下：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202191312.png)标注对应的实物图：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250202191332.png)


