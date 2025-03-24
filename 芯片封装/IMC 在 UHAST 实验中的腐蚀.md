前面我们介绍了铝焊盘在潮湿环境下的腐蚀现象，铝焊盘表面暴露于潮湿的环境中发生腐蚀，并且会在氯的加持下加重并加速。其腐蚀路径如下图(以金、铝键合为例)：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250227175421.png)
外界环境中的水汽(包含溶于水中的氧气)穿透塑封体(Compound)进入封装内部，同存在于塑封体内的Cl离子与Al焊盘表面发生反应，造成Al焊盘表面被腐蚀，并逐步向焊盘内部延申。

但细心的朋友会发现，湿气除了会进入焊盘表面，同时也会沿wire bonding时金球与焊盘之间挤铝的位置到达IMC区域，如下图：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250227175645.png)

那么问题来了，当湿气从金球与铝的缝隙侵入进去接触到IMC时，在这狭小的空间内，腐蚀如何进行.
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250227175712.png)
当水汽携带着Cl离子、氧气沿Wire bonding产生的挤铝中进入时，同时与Al层、IMC层、Au层发生接触，Au属于惰性金属，很难发生腐蚀，因此我们只需要考虑Al和IMC的腐蚀即可。

在早期我们介绍IMC一文中有提到过，IMC即金属间化合物，Au与Al键合时，不同金属间化合物。其被腐蚀过程如下：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250227175728.png)

此时IMC则沿其边缘由外自内被腐蚀，从而降低金球与铝焊盘的结合力，当该结合力低于向上的内、外应力时，便会发生焊球脱落，导致产品失效！

记得早前有人问过我遇到这种情况该怎么改善，除了增加IMC覆盖面积外，还有一个野路子，就是bonding结束后，再用一个大的压力，将球再压扁一些，将挤铝盖住，如此就延长了水汽的侵入路径，且增加了侵入阻力，以此延缓腐蚀！

