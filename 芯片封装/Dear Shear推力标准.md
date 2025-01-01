
# Dear Shear推力标准——Die Shear后残留应如何评估？

![](../readme.assets/Pasted%20image%2020241222081952.png)

- 第一种是芯片同底部银浆剥离，背面完全无银浆残留，NG；
    
- 第二种是芯片底部和基板底部都有银胶残留，从图中看几乎均是100%残留，也就是剪切位置在银浆处，OK；
    
- 第三种是芯片底面有银浆残留，基板底部没有残留，等于银浆和基板的结合面剥离，NG；
    
- 第四种是在芯片处剪切，OK；
    
- 第五种是结合力很大，且韧性很足，直接没推动，OK。
![](../readme.assets/Pasted%20image%2020241222082007.png)
![](../readme.assets/Pasted%20image%2020241222082015.png)
![](../readme.assets/Pasted%20image%2020241222082040.png)
![](../readme.assets/Pasted%20image%2020241222082049.png)
![](../readme.assets/Pasted%20image%2020241222082100.png)

# Wire Bond Shear失效标准——键合金丝做完推力后铝垫上是否必须有残金

近日一封装厂的小兄弟在做一款金线键合的产品时，由于Ball Shear过后在芯片的铝垫表面没有残金存在，被其终端客户质疑产品质量不合格，因此咨询我该如何进行调试才能在Ball Shear后留下残金！

起初我并没有在意这个问题，仅仅从参数优化角度向其提供了一些参考，但后来仔细一想，曾做了多年的Wire Bonding设备调试，在做金线产品时，的确经常听到金线Ball Shear后需要有残金的说法，但在实际调试、生产的品质监控中，主要关注的是Ball Shear力度，严谨一点的话会把金球拔掉看看IMC，貌似从来没关注过Ball Shear后铝垫是否会有残金存在。

为此我特地咨询了在国内一些大厂做工艺的朋友，他信誓旦旦的告诉我说金球焊接力的主要通过推力、IMC以及Cross section后的铝垫残留判断其键合性，Ball Shear后的残金只作为参考，不作为管控项。  

通常了解到这里，这件事就算是有了答案，但转头一想，我们面对终端客户的时候，要说服他们，很多时候是需要让他们看到一些文件类的东西的，并不是简简单单的国内大厂是这么管控的就可以，证据呢？而那些大厂的管控又那么严格，怎么可能将其评定标准随意拿给别人看！于是我便在业界标准的大哥JEDEC中寻找突破，终于让我发现了它——JESD22-B116B(Wire Bond Shear Test Method)。

该文件对集成电路封装Wire Bond产品Ball Shear的作业方法、评判标准等项目做了相应介绍，Ball Shear后的模型做了以下分类：
![](../readme.assets/Pasted%20image%2020250101123242.png)
Bond Lift文中共给出了三种模型，前两种为不同键合线与芯片Bond Pad间的焊接，第三种为键合线与基板/引线框架之间的焊接，本文主要讨论键合线与芯片焊盘之间的焊接模式。其Ball Shear后的残留文中也给出了样本参考，Ball Shear后焊球脱离焊盘表面，且无任何剪切痕迹出现。
![](../readme.assets/Pasted%20image%2020250101123252.png)
文中也明确指出遇到Bond Lift时需要检查焊接参数以及检查芯片焊盘表面是否存在异常，因此Bond Lift不可接受
![](../readme.assets/Pasted%20image%2020250101123301.png)
![](../readme.assets/Pasted%20image%2020250101123306.png)
文中也给出了Type2中每种剪切模式下的实物参考图(如下图)，其中Variation A与Variation B焊盘上无焊球金属残留，但焊盘上存在明显的球剪切痕迹；Variation C与Variation D焊盘上有焊球金属残留。
![](../readme.assets/Pasted%20image%2020250101123315.png)

![](../readme.assets/Pasted%20image%2020250101123324.png)
![](../readme.assets/Pasted%20image%2020250101123335.png)
![](../readme.assets/Pasted%20image%2020250101123344.png)


















