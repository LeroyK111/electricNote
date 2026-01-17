
动态范围是指接收器可以容忍的最大输入电平与最小输入电平（即灵敏度）的比值。动态范围等于系统输入处最大信号功率与最小信号功率的比值。将其转换为对数域，我们得到最大可接受功率 Psig, in(max)（以 dBm 为单位）与最小可接受功率 Psig(min) 之间的差异。

 射频灵敏度 ，即最小可接受功率，由 Psensitivity 定义。因此，如图所示，我们无法将输入信号功率电平降低到 Psig(min) 值以下；否则，信号将无法被检测到。类似地，也存在一个上限，我们无法将信号功率增加到超过该值。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251017171539.png)

在 非线性 类似的情况，其中提到我们不允许将幅度增加到超过特定值；否则，系统将出现增益压缩，该值为 A-1dB 或 A1dB。

同样地，我们可以说 Psig(max), 信号功率的最大值，等于 1dB 的功率。如果输入功率超过这个值，系统将经历压缩，变得非线性，无法正常工作。因此，在对数域中，动态范围被定义为最大信号功率与最小信号功率之间的差值。

## 什么是无杂散动态范围（SFDR）？

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251017171653.png)

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251017171653.png)

## 什么是输入参考的IM产物
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251017171802.png)

我们在输入端有干扰源，由于非线性，三阶 IM 产物将出现在我们的通道中，它会阻塞信号。

输出端的 IM3 或三阶 IM 产物（A）可以称为放大器模块（可以是任何模块、系统或整个系统）的输入，因此 A 被称为输入优选的 IM 产物。我们必须注意，这只是比较，因为 A 没有物理存在，我们实际上并没有这样的信号。然而，A 在输出端是真实的，它起到阻塞作用，但我们没有 A。因此，为了定义 SFDR，我们必须通过将输出 IM3 与输入进行比较来做出这样的定义。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251017172007.png)
如果我们从 IM 产品的输出功率中减去对数域中的功率增益，我们可以找到输入参考 IM 产品的输入功率。

SFDR：如果我们增加的功率超过 Pint(max)的值，输入参考的互调产物功率将增加超过噪声基底，这是我们不希望看到的。我们希望功率小于或等于噪声基底。因此，这定义了 SFDR 的上限。所以，在功率增益方面，最大功率和最小功率之间的关系将是：

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251017172031.png)


#### 查找系统的 SFDR

我们可以根据输出端干扰源和IM产物的输入幅度和输出幅度找到IIP3，方程为：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251017172058.png)

利用上述公式，我们可以根据 IIP3 功率和输入参考的 IM3 功率来计算干扰输入功率。当 P 为最大值时，输入参考的 IM3 功率不应超过噪声底噪。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251017172132.png)

SFDR 的示例问题：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20251017172221.png)





