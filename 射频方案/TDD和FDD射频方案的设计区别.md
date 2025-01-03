
# TDD和FDD射频方案的设计区别
通信的方式有两种，频分双工（FDD）和时分双工（TDD），TDD指传输数据时需要两个独立的信道，一个信道用来向下传送信息，另一个信道用来向上传送信息。TDD的发射和接收信号是在同一频率信道的不同时隙中进行的，彼此之间采用一定的保证时间予以分离。

## 1. 频分双工
    

频分双工上文说了，两个独立的信道在不同频率同时工作，一个用来收，一个用来发，互不干涉。从设计电路的角度来说，收发通道完全独立，业务处理通道也完全独立。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241201124811.png)
由于收发不同频，必须考虑发射对接收的影响，所以在这里电路需要引入双工器。

双工器其实是一种滤波器，其作用是将发射和接收讯号相隔离，保证接收和发射都能同时正常工作。

双工器的指标设计成为了收发能否独立工作的关键点。

这里引入了一个概念，宽带噪声。

宽带噪声：发射机输出的功率中除去有用信号、杂散、谐波外，在宽泛的频率范围，噪声的功率谱密度。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241201124829.png)
宽带噪声是怎么产生的？

信号源的热噪声经放大器放大；

本振的相位噪声经放大器放大；
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241201124929.png)
信号源的噪声、本振的相位噪声经过级级放大，最后从天线辐射出去。

若不经优化处理，末级功率放大器输出的宽带噪声功率谱密度可达到-100dBm/Hz量级(常见)。U-V频段，在距离50m处宽带噪声功率仍有-160dBm/Hz，这使得周围环境噪底抬高10dB以上。

噪底抬高后，该范围内接收机的接收噪声剧增，接收灵敏度明显下降。

双工器的设计原则一是发射在接收频点的宽带噪声要低于接收频点的噪底。

所以，在电路设计时要计算清楚发射链路的宽带噪声，根据宽带噪声和接收噪底的差值设计双工器的抑制指标。

双工器还有一个设计依据就是外界对接收机的影响，公网频段多，需要考虑其他频率对接收的影响。

## 2 时分双工
    

在TDD模式的移动通信系统中，接收和传送在同一频率信道(即载波)的不同时隙，用保证时间来分离接收和传送信道。

时分双工其实就是半双工，也就是同一时刻只有一个业务在处理。所以从概念上来说，同样的信道带宽，FDD的业务容量是TDD的两倍。从电路设计上来说，数据处理通道收发复用，资源占用少。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241201124958.png)

因为是时分，很多电路也可以复用，比如说频率源。

时分电路设计与频分不同的关键点就是收发切换的时间，为了资源的最大利用，一般都会复用锁相环和资源处理器。

而这个收发切换的时间主要点就是，频率源的切换时间和功放的上升时间。

频率源的切换时间前文有讲过，如果用集成芯片，这个指标的设计主要在芯片的选型，记得加上程序加载的时间。

一般来说功放的爬坡时间都在us级，功率高的时间相对而言长一点，设计难度不大，设计时需要考虑过冲的问题。

TDD的典型就是WIFI，下面的器件是涉及WiFi所用的开关和PA，可以看到都对爬坡的时间有详细的指标阐述。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20241201125027.png)

## 总结

对比两种通信方式，各有优缺点，TDD模式对电路的时间要求很高，但是因为同一时段只有一个信道在工作，所以设计时不用考虑对收通道的影响。

FDD模式，信道业务宽，相对独立，需要考虑宽带噪声的设计。






