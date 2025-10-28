**Controller工作原理：**

通过TinyUSB将ESP32-S2模拟为自定义HID设备，同时使用自定义HID报文通信控制ESP32 GPIO状态。

> **相关资料：**<br>
> [ESP-IDF开发USB HID设备记录（3）——ESP32开发自定义HID设备，通过HID报文通信](https://www.bilibili.com/opus/1093492407233675281)
> [ESP-IDF开发USB HID设备记录（4）——利用HMAC-SHA256给自定义报文加“料”](https://www.bilibili.com/opus/1093956714689986579)

电脑端则使用基于Python+Pyside6开发的R-SODIUM Ultra Enclosure Manager控制端进行硬盘盒控制。

![](resources\image\software\gui.png)