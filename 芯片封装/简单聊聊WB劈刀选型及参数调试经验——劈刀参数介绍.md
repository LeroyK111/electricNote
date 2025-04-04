![](../readme.assets/Pasted%20image%2020250101021016.png)
![](../readme.assets/Pasted%20image%2020250101021028.png)
设备公差我们都知道，是±2.5um，最难以评估的就是这个挤铝，与铝层厚度、材质以及Fab厂工艺关系巨大，因此只能根据相似产品的调试经验去评估。如果对要调试的产品挤铝没概念，也可以在范围内选一个常用的尺寸去做尝试，毕竟这玩意试错成本很低，大不了换一个。如果之前完全没接触过WB，也可直接参考下图SPT给出的参考公式：
![](../readme.assets/Pasted%20image%2020250101021056.png)









T：Tip Diameter

[Wire Bonding封装Design Rule——BPP定义](https://mp.weixin.qq.com/s?__biz=MzkxNDcxMjMyNA==&mid=2247484432&idx=1&sn=5b9a97ab7c1da72602013cc9971196ac&scene=21#wechat_redirect)一文中也有过相关介绍，Tip的尺寸对鱼尾长度起着主要决定作用，Tip越大，鱼尾同键合面的结合力越强，也越容易调试，因此通常Tip在满足BPP限制的情况下越大越好。
![](../readme.assets/Pasted%20image%2020241225091757.png)
FA(Face Angle) & OR(Outside Radius) 

FA与OR相切，以获得更为平滑的过渡，因此通常小的FA匹配大的OR，大的FA匹配小的OR。鱼尾的厚度受FA影响较大，鱼尾根部(线与鱼尾交界处)变化平滑程度主要受OR影响。
![](../readme.assets/Pasted%20image%2020241225091804.png)

从图片我们可以看出，小的FA匹配大的OR可以在键合的过程中使鱼尾获得更为均匀的受力，以增强整个鱼尾长度方向的键合效果，但会导致FA与OR衔接位置附近鱼尾厚度相对较薄，鱼尾根部相比更容易出现裂痕；

大的FA匹配小的OR，结果则相反，整个鱼尾受力不均匀，靠近鱼尾根部位置相比靠近劈刀中心位置键合牢固能性差，容易出现鱼尾脱落，或stitch pull后残留较少，但FA与OR衔接处鱼尾厚度相对较厚，不容易出现根部撕裂风险。

![](../readme.assets/Pasted%20image%2020241225091814.png)

因此通常在键合框架镀层较硬的产品时，镀层不容易在鱼尾根部下方挤出，但键合较为困难，我们选择小的FA配大的OR，以增强整个鱼尾的键合效果；当遇到键合框架镀层较软产品时，容易获得较好的键合效果，但镀层也容易在鱼尾根部下方挤出，增加鱼尾根部断裂风险，此时大的FA配合小的OR则为最佳选择。

![](../readme.assets/Pasted%20image%2020241225091820.png)


如果有人看到这里觉得有点云里雾里，也没关系，对于大多数产品调试来讲，并不需要过多关注FA与OR，玩好tip就够了，整个WB生涯，我几乎也没怎么研究过这几个参数。

最后来讲讲个人对Tip选择与BPP关系的一些看法。


![](../readme.assets/Pasted%20image%2020241225091827.png)

我们先来看看常见的劈刀选型方式，即1/2*Tip<BPP-1/2*MBD，但是大家不知有没有提出过质疑，为什么该选型公式未考虑设备±2.5um的公差，若两个焊球相互靠近，即多出了5um的距离，这对ultra fine pitch的劈刀来讲也是个不小的数据；难道这些专家真的这么蠢吗？想不到这个问题？

此时我们回到前面讲的FA界面，因FA的存在，会导致劈刀tip边缘会高于球厚，因此即使两焊球相邻距离再靠近5um，tip也不见得会碰到相邻焊球(专家真的该用砖头拍死妖言惑众的我了)；

既然焊球边缘碰不到，那么到底可以向内延申至什么地步？个人认为可以延申至CD这个位置，但此时需将设备公差精度计算进去，因为我们很难去评估CD的台阶与劈刀Tip处到底哪个高，于是公式又变为：1/2*Tip<BPP-1/2*CD-5um；我们在上一篇文章中介绍过，MBD≈CD+8~12um，取中间值10um，则该公式可变换为1/2*Tip<BPP-1/2*(MBD-10um)-5um=BPP-1/2*MBD，此时两个公式一致(等于半天说了个废话。不过个人更习惯用1/2*Tip<BPP-1/2*CD-5um去计算，因为CD是个定值，而MBD会随参数变化。

另外最近遇到很多WB调试的新人，希望能给他们提供点建议，以下是个人WB调试工作时一点点小经验，希望能帮助到即将或者正在走弯路的人：

对于一个新人，大家不用去每天看各种参数概念或者某款产品的调试经验，应先把精力放在WB 1st bond、2nd bond以及loop工艺品质的判断上，也就是去了解其外观形状、大小参数、以及各工艺指标的最佳目标值是多少；知道目标值之后再去研究设备的运行动作与参数之间的关系；了解到这些之后就随便折腾去吧，折腾到某个参数可达到该目标后，然后去进行反推，尝试去解答为什么。至于这个为什么是不是真理，对于一个初学者来讲并不重要，只要在当前的条件下能说服自己就好，时间久了，见识广了，你也就出师了！当然我这都是野路子，如果有正儿八经的师承某派，就中规中矩点。

