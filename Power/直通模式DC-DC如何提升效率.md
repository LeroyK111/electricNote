
    直通模式也就是Pass-Through模式，是DC-DC中的一种在合适的输入与输出电压时使用的直接让输入与输出直通来减小损耗的模式。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728103851.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728103909.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728103945.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728104035.png)
    **2. 突发模式**  

        突发模式Burstmode（BM）是直接通过与输出电压比较来进行PWM调控，当负载很轻时，输出电压会升高，突发模式的控制器降关闭开关管来降低损耗，此时完全由输出电容维持输出电压，当输出电压降低后再此开启开关管。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728104256.png)

**三、直通模式DC-DC**    

    **1. BOOST拓扑的**直通模式****
BOOST的直通模式需要再增加一个开关管，连通VIN与VOUT，当输入电压合适时，打开直通开关管，让电流直接通过这个管子流向VOUT，由于导通后不会有任何的开关动作，只剩下开关管自身的导通损耗，因此效率很高。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728104510.png)

BOOST的直通模式需要再增加一个开关管，连通VIN与VOUT，当输入电压合适时，打开直通开关管，让电流直接通过这个管子流向VOUT，由于导通后不会有任何的开关动作，只剩下开关管自身的导通损耗，因此效率很高。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728104606.png)

        我们以TI的TPS6128x为例来讲解直通模式的应用场景，这个芯片是用于锂电池的升压，所以输入范围为2.3~4.8V，输出电压可调，我们假设一个一个需要稳定3V的负载场景，如下图，TPS61281D作为第一级稳压来充分利用电池的整个电压范围，当电池电压为2.7~3.3V范围内，TPS6128D工作在升压BOOST模式，输出电压为3.3V，而当电池电压为3.3V~4.35V范围内时TPS61281D进入直通模式，输出电压等于输出电压，因此在电池的整个电量周期内，TPS61281D输出电压范围为3.3V~4.35V，再经过二级稳压的BUCK来将输出电压稳定在3V。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728104628.png)
    **2. **BUCK-BOOST拓扑的直通模式****  

        BUCK-BOOST拓扑下的直通模式可以直接让两个上管导通 ，形成经过两个上管和电感的通道，因此无需再另外增加开关管，当然这样也会有电感和两个上管的导通损耗。

![[Pasted image 20250728104719.png]]

        以LT8210为例介绍这个拓扑的直通模式的应用场景。假设负载需要稳定的6V，而输入电压VIN是4~24V，可以用LT8210将升压输出电压设置为8V，降压输出电压设置为16V，则LT8210在输入电压为4~8V时进入boost模式，输出电压固定为8V，输入电压为8~16V时进入直通模式，输出电压等于输入电压，输入电压为16~24V时进入BUCK模式，输出电压固定为16V。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728104737.png)
        如下图是效率曲线以及输入与输出电压关系，可以看出输入电压为８～16V时，效率很接近99％

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728104808.png)


总结来说直通模式的DC-DC虽然效率高，但是使用条件太苛刻了，基本都需要二级降压才行， 可能本来就是为了有二级电源的系统设计的吧。














