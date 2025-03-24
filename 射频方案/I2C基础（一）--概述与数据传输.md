**I2C****（****Inter-Integrated Circuit****）也称为IIC，**是由飞利浦公司（现在属于恩智浦半导体）开发的串行通信协议，广泛应用于连接低速外围设备，例如ADC、DAC、电源管理芯片、电流采集芯片等，采用两线制，最高支持5Mhz通信频率，支持多主机多从机模式。

## 基本结构

 I2C基本结构如下图，SCL与SDA为通信总线，**SCL：时钟线，SDA：数据线**，由于其协议有冲裁功能，因此上面可以挂多个主机和多个从机，另外由于其开漏输出结构，两根线上均有上拉电阻来提供高电平。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121056.png)

 当然最常见的还是单主机和（单）多从机用法：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121233.png)

## 开漏输出与线与逻辑

**开漏输出：**

    开漏输出是I2C的特点，开漏输出是指输出端通过 **MOS****管的漏极**连接到外部电路，MOS管的源极接地，栅极受控，如下图，此种结构的特点无法输出高电平，当MOS管开启时输出低电平，当MOS管关断时，输出高阻态，也就是说不拉高也不拉低，因此需要外部加一个上拉电阻来实现高电平输出。

    开漏输出的一个好处是即使总线上各个设备有的输出高电平有的输出低电平，由于高电平是由上拉电阻产生，因此总线上不会出现VDD与GND短路。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121452.png)

**线与逻辑：**

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121543.png)

    线与逻辑类似于与门的逻辑，由于开漏输出原因，总线上只要有一个设备输出低电平，那么总线上就会是低电平，如下表：
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121605.png)


## 数据有效性

  I2C再进行数据传输时，需要遵循以下逻辑才能保持数据有效：

    SCL为高电平期间：SDA线上的数据必须保持稳定。

    SCL为低电平期间：SDA线上的数据可以发生变化。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121635.png)


## 数据传输
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121656.png)

**5.1 位传输：**

    从上面的时序图可以看出，I２C传输中，每８位为一个字节，后面跟随一个应答或非应答位
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121714.png)

**应答位（ACK）**：设备将SDA拉低

**非应答位（NACK）**：SDA保持高电平

    另外：

     读（Read）：SDA保持高电平

     写（Write）：主机将SDA拉低

     起始位（Start）**：在SCL为高电平时，SDA由高电平变为低电平。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121739.png)

    终止位（Stop）：在SCL为高电平时，SDA由低电平变为高电平。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121916.png)

**5.2 常见时序**

        以下是最常见的时序的抽象图，该时序由①起始位开始然后主机发送②七位从机地址，后面跟随一个③读或写位，此后等待从机④应答，待从机应答后，⑤如果是读数据，那么后续将会是从机返回数据，主机读取数据；如果是写数据，那么就是主机写入一位数据，读或者写完成后，主机或者从机再次发⑥非应答信号，此后发送⑦停止位停止此次通信。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213121938.png)

另外还有常见的复杂一点的时序，该时序由**①起始位**开始然后主机发送**②七位从机地址**，后面跟随一个**③写位**，此后等待从机**④应答**，从机应答后，主机**⑤发送寄存器地址**，从机收到后再次**⑥应答，**主机收到应答后再次发送⑦起始位，并再次⑧发送从机地址，然后发送⑨读或写位，此后再次等待从机**10. 应答**，主机收到应答后，**11.** 向从机发送数据或者读取从机返回的数据，后续主机或者从机再次发**12. 非应答**信号，最后主机发送**停止位**停止此时通信。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213122014.png)
上述时序的关键在于**第一个字节发送从机地址**，后跟随**第二个字节**从******机某个寄存器的地址**，**第三个字节再次发送从机地址**，然后**第四个字节才是写入或读取的数据**，这样的时序对于稍微复杂一点的芯片通信比较友好，因为可以对不同的寄存器进行配置。

## 实际时序示例：
   如下是一个读数据的时序示意图，从机地址为：1011 001，从机返回的数据为：0101 1101
 ![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250213122102.png)

