
ADC堪称数字世界与现实世界对话的 “翻译官”。因其设计复杂、专利壁垒高筑，国内 ADC 产业长期处于追赶状态。不过近年来，国产ADC发展迅猛，在中低端市场已站稳脚跟，高端领域也在不断突破。

目前，国产中低端ADC（8~16位分辨率、采样率≤1MSPS）市场已经成熟，高端领域在逐步突破，14~24位高精度ADC已有，高速ADC（≥1GSPS）仍依赖进口，正在攻关。

在这种情况下，工程师怎么看待国产ADC芯片，目前整个行业的发展现状又如何？本文将详细解析。

## ADC芯片，速度和精度不可兼得

ADC全称是Analog-to-Digital Converter模数转换器，一般我们把模拟信号(Analog signal) 用A来进行简写，数字信号(digital signal) 用D来表示。

在自然界中，模拟信号极为常见，诸如压力、温度测量等都是模拟信号。为便于储存、处理与传输，需通过模数转换器（ADC）将模拟信号转化为数字形式，由计算机进行处理。将模拟信号转化为数字形式包含采样和量化两个步骤。

采样是在时间域上对模拟波形进行切分，每个切片近似等于原波形的值，该过程会损失部分信息，但数字系统具备去噪、数字储存和处理等优势，远远超过信息丢失带来的不足。采样完成后，为每个时间片分配数字，这一过程就是量化，量化生成的数字可供计算机处理。

ADC分为五种架构，有些速度快，有些则精度高：并行比较型（Flash），逐次逼近型（Successive Approximation Register），积分型（Integrating），增量型（Delta-Sigma/Δ-Σ），流水线型（Pipeline）等。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250515130412.png)对于ADC来说，高速和高精度不可兼得，因为二者为反比关系。所以说，如何平衡速度和精度，是做好ADC的重中之重。

## ADC芯片，市场现状

数据显示，全球数据转换芯片（含 ADC、DAC 芯片）规模约 280 亿元，其中 ADC 芯片占 80%，约 224 亿元。中国市场约占 35%~40%，规模近80亿元。从应用领域看，通信约35%，汽车占22%，工业占20%，消费占10%。ADC 芯片市场分散、应用广泛，方案能力至关重要。目前，国产ADC主要集中在中低端领域。

国内ADC芯片企业主要分为三派：

- 科研院所：航天 772 所（时代民芯）、中电24所、中电55所、中科院微电子所、吉芯科技（中国电科声光电公司 / 重庆24所）、中科芯集成电路股份有限公司（中电58 所）等；
    
- 上市公司：思瑞浦、纳芯微、上海贝岭、芯海科技、圣邦股份、晶华微、智明达（铭科思微）、振芯科技、振华集团（云芯微）等；
    
- 高校和海归创业公司：昆腾微、灵矽微、韬润半导体、治精微、类比半导体、核芯互联、领慧立芯、微龛、山海半导体、芯炽科技、云芯微、迅芯微电子、士模微（思瑞浦投资）、智毅聚芯、原子半导体、徵格、奇历士、恒芯微、晟朗、共模半导体、英彼森半导体、无锡晟朗、杭州瑞盟、苏州润石等。

## 工程师，在用什么国产ADC

EEWorld网友hjl2832表示，其在工作中主要接触24位ADC，主要使用国际的高端产品。目前他也尝试使用了一些国产产品，根据他的测试，使用国产ADC，同样电路参数国产芯片采样后的波动大于进口的，调整了输入滤波电路的参数，同时加了滤波算法后，结果才能将就保持与进口的一致。

EEWorld网友y7y7y7y7则表示，曾在一个项目中使用过士模微的CM2249这款 ADC 芯片，整体使用体验相当不错，确实是一款值得推荐的国产替代ADC芯片。CM2249是士模自主设计且量产的SAR ADC，已经通过了广五所的AQKK安全可控认证。

EEWorld网友TTDZ在项目中试用过 Everest-semi(顺芯)生产的ES7243E 24位ADC，该产品分辨率达到 8到48KHz。从Datasheet中可以看到，该产品为102dB信噪比，支持-85dB THD+N.24bit 8-200kHz采样率，可应用于麦克风阵列、音响、数字电视、音频接口、音频接收器等场合。

一些工程师推荐上海贝岭的 BL108X系列的高精度SAR ADC产品。该系列产品具有高精度、多通道、高采样速率、零转换延迟的特点，涵盖了1~16通道、16~18位分辨率、100KS/s~1MS/s采样速率，可提供QFN、QFP、TSSOP等多种封装形式。产品广泛应用于工业自动化、智能电网、轨道交通等领域。

另外，还有一些工程师推荐芯佰微电子的CBM16AD125，该产品是双通道、16 位、125MSPS模数转换器(ADC)，支持需要高性能、低成本、小尺寸的多功能通信应用。这款双通道的ADC内核采用多级差分流水线架构，每个ADC均集成高带宽的差分采样保持电路，支持用户可选的各种输入范围。

一些工程师也表示，最近也关注到了上海海思推出的一款高端ADC产品——AC9610 2Msps 24bit ADC芯片。这是一款采用SAR架构的ADC。在上海海思沉淀多年的先进算法和设计上，AC9610采用SAR ADC架构实现了2Msps采样率的同时，保持了24bit的超高采样精度。目前从参数来看，131dB的SFDR非常高，说明ADC的杂散信号非常低，可以提供非常干净的信号。103.5dBFS @ 2Msps，138dBFS @ 1Ksps的SNR（信噪比），ADC的噪声性能非常出色。静态性能指标也好看。不过，具体要看做的是不是集成输入缓冲和参考的模组，类似4630。


