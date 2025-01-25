# 如何启动并测试RK3562J处理器的MCU？

![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250125123747.png)


**RK3562J处理器概述**

RK3562J处理器采用了4*Cortex-A53@1.8GHz+Cortex-M0@200MHz架构。其中，4个Cortex-A53核心作为主要核心，负责处理复杂的操作系统任务和应用程序；Cortex-M0核则作为辅助核心，运行裸核系统，能够快速响应和控制，实现实时性要求较高的任务。

## 启动M0核固件的前期准备

目前，飞凌嵌入式OK3562J-C开发板上默认并没有启动M0核固件。因此，我们需要通过一系列步骤来配置和启动M0核。以下是具体的操作步骤：

  
### **1. U-Boot修改**

理论上我们需要打开AMP（非对称多处理）编译宏，但由于飞凌嵌入式OK3562J-C开发板的U-Boot已默认配置AMP功能，因此用户无需进行任何U-Boot修改操作。

### **2. Kernel修改**

**（1）安装工具包**

首先，我们需要安装SCons工具包，用于后续的编译工作。可以通过以下命令进行安装：

```shell
forlinx@ubuntu:~$ sudo apt-get install scons
```
**（2）添加AMP设备树的调用**

OK3562J-C开发板已经添加了AMP设备树的调用，我们可以查看相关配置文件以了解其内容。
```sh
forlinx@ubuntu:~$ cd /home/forlinx/work/OK3562-linux-source/forlinx@ubuntu:~/work/OK3562-linux-source$ vi kernel-5.10/arch/arm64/boot/dts/rockchip/OK3562-C-common.dtsi +include "rk3562-amp.dtsi"
```

rk3562-amp.dtsi 主要内容包括：
```dtsi
/ {/* 描述设备 */    rockchip_amp: rockchip-amp {        compatible = "rockchip,amp";        clocks = <&cru FCLK_BUS_CM0_CORE>, <&cru CLK_BUS_CM0_RTC>,            <&cru PCLK_MAILBOX>, <&cru PCLK_INTC>,        //  <&cru SCLK_UART7>, <&cru PCLK_UART7>,            <&cru PCLK_TIMER>, <&cru CLK_TIMER4>, <&cru CLK_TIMER5>;        //pinctrl-names = "default";        //pinctrl-0 = <&uart7m1_xfer>;        amp-cpu-aff-maskbits = /bits/ 64 <0x0 0x1 0x1 0x2 0x2 0x4 0x3 0x8>;        amp-irqs = /bits/ 64 <GIC_AMP_IRQ_CFG_ROUTE(147, 0xd0, CPU_GET_AFFINITY(3, 0))>;        status = "okay";    };
/* 定义了一些保留内存区域 */    reserved-memory {        #address-cells = <2>;        #size-cells = <2>;        ranges;        /* remote amp core address */        amp_shmem_reserved: amp-shmem@7800000 {            reg = <0x0 0x7800000 0x0 0x400000>;            no-map;        };        rpmsg_reserved: rpmsg@7c00000 {            reg = <0x0 0x07c00000 0x0 0x400000>;            no-map;        };        rpmsg_dma_reserved: rpmsg-dma@8000000 {            compatible = "shared-dma-pool";            reg = <0x0 0x08000000 0x0 0x100000>;            no-map;        };        /* mcu address */        mcu_reserved: mcu@8200000 {            reg = <0x0 0x8200000 0x0 0x100000>;            no-map;        };};
/* 实现Rockchip RPMsg功能 */    rpmsg: rpmsg@7c00000 {        compatible = "rockchip,rpmsg";        mbox-names = "rpmsg-rx", "rpmsg-tx";        mboxes = <&mailbox 0 &mailbox 3>;        rockchip,vdev-nums = <1>;        /* CPU3: link-id 0x03; MCU: link-id 0x04; */        rockchip,link-id = <0x03>;        reg = <0x0 0x7c00000 0x0 0x20000>;        memory-region = <&rpmsg_dma_reserved>;        status = "okay";    };};
```
### 3. 生成配置文件
接下来，我们需要生成M0核固件的配置文件。在RTOS源码目录下，通过复制默认配置文件并运行SCons菜单配置界面来生成所需的配置文件。虽然在此示例中无需进行额外配置，但用户可以根据需求进行相应的配置。

