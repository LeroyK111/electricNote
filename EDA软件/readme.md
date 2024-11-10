# IC集成电路学习

![](../readme.assets/Pasted%20image%2020241110201652.png)

| 阶段            | 学习目标                   | 推荐工具                                            | 学习顺序                                           |
| ------------- | ---------------------- | ----------------------------------------------- | ---------------------------------------------- |
| 规格定义          | 了解芯片的基本要求和设计目标         | 文档工具 (Word, Excel, Google Docs)                 | 编写芯片规格文档                                       |
| RTL 设计        | 使用 Verilog/VHDL 编写功能描述 | Vivado, Quartus Prime, Synopsys Design Compiler | 学习 Verilog/VHDL，使用 Vivado/Quartus 进行 RTL 设计和综合 |
| 功能验证          | 验证 RTL 设计的功能           | ModelSim/QuestaSim, Synopsys VCS                | 学习基础仿真工具 ModelSim，使用 UVM 进行复杂验证                |
| 综合            | 将 RTL 代码转换为门级网表        | Synopsys Design Compiler, Cadence Genus         | 使用 Design Compiler 进行 RTL 综合                   |
| 静态时序分析（STA）   | 分析门级网表的时序              | Synopsys PrimeTime                              | 学习时序分析基础，使用 PrimeTime 进行 STA                   |
| 布局布线（P&R）     | 物理实现，包括单元布局、全局和详细布线    | Cadence Innovus, Synopsys IC Compiler           | 使用 Innovus 进行从门级网表到 GDSII 的物理实现                |
| 物理验证          | 确保设计符合制造工艺规则           | Mentor Calibre, Cadence Pegasus                 | 使用 Calibre 进行 DRC 和 LVS 验证                     |
| 功耗和信号完整性分析    | 分析功耗和信号完整性             | Synopsys PrimePower, ANSYS RedHawk              | 使用 PrimePower 进行功耗分析，使用 RedHawk 进行信号完整性分析      |
| 最终仿真          | 布线后对设计进行仿真             | Synopsys VCS, Mentor Questa                     | 使用 VCS 或 Questa 进行门级仿真                         |
| GDSII 导出和制造支持 | 生成 GDSII 文件并进行最终验证     | Cadence Virtuoso, Mentor Calibre                | 使用 Virtuoso 生成 GDSII，使用 Calibre 进行最终验证         |


