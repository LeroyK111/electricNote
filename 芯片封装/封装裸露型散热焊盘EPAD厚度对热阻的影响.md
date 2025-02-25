一款Power SOP封装的产品，针对这种Die Pad外露的封装，通常我们把这种外露的焊盘称为EPAD，即Expose Pad,如下图所示：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250225094539.png)

当时有一个小伙伴提出，他们做的是高功率的封装器件，需要这种很厚的EPAD来降低热阻，否则无法满足散热要求；听到这里，另一个小伙伴坐不住了：降低热阻不应该是越薄越好么？热阻大小和传热距离不是成正比关系么？

其实这个问题，我在21年的时候也被搞糊涂过，好在最后结结实实把传热学这本书的前三章看完后，得到了答案，今天顺便就把这个坑填掉，省的再有人掉进去！

其实呢，这两个人说的都没错，只是他们考虑的方向不同而已！

从热传导原理层面来讲，当芯片尺寸和EPAD尺寸接近时，随着EPAD厚度的增加，热阻的确会越来越大：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250225094554.png)

那么当芯片远小于EPAD时又当如何呢？
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250225094609.png)
结果大家看到了，两种情况都会增加芯片与EPAD的热阻，当芯片尺寸远小于EPAD时，因传热过程中导热面积逐步增大，热阻随EPAD厚度的增加幅度会小于芯片尺寸与EPAD接近时。

此时新的问题来了，既然都是导致热阻增加，为什么高功率类封装都喜欢将EPAD做的又大又厚呢？
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250225094622.png)
从上图我们可以看到，当EPAD很薄时，热量向四周扩散能力受限，到达EPAD底部时，热扩散面积几乎等于芯片面积；当EPAD很厚时，热量在向下传递过程中也会同时向四周扩散，到达EPAD底部时，热扩散面积会远大于芯片面积。

当我们将芯片接至PCB板或者外部散热器时，可以将芯片简单看作是等效于上述热扩散面积的一个面热源，此时热量会沿着PCB继续进行传导，芯片的等效热源面积越大，PCB板的热阻则越小。而PCB的热导率远低于EPAD的铜，因此因EPAD加厚所增加的热阻幅度，要远低于因等效热源面积增大时PCB热阻减小的幅度，最终降低整个系统热阻！
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250225094635.png)