## **电气性能**

电气性能包含下图时钟频率、IO电平、时序定义等内容，下面将一一介绍（注意：本文只介绍标准模式下的数据，其它模式数据参考协议原文文档）

     以下的时序是常见的，项目中I2C实际波形需要符合这些时序的要求，因此测试中应该用示波器抓到实际波形然后查看每个时序，当然以上只是标准协议，不同的器件要求可能有所不同，需要查询相应的数据手册确定极限值。其他的时序可以参考原文，不再赘述。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219003216.png)

###  **1. VIH与VIL**

VIH：高电平阈值电压，高于此电压才会判断为高电平，一般取VDD+0.5V或5.5V中的小值，例如3.3V的系统中，取3.3+0.5等于3.8V

VIL：低电平阈值电压，低于此电压才会判断为低电平，一般最小值为-0.5V，最大值为0.3VDD，这里的最大最小值器件生产误差或者不同器件的微小差异，后续的最大值最小值也是类似。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219003302.png)


### **2. tlow**

     指低电平持续时间，也就是SDA或SCL一个时钟周期内电平低于0.3VDD的持续时间。标准模式下最小为4.7us。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219003647.png)


### **3. tHIGH**

     指高电平持续时间，也就是SDA或SCL在一个周期内电压大于0.7VDD后直到电压再低于0.7VDD时持续的时间。标准模式下最小为4.0us。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219003706.png)

### **4** **. tr与tf**

     tr是指数据上升沿时间，即SDA从0.3VDD到0.7VDD这一段时间，标准模式下最大为1us，最小不限

     tf是指数据下降沿时间，即SDA从0.7VDD到0.3VDD这一段时间，标准模式下最大为0.3us，最小不限

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219004104.png)

### **5. tHD;STA**

     指开始保持时间，数据SDA变为低电平（SDA低于０.３VDD瞬间开始）一直到SCL开始变为低电平（SCL低于０.７VDD瞬间）这段时长，标准模式下最小值为4.0us
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219004441.png)

### **6** **. tSU;STA**

     指起始建立时间，只有在发完上一个数据Re-Start时才会有这个时序，从SCL变为高电平（SCL电压大于0.7VDD瞬间）开始到SDA开始变为低电平（SDA低于0.7VDD瞬间）这一段时间的时长，标准模式下最小为4.7us。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219004516.png)

### **7** **.  tSU;DAT**

     指数据建立时间，数据SDA完成电平变换（指SDA由0V变为0.7VDD瞬间或从VDD变为0.3VDD瞬间开始）一直到时钟SCL开始采集（指SCL大于0.3VDD）这一段时长，标准模式下最小250ns。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219004538.png)

### **8** **. tHD;DAT**

     指数据保持时间，时钟SCL高电平结束后变为低电平（指SCL低于0.3VDD瞬间开始）一直到数据SDA完成电平转换（指SDA由0V变为0.7VDD瞬间或从VDD变为0.3VDD瞬间开始）这一段时长，标准模式下最小5us。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219004636.png)

###  **9** **. tSU;STO**

     指结束建立时间，从SCL变为高电平（指SCL高于0.7VDD瞬间开始）到SDA开始变为高电平（指SDA高于0.3VDD瞬间）这段时间的时长，标准模式下最小4.0us。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219004712.png)



## **二、上拉电阻的计算**    

        对于标准模式（即通信速率为100k），上拉电阻受电源电压、输入电流和总线电容影响，引脚输入电流有限，一般为3mA，总线电容Cb最大为400pF。

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219004828.png)
   由于引脚输入电流有限，过大可能损坏设备，因此可以计算上拉电阻的最小值Rpmin=（VDD-0.3VDD）/0.003，取0.3VDD是因为低电平最大值为0.3VDD。以VDD为3.3V计算，Rp的最小值为（3.3-3.3*0.3）/0.003=0.77kΩ

     另外由于前面讲过的上升沿时间tr最大值（标准模式1us）影响，可以计算上拉电阻的最大值。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250219004850.png)

