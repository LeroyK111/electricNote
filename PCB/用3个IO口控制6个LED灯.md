
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115205716.png)

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115205817.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115205913.png)
6个LED灯为相同的型号，为方便查看，用红绿两种颜色区分两种不同的方向。

## 一、原理分析

这个电路用到了单片机GPIO的三种状态：

- 高电平
    
- 低电平
    
- 高阻态
    

所谓“高阻态”，是指GPIO对外部电路表现出极大的阻抗。因阻抗很大，几乎不会吸入电流，也不会对外输出电流。

各个LED灯单独亮起，分为六种情况。

1、当只有LED1亮起时，单片机各GPIO的状态如下：（带箭头的红线为电流回路）
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115210212.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115210235.png)

## 二、总结提升

以上其实是用了一种叫“查理复用”（Charlieplex）的方法。

为什么叫查理复用？

很简单，因为这个方法来源于美信半导体公司的工程师Charlie Allen。

查理复用是一种能够在驱动LED，特别是驱动大量LED时有效地节约GPIO的方法。

使用该方法，n个GPIO可以驱动 n*(n-1) 个LED，所以：

- 使用2个GPIO可以驱动2个LED。
    
- 使用3个GPIO可以驱动6个LED。
    
- 使用4个GPIO可以驱动12个LED。
    
- 以此类推。
    

这种方式能够实现的基础是：

- 单片机GPIO的三个状态：高电平、低电平、高阻态。
    
- LED具有单向导电性。
    

查理复用设计的方法：

- 任意两个GPIO引脚之间串入两个LED，这两个LED为并联，且LED方向相反。
    
- 当你想要点亮某个特定的LED时，就将其两端所连接到的GPIO引脚分别设定为高电平和低电平，其它剩余的GPIO引脚设定为高阻态。
    
- 前面电动牙刷中6个LED灯的电路，就是这么设计的。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115210318.png)
这里只用到高电平、低电平的状态，不需要用高阻态的状态。


2、使用3个GPIO时，前面已经分析过：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115210502.png)
可以等效为下图：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115210518.png)

可以看出，确实是任意两个GPIO之间均串入了两个并联的LED，且LED方向相反。

3、同样的原理，使用4个GPIO时：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115210544.png)
其次，如果出现了某个LED开路或短路的情况，电流的流向会被打乱，LED亮起来的逻辑会变得错乱。最坏的情况下，电路会对GPIO索取大电流，导致单片机损坏。下图是假设LED1短路，那么在点亮LED5时，LED3也会亮起：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250115210600.png)

## 三、继续进阶

如果要同时亮起两个以上的LED，怎么办？

交替点亮他们就行，只要交替切换的速度够快，由于人眼的视觉暂留效应，看起来就是同时亮起的。

值得一提的是，如果要同时亮起的LED较多，比如大规模的LED点阵，那么还要注意一些新的问题，颇有门道。










