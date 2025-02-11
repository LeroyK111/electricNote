## Wire Bonding设计中，最短线弧该如何设计？

首先介绍下什么时候需要考虑这种问题。我总结了以前遇到过的如下几种情况：

1. 封装尺寸需要控制在一定大小以匹配系统设计预留面积限制，而芯片设计尺寸好死不死又无法减小至design rule以内，这时候就只能通过缩减封装wire layout占地面积来将最终封装尺寸控制在预期范围内；

2. 射频产品因阻抗匹配要求某根线3D线长尽量又低又短；

3. 系统设计中，由于芯片外购，并非专门为该系统方案设计，存在尺寸焦虑、厚度太大或者信号完整性需求；因此需评估最短线长；

情况虽多种多样，但目的都是一样的，即获得最短的投影线长，下面我们以QFN的地线进行举例进行说明：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250211121610.png)

当我们需要将线长做到最短的时候，会出现最大的风险则是线弧与芯片边缘之间的gap无法保证，gap的大小主要受线长(WL)、1st/2nd bond高度差(CD)、线弧高度(LH)、焊盘与芯片边缘的距离(d)影响；与此同时，芯片最小线长受沾片胶爬胶距离(c)制约，且需考虑在芯片较厚时劈刀至2nd bond时劈刀不会撞击芯片。

关于gap的测量手段，通常是通过光学测量显微镜测量线弧在芯片边缘位置与芯片高度差，再减去线径所得，如下图所示：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250211121624.png)

其中wire gap则是我们在产线的实测值，该项通常需>1*WD(WD为焊丝直径)。

我们先忽略沾片胶爬胶距离以及劈刀撞击芯片边缘的影响，只考虑WL、CD、LH、d对gap的影响。

由于通常我们遇到这种需求时，均是要求3D线长最短，因此在线弧Down Bond时(即线弧由上自下bonding)，常将线弧高度LH控制在100um以内。

此时对gap大小起决定性作用的则是WL、CD、d以及工艺参数调试，我们作为封装设计工程师需要在满足产品性能需求的情况下尽量将工艺难度降低，此时则需要评估在工艺难度适中的情况下，WL、CD、d之间的关系。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250211121640.png)

我们以该模式下常用的2个kink的线弧做介绍，何为2个kink，如下图所示，也就是使用两个折角的结构(1st kink和2nd kink将线弧撑起，以增大线弧与芯片边缘之间的间距)。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250211121655.png)

而当WL太短，CD太大时，2nd kink撑起的难度也随之增加，为此我咨询了某WB设备厂商研发中心负责looping的专家。

其表示，在Down Bond条件下，WL≥1.2*CD，2nd kink折角较容易实现；距离再近，则2nd kink很难做出折角，即线弧形状类似于Q-loop，如下所示，很容易碰到芯片边缘。(至于为什么会这样，主要同loop成型的motion有关，大家对此若有兴趣，可在评论区留言，我会根据需求度考虑后续是否出一篇)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250211121709.png)
而1st bond 与 2nd bond 的高度差CD可以通过芯片厚度和沾片胶厚度获得，由此我们则可轻松计算出最小的WL，但还需加一个限定条件，即不考虑pad距芯片边缘间距，好在大多数IC设计时会将pad尽量靠近芯片边缘。

但倘若IC设计时pad偏内部，导致Down Bond方案无法获得＞1倍线径的gap时又当如何？此时我们则可基础终极大招Up Bond(即线弧由下自上bonding)，如下图：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250211121723.png)
关于Up Bond，d的距离对wire gap的影响则要小很多，当芯片厚度比较小的时候，劈刀在打1st bond时不碰到沾片胶，劈刀边缘不会撞击芯片边缘即可；而随着芯片厚度的增加，CD过大时，WL也会对调试难度造成较大的影响，此时推荐WL≥1.0*CD；否则线弧会出现如下两种情况：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250211121744.png)
现在我们来总结下上面的信息：我们在评估最短线弧时，首先需考虑劈刀在基板、框架点位是否会碰到沾片胶(即下图L需大于c)；其次根据Down Bond的推荐参数WL≥1.2*CD，确定WL值；然后再评估劈刀是否存在撞击芯片边缘的风险；此时则可确定最短线长；由于Up Bond的Disign Rule相比Down Bond更为友好，在产品导入时，若遇到工艺调试难度问题导致Down Bond无法确保线弧与芯片间的gap，则可直接启用Up Bond进行调试。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250211121756.png)








