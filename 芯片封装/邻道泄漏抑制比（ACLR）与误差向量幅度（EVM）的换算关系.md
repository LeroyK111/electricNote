
现代无线通信系统（如5G NR）中，发射机线性度是核心设计挑战。邻道功率抑制比(ACLR)和误差向量幅度(EVM)是衡量发射机性能的两个关键指标：ACLR表征频谱泄露对相邻信道的干扰，EVM反映调制信号的失真程度。本文深入探讨二者间的理论关系，并通过MATLAB建立包含射频非线性的端到端仿真系统进行验证。

1.什么是ACLR？

邻道泄露抑制比是用于衡量下行的发射性能，是主信道的发射功率与测得的相邻信道的功率之比。ACLR值越低，表示相临信道的功率的干扰越小，说明系统的性能越好。一般用dBc作为单位。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728102609.png)

ACLR对无线通信系统的性能和质量具有重要影响。如果ACLR过高，那么相邻信道之间的干扰将会增加，导致接收端无法准确解码原始信号，从而降低了通信质量和可靠性。

2.什么是EVM？

EVM是数字通信中最常用来表征调制质量的量度。它表示了一个星座点距离其理论位置的偏差。  通常用%RMS 或者 dB作为单位。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728102644.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728102703.png)

假设

• 单载波、无记忆、弱非线性 → 可用三阶截点（OIP3）模型近似。            
• 邻道泄漏主要由 3 阶交调产物（IM3）贡献 → 高阶项忽略。            
• 热噪声远小于非线性噪声 → 噪声底由交调决定。

在了解ACLR和EVM之间的关系时，我们先来了解一下ACLR的来源。

ALCR是衡量宽带信号的一个指标。我们可以把宽带信号理解成独立的单音信号的组合。我们知道，输入双音信号时，系统会产生三阶交调。假设下图绿色的线是输入的单音信号，那么红色的信号就是绿色信号的交调产物。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728102725.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728102743.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250728102802.png)

ACLR是因为系统的非线性产生，且产生的非线性叠加在信号和邻道内，抬升了底噪，恶化的噪声一部分落在带内，恶化信噪比。一部分落在邻道，即ACLR。

系统非线性产生的噪声功率均匀分布在主信道和邻道中。设主信道带宽内的噪声功率为Nin，邻道内的噪声功率为Nadj，由于非线性失真的广谱特性，可认为Nin≈Nadj。 因此，ACLR 可表示为：ACLR = P主信道 - Nin=SNR结合 EVM 与 SNR 的关系，最终得到：EVM^2≈ACLR





