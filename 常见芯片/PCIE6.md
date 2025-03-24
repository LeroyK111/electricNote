PCIe 6.0 规范于 2021 年发布，采用 PAM4 调制（即 4 电平脉冲幅度调制），使数据传输速度翻倍，达到 64GT/s。同时，PCIe 6.0 规范使用 FLIT（流量控制单元）作为新的数据传输单元，显著提高了传输效率。

**PCIe 6.0 拥有众多新功能和变化，我们将讨论其中一项重要功能：FLIT。接下来，我们将结合自身的设计和验证经验，重点分析当前面临的挑战，探讨相应的解决方案。**

  

**PCIe 6.0 中的 FLIT 是什么？**

在先前版本中，事务数据以可变长度的形式存在，称为 TLP。它们的包头大小是固定的，但数据有效负载的长度有所不同。无论 TLP 有多长，都是使用 32 位 CRC 进行保护。在 PCIe 6.0 中，由于额外的信号状态，PAM4 信号本身比 NRZ 信号更脆弱。新的调制需要 FEC 来缓解 PAM4 较高的误码率，且要求在固定大小的数据包上实现纠错功能，因此在 PCIe 6.0 设计中使用了 FLIT（流控制单元）。 

FLIT 的长度固定为 256 字节，其中包含 236 字节的 TLP、6 字节的 DLP、8 字节的 CRC 和 6 字节的 FEC。它去除了 1b/1b 编码的同步头、帧定界符等部分。FLIT 也具有类似的序列号概念，其中 DLP 的前 2 个字节包含专用于 FLIT 级别序列号、Ack/Nak、重试机制等方面的相关信息。 

FEC（前向纠错）是针对延迟而设计的，其复杂性会随着纠正的符号数量增加而呈指数增长。6 字节的 FEC 负责 3 个交织组，每组有 2 个 FEC 字节，以防在少于 3 个字节时出现突发错误。 

  

**第一个挑战涉及新的 FLIT 格式和编码变化**

在链路训练、轮询和配置阶段开始时，使用 TS1 中“Data Rate Identifier（数据速率标识符）”字段（符号 4，位 0）中的“FLIT 模式支持”位，实现 FLIT 模式的启用机制和协商。协商成功后，FLIT 模式将适用于所有数据速率，因此也支持 8b/10b 和 128b/130b（混合模式）。

进入 FLIT 模式后，我们采用一种全新的 TLP Header 格式。先前的 TLP Header 存在诸多局限性，例如没有为标签尺寸增加预留空间。为了满足 FLIT 模式的需求，PCI-SIG 对包头进行了重新设计。在此期间，我们面临的挑战是需要测试所有新的组合。

TLP Header 包含 3 到 7 个 DW TLP Header Base，后面跟随 0 到 7 个额外 DW 的 OHC（正交头内容）。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250227175055.png)
新引入的字段具有完全解码的 8b Packet Type（数据包类型）字段。这意味着所有 256 个类型值都已被明确定义或预留用于特定群组，以确保正确实现帧定界和转发。

此外，还为 FLIT 模式设计了新的填写规则。未发布的 TLP 的填写也有重大更新，包括 14 位标签、使用 OHC-A5 的错误报告等。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250227175145.png)


据我们所知，236 字节可以容纳 1 个完整的 TLP 或多个 TLP。如果遇到较长的 TLP，还可以将其拆分成多个 FLIT 进行传输。在每个 TLP 之间，如果没有计划发送更多的 TLP，可能会插入一个 NOP  TLP。PCIe 6.0 规范中还有一些新的规定，例如在 FLIT 的 32 DW 边界内，每个 FC/VC 最多只能包含 4 个 TLP（非 NOP）。

DLP 是一个 6 字节序列，其中前 2 个字节专门用于处理 FLIT 级别的 Ack/Nak、重试等信息，因此设计了新的格式。比如，“FLIT usage（FLIT 用途）”字段用于区分空闲 FLIT、NOP FLIT 或有效负载 FLIT；“Prior FLIT（前一个 FLIT）”字段用于标识非 NOP 或 NOP，避免重试错误的发生；“replay command（回放命令）”和“sequence number（序列号）”用于确定 Ack/Nak/Retry 的执行。

