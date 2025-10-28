# 单片机I/O口驱动，单片机I/O口驱动，为何三极管成首选而非MOS管？

1.单片机为什么不直接驱动负载？  

2.单片机为什么一般选用三极管而不是MOS管？
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250328104105.png)

答：  

1.单片机的IO口，有一定的带负载能力。但电流很小，驱动能力有限，一般在10-20mA以内。所以一般不采用单片机直接驱动负载这种方式。

2.至于单片机为什么一般选用三极管而不是MOS管？需要了解三极管和MOS管的区别，如下：

①三极管是电流控制型，三极管基极驱动电压只要高于Ube（一般是0.7V）就能导通。

②MOS管是电压控制型，驱动电压必须高于阈值电压Vgs（TH）才能正常导通，不同MOS管的阈值电压是不一样的，一般为3-5V左右，饱和驱动电压可在6-8V。

**我们再来看实际应用：**

处理器一般讲究低功耗，供电电压也越来越低，一般单片机供电为3.3V，所以它的I/O最高电压也就是3.3V。  

①直接驱动三极管  

3.3V电压肯定是大于Ube的，所以直接在基极串联一个合适的电阻，让三极管工作在饱和区就可以了。Ib=（VO-0.7V）/R2。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250328104216.png)
②驱动MOS管

通过前面也了解到，MOS管的饱和电压>3.3V，如果用3.3V来驱动的话，很可能MOS管根本就打不开，或者处于半导通状态。

在半导通状态下，管子的内阻很大，驱动小电流负载可以这么用。但是大电流负载就不行了，内阻大，管子的功耗大，MOS管很容易就烧坏了。

所以，一般选择I/O口直接控制三极管，然后再控制MOS管。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250328104236.png)


当I/O为高电平时，三极管导通，MOS管栅极被拉低，负载RL不工作。

当I/O为低电平时，三极管不导通，MOS管通过电阻R3，R4分压，为栅极提供合适的阈值电压，MOS管导通，负载RL正常工作。

为什么要这样操作呢？一定要用三极管来驱动MOS管吗？

那是因为三极管带负载的能力没有MOS管强，当负载电流有要求时，必须要用MOS管来驱动。

那可以用I/O口直接驱动MOS管吗？答案是可以的，但这种型号不好找，这里给大家推荐一个NMOS型号：DMN6140L-13

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250328104302.png)
这个管子的阈值电压是1V，3.3V的时候可以完全导通，导通时的最大电流大约2.3A的样子。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250328104318.png)

我们再来看看，常用的NPN三极管LMBT2222ALT1G的带载能力，最大电流IC=600mA。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250328104332.png)

可见MOS管的驱动能力是三极管4倍，所以对负载电流有要求的都使用MOS管。  

那他们的价格相差多少呢？搜了一下，MOS管的三极管的价格几乎是三极管的6倍。如下图：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250328104353.png)
所以，在要求不高，成本低的应用场合，一般使用三极管作为开关管。




