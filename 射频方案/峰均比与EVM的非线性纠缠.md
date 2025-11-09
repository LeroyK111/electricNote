

## 一、引言    

在现代通信系统中，信号的质量对于数据的准确传输至关重要。峰均比（PAR）和误差向量幅度（EVM）作为衡量信号特性的关键参数，它们之间存在着紧密的联系。深入理解这种关系有助于优化通信系统的设计，提高信号传输的可靠性和效率。


## 二、峰均比（PAR）    

### （一）定义    

峰均比（PAR）定义为信号的峰值功率与平均功率之比，数学表达式为：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250415093616.png)

其中，Ppeak是信号的峰值功率，Pavg是信号的平均功率。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250415093632.png)

高 PAR 值的信号对功率放大器（PA）的线性度要求极高。当信号通过 PA 时，如果 PA 的动态范围无法适应信号的高峰值，就会导致信号失真。这种失真不仅会降低信号的质量，还可能干扰其他信道，影响整个通信系统的性能。

### （二）对通信系统的影响    

高 PAR 值的信号对功率放大器（PA）的线性度要求极高。当信号通过 PA 时，如果 PA 的动态范围无法适应信号的高峰值，就会导致信号失真。这种失真不仅会降低信号的质量，还可能干扰其他信道，影响整个通信系统的性能。


```
%% OFDM信号生成与峰均比分析（兼容R2019a）

%% 参数设置

N = 64;             % 总子载波数

used_carriers = 52; % 有效子载波数（类似802.11a）

cp_len = 16;        % 循环前缀长度

mod_order = 16;     % 16-QAM调制

num_symbols = 1000; % OFDM符号数

%% 数据生成与调制

data = randi([0 mod_order-1], used_carriers, num_symbols);

mod_symbols = qammod(data, mod_order, 'UnitAveragePower', true);

%% 子载波映射

sc_indices = [2:27, 39:64]; % 有效子载波位置

ofdm_freq = zeros(N, num_symbols);

ofdm_freq(sc_indices, :) = mod_symbols;

%% IFFT变换

time_signal = ifft(ofdm_freq, N);

%% 添加循环前缀

cp = time_signal(end-cp_len+1:end, :);

tx_signal = [cp; time_signal];

%% 计算PAPR

papr = zeros(1, num_symbols);

for n = 1:num_symbols

    symbol= tx_signal(:, n);

    avg_power= mean(abs(symbol).^2);    

    peak_power= max(abs(symbol).^2);

    papr(n)= 10*log10(peak_power/avg_power);

end

%% 绘制结果

figure('Position', [100 100 1000 400])

% 时域功率

subplot(1,2,1)

plot(abs(tx_signal(:,1)).^2, 'LineWidth', 1.5)

title('OFDM符号时域功率')

xlabel('采样点'), ylabel('功率')

grid on

xlim([1 N+cp_len])

% CCDF曲线

subplot(1,2,2)

[ccdf_x, ccdf_y] = calculate_ccdf(papr, 50); % 调用函数

semilogy(ccdf_x, ccdf_y, 'b', 'LineWidth', 2)

title('峰均比统计特性 (CCDF)')

xlabel('PAPR (dB)'), ylabel('Probability(PAPR > x)')

grid on

set(gca, 'YScale', 'log')

%% ===== 函数定义=====

function [x_ccdf, y_ccdf] = calculate_ccdf(papr, bins)

    [counts,edges] = histcounts(papr, bins, 'Normalization', 'cdf');

    x_ccdf= edges(1:end-1) + diff(edges)/2;

    y_ccdf= 1 - counts;

end
```
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250415093700.png)
## 三、误差向量幅度（EVM）     

Error Vector Magnitude，误差向量幅度（EVM）用于衡量调制信号的实际向量与理想向量之间的偏差。误差向量(包括幅度和相位的矢量)是在一个给定时刻理想无误差基准信号与实际发射信号的向量差，能全面衡量调制信号的幅度误差和相位误差。

EVM：星座图含有多个矢量，各矢量等概均匀分布，EVM是统计量
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250415093717.png)
EVM 是衡量数字调制信号质量的关键指标。较低的 EVM 值表示信号更接近理想状态，误码率（BER）也会相应降低，从而提高数据传输的准确性。相反，高 EVM 值意味着信号存在较大的失真，会增加误码的概率，严重时可能导致通信链路中断。

## 四、峰均比与 EVM 的关系    

PAPR和EVM都与信号的质量密切相关。高PAPR信号可能导致更大的误差矢量幅度（EVM），因为信号的瞬时功率波动较大，容易受到噪声和非线性失真的影响。具体来说，白噪声引起的EVM与PAPR直接相关，信噪比越差，EVM越差。

PAPR信号经过手机或基站里的功率放大器 时，放大器的工作范围有限，一旦信号峰值超过它的处理能力，就会发生非线性失真 。这种失真会导致两个后果：

带内失真 ：信号本身的星座点（比如下图的64QAM）变得模糊，接收端难以识别；

带外泄露 ：信号像水花一样溅射到隔壁频段，干扰其他设备。

高峰均比比较麻烦，应对的三种策略：

削峰（Clipping） ：直接砍掉信号峰值，简单粗暴但会引入噪声；

编码优化（如SC-FDMA） ：4G/5G中用的技术，主动生成低PAPR信号；

数字预失真（DPD） ：提前反向补偿功放的失真，相当于给信号“戴矫正器”。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250415093745.png)


PAPR-限幅比曲线 ：显示随着限幅强度增加，PAPR逐渐降低

EVM-PAPR曲线 ：呈现典型非线性特性——当PAPR降低（限幅增强）时，EVM显著恶化

## 五、结论    

峰均比与EVM的关系，本质是数据速率和信号质量的博弈。更高的峰均比意味着能传更多数据，但代价是信号容易失真；而过低的峰均比又会浪费频谱资源。峰均比和误差向量幅度是通信系统中相互关联的重要参数。高峰均比的信号容易在通过非线性器件时产生失真，进而导致误差向量幅度增大，影响信号的调制质量和数据传输的准确性。通过理论分析和 MATLAB 仿真，我们清晰地展示了二者之间的这种关系。在通信系统设计中，应充分考虑峰均比和 EVM 的影响，采取有效的措施，如信号预失真技术、功率放大器线性化技术等，来降低峰均比，减小 EVM，从而提高通信系统的性能。



