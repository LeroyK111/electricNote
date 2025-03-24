老规矩，我们先聊聊什么时候需考虑劈刀撞击芯片问题，根据以往的浅薄见识，总结了以下几种情况：

1. 研发过程为了快速获得产品初步性能信息，使用未进行减薄的芯片进行验证；

2. 上篇文章所描述的需要加工最短线弧，导致劈刀极度靠近芯片边缘；

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213125445.png)

1. 军工、光通信行业使用管壳封装时，焊线位置靠近腔体导致劈刀下降时有撞击腔体的风险；

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213125518.png)
1. Memory产品存在多叠层设计导致总堆叠厚度过高时；
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213125540.png)

1. 打线反方向（reverse motion）存在芯片、管壳腔体阻挡时；
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213125639.png)

看起来五种情况貌似有点复杂，实则总结起来就如下两种评估方法：

1. Bonding时劈刀下降空间是否足够？适用于上文1，2，3，4；

这种情况比较简单，只需要根据Wire Length(WL)，计算出劈刀尺寸即可，最简单的办法就是在CAD中画个二维模型，也花不了几分钟：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213125655.png)
根据劈刀直径(1.585mm)、长度(常规劈刀为11.1mm)、MTA(典型值为20°)、Tip则可轻松画出二维的劈刀结构图；其中Tip可参考：[Wire Bonding封装Design Rule——2nd Bond焊区尺寸定义](https://mp.weixin.qq.com/s?__biz=MzkxNDcxMjMyNA==&mid=2247484453&idx=1&sn=1923a9def530947aa1f3351dd010562e&scene=21#wechat_redirect)

再根据芯片表面与die pad的高度差(CD，取芯片厚度+银浆厚度)芯片边缘距焊点的距离L，将劈刀底部中心位置放置在焊点上，直接使用标注功能即可轻松测量出gap大小。gap大小必须满足如下条件：

gap＞Die Placement+Wire Bond Placement，此处Die Placement建议取±30um，Wire Bond Placement：通常来讲为±2um，但经常受制于框架、基板的加工精度，因此该数据建议参考框架、基板的加工公差，框架取引脚长宽公差，基板取solder mask公差。

1. Bonding时Loop Reverse motion引起的撞击风险；适用于上文5；
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213125723.png)


将劈刀底部的中心位置移动到Reverse motion的终点即可，该终点是通过reverse height(RH)和Reverse Angle(RA)计算出来的，这个数据计算起来比较麻烦，同线弧高度、长度、一/二焊点高度差均有关系，就不能随便纸上谈兵了，这里推荐取个常见情况的保守值：

RH取线弧一、二焊点的高度差ΔH(这里为了方便理解，刻意将ΔH画的和RH不同)，RA取保守值60°；计算出的gap仍然需满足第1种情况的不等式：gap＞Die Placement+Wire Bond Placement