```sh
forlinx@ubuntu:~/work/OK3562-linux-source$ cd rtos/bsp/rockchip/rk3562-32forlinx@ubuntu:~/work/OK3562-linux-source/rtos/bsp/rockchip/rk3562-32$ cp board/rk3562_evb1_lp4x/defconfig .configforlinx@ubuntu:~/work/OK3562-linux-source/rtos/bsp/rockchip/rk3562-32$ scons --menuconfig
```
打开图形化配置界面后，无需配置，直接退出即可。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250125124350.png)
若有其他功能需求，可进行相应配置后再退出并保存。
```sh
forlinx@ubuntu:~/work/OK3562-linux-source/rtos/bsp/rockchip/rk3562-32$ cp .config board/rk3562_evb1_lp4x/defconfigforlinx@ubuntu:~/work/OK3562-linux-source/rtos/bsp/rockchip/rk3562-32$ cp rtconfig.h board/rk3562_evb1_lp4x/defconfig.h
```

### **4. 编译源码**

完成配置文件的生成后，我们可以开始编译源码。通过运行构建脚本，选择相应的defconfig配置，并分别编译Linux系统和M0核固件。编译成功后，会在指定目录下生成 amp.img 镜像文件。
```
forlinx@ubuntu:~/work/OK3562-linux-source/rtos/bsp/rockchip/rk3562-32$ cd ../../../../forlinx@ubuntu:~/work/OK3562-linux-source$ ./build.sh chipLog colors: message notice warning error fatal
Log saved at /home/forlinx/work/3562/git/OK3562-linux-source/output/sessions/2024-08-27_15-48-21Switching to chip: ok3562Pick a defconfig:
1. forlinx_defconfig2. forlinx_ok3562_linux_defconfig3. forlinx_ok3562_linux_mcu_defconfig4. forlinx_ok3562_linux_rtos_defconfigWhich would you like? [1]: 4   //选择第四个配置forlinx@ubuntu:~/work/OK3562-linux-source$ ./build.sh rtosforlinx@ubuntu:~/work/OK3562-linux-source$ ./build.sh mcu
```
编译后在rockdev目录下生成amp.img：
```sh
forlinx@ubuntu:~/work/OK3562-linux-source$ ls rockdev/amp.img  boot.img  linux-headers.tar  MiniLoaderAll.bin  misc.img  oem.img  parameter.txt  recovery.img  rootfs.img  uboot.img  update.img  userdata.img
```

## 烧写镜像

将生成的 amp.img 镜像文件拷贝到电脑中，并将开发板切换到烧写模式。使用烧写工具配置 amp.img 的路径。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250125124602.png)
点击“设备分区表”，读取成功后点击“执行”。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250125124615.png)
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250125124627.png)

## 验证启动

重新启动开发板时按下空格键进入U-Boot菜单。在U-Boot菜单中，输入 3 将 amp start 配置成 on。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250125124651.png)
然后输入 1 重启开发板。在启动过程中，观察U-Boot阶段的打印信息，如果看到与M0核固件启动相关的打印信息，则说明已成功使用U-Boot启动M0核固件。
![](https://raw.githubusercontent.com/LeroyK111/pictureBed/master/20250125124709.png)

##  **总结**
上述操作仅为简单启动M0核并打印信息。实际上，M0核的功能非常强大，支持UART、PWM、I2C、SPI等多种外设接口。（目前飞凌嵌入式暂无更多M0核接口的测试例程，您若有相关需求，可以联系技术支持获取瑞芯微官方资料进行深入学习和开发）