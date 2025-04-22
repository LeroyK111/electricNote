
Ball Shear Ratio指的是单位面积下焊球的推力值，单位为g/mil²。这里所指的面积，即焊球与Bond pad的有效键合面积，而封装量产过程中，金球直径d属于固定监控项，如下图所示：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250422101211.png)

多数封装厂用直径d去计算焊球面积，并将其近似等效为焊球和Bond pad的有效键合面积。因此根据圆面积计算公式，可以计算出焊球和Bond Pad的结合面积公式如下：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250422101233.png)
正常情况下，我们国内的封装厂更愿意用um来用作测量单位，因此测量的焊球直径d通常单位为um，而Ball Shear Ratio的单位为g/mil²，因此需要将直径换算为英制单位mil，1mil=25.4um，因此换算成mil后键合面积公式如下：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250422101307.png)
如此则可得到Ball Shear Ratio(BSR)计算公式：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250422101351.png)
公式到这里就结束了。再说一个之前我们自己公司内部对于计算BSR时测量Ball size直径的一个观点：我们认为通常所测量的Ball Size直径如下图d所示，但实际的键合面积为红色区域IMC的尺寸，是要小于d值的。这样会导致BSR计算不准确。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250422101405.png)

因此我们内部报告，通常以焊球金边尺寸定义这个d值(通过Cross Section实际测量，这个值要更接近实际键合面积的尺寸)，如下图：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250422101432.png)
不过以前接触过的封装厂，也还都是按黑边算的，因此这里只能追加一句：仅供参考！

最后再回复下之前频繁被提问的关于BAR的问题：这个BAR推荐值源自于哪一个参考标准？以及为什么要取这么一个推荐范围？这个其实在早期的文章中有介绍过，只是我没有提及它就是是BAR：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250422101448.png)

文中的MBD指的是焊球直径，MBH指的是焊球厚度，BAR则是两个数据的比值，具体是谁比谁我忘记了，总之结论是焊球厚度比焊球直径大致推荐在0.18~0.3之间，我们日常调试为了便于记忆，取了个大致中间值，即球厚：球径≈1/4，这个不是什么参考标准，而是某些企业通过长期的DOE验证给出的一个经验值，其影响，也在文中有大致描述。因此这个值只是给参数调试人员一个大致目标，调试到这个比例更容易获得符合要求的产品质量，有利于提升工作效率。












