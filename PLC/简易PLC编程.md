
目前使用频率最高的PLC编程语言是结构化文本和梯形图，对于没什么基础的技术人员，从梯形图开始学习PLC编程是最快捷的，不管什么品牌的PLC，其梯形图的结构都和实际电气控制回路神似。下面，我们就推荐几种最常用的控制电路，让大家温故而知新。
![](../readme.assets/Pasted%20image%2020241221211504.png)

**1、启动、保持和停止电路**

实现Y10的启动、保持和停止的四种梯形图如图所示。这些梯形图均能实现启动、保持和停止的功能。x0为启动信号，X1为停止信号。图a、c是利用Y10常开触点实现自锁保持，而图b、d是利用SET，RST指令实现自锁保持。
![](../readme.assets/Pasted%20image%2020241221211519.png)
**2、多地控制电路**

下图是两个地方控制一个继电器线圈的程序。其中X0和X1是一个地方的起动和停止控制按钮，X2和x3是另一个地方的起动和停止控制按钮。
![](../readme.assets/Pasted%20image%2020241221211529.png)
  

**3、互锁控制电路**

下图是3个输出线圈的互锁电路。其中X0、 X1和X2是起动按钮，X3是停止按钮。由于Y0，Y1，Y2每次只能有一个接通，所以将Y0， Y1，Y2的常闭触点分别串联到其它两个线圈的控制电路中。
![](../readme.assets/Pasted%20image%2020241221211539.png)

**4、顺序启动控制电路**

如图所示，Y0的常开触点串在Y1的控制回路中，Y1的接通是以Y0的接通为条件。这样，只有Y0接通才允许Y1接通。Y0关断后Y1也被关断停止，而且Y0接通条件下，Y1可以自行接通和停止。X0，X2为起动按钮，X1，X3为停止按钮。
![](../readme.assets/Pasted%20image%2020241221211552.png)

5、电机正反转电路

![](../readme.assets/Pasted%20image%2020241221211607.png)

**6、集中与分散控制电路**

  

在多台单机组成的自动线上，有在总操作台上的集中控制和在单机操作台上分散控制的联锁。集中与分散控制的梯形图如图所示。x2为选择开关，以其触点为集中控制与分散控制的联锁触点。当 X2为ON时，为单机分散起动控制；当x2为OFF时,为集中总起动控制。在两种情况下，单机和总操作台都可以发出停止命令。
![](../readme.assets/Pasted%20image%2020241221211617.png)

