# 介绍

## 欢迎

若你是个使用 [ROC-RK3328-CC] 的新手, 这个 [开始上手](started.html) 为你提供了烧写固件并使其运行所需的一切指南

若你需要帮助并且通读 [开始上手](started.html), 看下 [FAQ](faq.html)

若你还找不到你想要的, 看 [串口调试](debug.html) 一节, 生成日志, 在 [资源](resource.html) 处联系我们, 帮助修改这份文档

## 说明


[ROC-RK3328-CC], 是 Firefly 第一块仅有信用卡大小且经济实惠的开源主板, 特征为:
 * 核心
   - Quad-Core ARM® Cortex-A53 64-bit 处理器, 高达 1.5GHz 的主频
   - ARM Mali-450 MP2 Quad-Core GPU, 支持 OpenGL ES1.1/2.0, OpenVG1.1
   - DDR4 RAM (1GB/2GB/4GB)
 * 连接
   - 2 x USB 2.0 Host 口，1 x USB 3.0 Host 口
   - 10/100/1000Mb 网口
   - 1 x IR 红外模组，支持自定义 IR 远程控制
   - 40 引脚拓展接口, 包含 GPIO, I2S, SPI, I2C, UART, PWM SPDIF, DEBUG 
 * 显示    
   - HDMI 2.0 ( Type-A ), 支持最大 4K@60Hz 显示
   - TV out, CVBS 显示, 参照 480i, 576i 标准
 * 音频    
   - I2S, 支持 8 通道
   - SPDIF, 音频输出
 * 视频
   - 4K VP9 和 4K 10位 H265 / H264 视频解码, 可达 60fps
   - 1080P 多格式视频解码(WMV, MPEG-1/2/4, VP9, H.264, H.265)
   - 1080P 视频解码, 支持 H.264/H.265
   - 视频后处理器：去隔行，去噪，边缘/细节/颜色优化
 * 存储
   - 高速 eMMC 扩展接口
   - MicroSD (TF) 卡槽

([全部说明](http://en.t-firefly.com/product/rocrk3328cc.html#spec))

这款令人难以置信的超小型主板能凭借其低功耗平稳安静地运行 Android 7.1 或 Ubuntu 16.04

## 包装 & 配件

[ROC-RK3328-CC] 标准套件包含以下项目:
 - ROC-RK3328-CC 主板
 - Micro USB 线

在开发人员的工作中，强烈建议使用以下配件：
 - [5V2A US Adapter]，一个好的电源是必须的
 - [USB Serial Adapter]，用于串口调试
 - [eMMC flash]，提供更高的系统性能和可靠性

The following accessories are highly recommended, especially when you are doing developer's work:
 - [5V2A US Adapter], a good power source is a must have
 - [USB Serial Adapter], for serial console debugging
 - [eMMC Flash], provides more system performance and reliability

[ROC-RK3328-CC]: http://en.t-firefly.com/product/rocrk3328cc.html "ROC-RK3328-CC Official Website"
[USB Serial Adapter]: http://shop.t-firefly.com/goods.php?id=32
[5V2A US Adapter]: http://shop.t-firefly.com/goods.php?id=68
[eMMC Flash]: http://shop.t-firefly.com/goods.php?id=69
