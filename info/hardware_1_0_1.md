V1.0的硬盘盒图片找不着了，所以只有V1.1的图片：

![](resources\image\hardware_1_0_1\V1_1.png)

因为V1.0和V1.1的硬件设计并无太大差异，所以主要以硬件版本V1.1为案例进行讲解。

***

**方案图：**

![](resources\image\hardware_1_0_1\diagram.png)

`VL822-Q7`通过上游端口与主机（Host Device）进行连接，并通过下游端口口连接到`ASM2362`和`ASM1352R`。

`ASM2362`扩展一个NVMe下游端口用于连接单个PCIE NVMe协议硬盘，`ASM1352R`扩展两个SATA下游端口用于连接两个SATA协议硬盘。

由于USB3与USB2的独特性，故在设计时将`VL822`第四下游端口的USB2.0单独级联`GL852G USB2.0 HUB`，`GL852G`所拓展的四个USB2.0端口可用于其他2.0下游端口的物理连接，并同时连接`ESP32-S2`以进行通信与控制。

***

**供电方案图：**

![](resources\image\hardware_1_0_1\power_diagram.png)

硬盘盒可通过USB上游端口5V供电（USB_5V），也可通过外接的`DC-ZXDN10`（EXT_5V）降压供电，后者的供电能力更强。

USB_5V和EXT_5V通过具有优先级切换功能的多路电源复用器芯片`TPS2117`进行电源链路管理。`TPS2117`最大可输出4A电流。

GL852G使用硬盘盒5V供电。

ESP32使用`TPS62A02`单路DCDC降压芯片供电。

VL822使用`MP212`2双路DCDC降压芯片供电，ASM2362和ASM1352R使用`EA3059`四路DCDC降压PMIC降压供电。同时PCIE NVMe协议硬盘还会和ASM2362共享`EA3059`其中一路3.3V输出。

MP2122和EA3059上游均由`MT9700-N`限流保护芯片管理，起到限流和电源开关的作用。`MT9700-N`可通过`ESP32-S2 GPIO`进行上电控制。

SATA硬盘使用单独供电。SATA1（NGFF SATA）使用`RY9050`单路DCDC降压芯片供电，SATA2（2.5 SATA）使用硬盘盒5V供电。SATA1可直接通过`ESP32-S2 GPIO`控制EN引脚进行上电操作，SATA2通过`ESP32-S2 GPIO`控制一路`MT9700-N`进行上电操作。

> Ver.1.1版本电源复用MUX已修改为TPS2121方案，最大支持5A电流。</br>
> Ver.1.1版本中NVME供电为RY9050独立供电。</br>
> Ver.1.1版本中，SATA2的限流开关更改为PW1555方案。</br>
> Ver.1.1版本中外接供电由ZXDN10方案修改为MP8771方案。

***

Ultra Enclosure的电源控制方案采用单片机模拟HID设备，并通过HID协议与HOST主机通信。

V1.1单片机方案最终敲定了`ESP32`作为中控方案。

***

**ESP32-S2控制原理图：**

![](resources\image\hardware_1_0_1\esp32s2_diagram.png)

通过编写固件将`ESP32-S2`设置为自定义HID通信设备，主机端通过软件与`ESP32-S2`通信以控制或获取`ESP32-S2`的GPIO状态，`ESP32-S2`根据通信内容控制下游的GPIO状态，从而控制整个硬盘盒。