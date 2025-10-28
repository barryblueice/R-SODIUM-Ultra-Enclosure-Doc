Ultra Enclosure本质上还是HUB拖多个桥的方案，没有什么新奇的技术。所以我在开发的时候力图添加更多便捷实用的功能。~~（多穴齐插）~~

自硬件版本v1.1开始的每一代Ultra Enclosure旨在通过软硬件结合，造出能适应用户多种需求的超强硬盘盒。

***

目前Ultra Enclosure已产出三代。其中主线代2代，改版代1代。

 - Ultra Enclosure 1.0为所有Ultra Enclosure的祖师爷。使用VL822作为USBHUB方案。第一版仅使用了STC 8051系列作为硬盘盒的电源控制系统。通过拨动开关切换不同硬盘盒的上电状态。
 - Ultra Enclosure 1.1在1.0的基础上弃用了物理拨动开关，并改用了ESP32-S2并将其模拟为USB-HID设备与电脑进行通信。同时修改了部分供电方案，使得其使用体验远远超过1代。
 - Ultra Enclosure 2.0仍在开发中，加入了非常多的实用功能。