此外，在 FLIT 序列号和重试机制中，新定义了多种 FLIT 传输和交换规则。例如，CONSECTIVE_TX_EXPLICIT_SEQ_NUM_FLITS 和 CONSECUTIVE_TX_NAK_FLITS 计数器及其相关规则。

为了测试所有主要的编码和格式变化，必须确保覆盖所有新的数据包字段。针对上述新功能的通用解决方案是定义并实现良好的覆盖范围，以确保新功能得到充分测试。一个好的覆盖模型有助于充分测试新功能。

  

**除 FLIT 格式外，FLIT 模式带来的另一项重大挑战是新的序列号和重试机制**

在验证过程中，其中一项最难的挑战是IMPLICIT_RX_FLIT_SEQ_NUM 规则。该计数器对于回放机制至关重要。隐式 FLIT 序列号未包含在 FLIT 中，因此完全依靠内部逻辑来处理。内部逻辑/计数器需要处理多种场景，以确保正确计算 IMPLICIT_EX_FLIT_SEQ_NUM。

确保 TX 重试缓冲区的准确性也非常重要，因为在接收 Ack 或 Nak 之前，所有 FLIT 都需要存储在缓冲区。一个 FLIT 可能包含多个 TLP，或者一个大 TLP 可以分成多个 FLIT，因此需要保证重传的 FLIT 不会跳过 TLP 或将额外的 TLP 添加在原始 FLIT 中。这对于 Posted TLP 至关重要，因为它没有 Completion 通知。一旦 TLP 丢失，将导致无法纠正的错误。 

借助新的标准 Nak/选择性 Nak，发射器可以回放特定 FLIT 或多个 FLIT。相关规则对 TX 和 RX 重试缓冲区均有影响。此外，发送标准或选择性 NAK 需要考虑具体的实施场景，因此有时很难预测和检查是否存在协议违规行为。

FEC 算法是一项新功能，我们需要确保 TX 和 RX 两端的计算都正确无误。

  

**基于上述要点，我们尝试了使用以下解决方案来验证设计**

![图片](https://mmbiz.qpic.cn/mmbiz_png/BDlCjzkajjEytZBeyNOzBMoyxm6yrIlBW0NZoeib1AHev3emnGv4sKoLmSuMiaDPwOibGaM31wWz5KElS4ovyeA7A/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

1. 除了按照规范正确实现 IMPLICIT_RX_FLIT_SEQ_NUM 计数器之外，我们还发现，将隐式 FLIT 序列号记录在状态寄存器中或显示在调试日志中，能够更方便地进行比较和调试。

2. 确保您的监视器可以存储所有的 FLIT，将保存的 FLIT 与每个符号上的已退役 FLIT 进行比较，并报告相应的错误。

3. 必须确保 RTL 和监视器都具有足够大的重试缓冲区，以便能够应对在收到标准 NAK 后重新发送多个 FLIT 的情况。另外，需要采用一个检查器，根据规范预测标准和选择性 NAK 传输条件。最后，检查回放的 FLIT 序列号是否与收到的选择性 NAK 相匹配。

4.为了确保 FEC 算法逻辑的正确性，需要使用具有随机符号位置的故障注入测试用例。

![图片](https://mmbiz.qpic.cn/mmbiz_png/BDlCjzkajjEytZBeyNOzBMoyxm6yrIlBhic1yp6VyGG77nhDibDFXibYTmEtPic0BfUuTXKMT1MibS4AO7ClyDZp9Uw/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

总而言之，PCIe 6.0 是一个复杂的协议，具有诸多验证挑战。要成功验证该协议，必须了解新规范的变化，并制定完善的新功能验证计划以及受新功能影响的向后兼容性测试。**Cadence 的 PCIe 6.0 验证 IP 完全符合最新的 PCIe Express 6.0 规范，并为验证 PCIe 6.0 接口交互的器件提供了一种高效且可靠的解决方案。适用于 PCIe 6.0 的 Cadence VIP 提供全面的验证解决方案，可用于验证基于 PCIe 的 IP 和 SoC。**我们正在与早期采用者客户密切合作，旨在加速每个验证阶段的进程。