[![version](https://img.shields.io/github/release/CyrilTaylor/Legion-Y9000X-Hackintosh/all.svg)](https://github.com/CyrilTaylor/Legion-Y9000X-Hackintosh/releases)
[![license](https://img.shields.io/github/license/CyrilTaylor/Legion-Y9000X-Hackintosh.svg)](https://github.com/CyrilTaylor/Legion-Y9000X-Hackintosh/blob/main/LICENSE)

# :warning:版本尚未发布，还存在若干问题，请勿直接用于实际引导

<!-- TOC -->

- [1. 硬件配置](#1-硬件配置)
- [2. BIOS设置](#2-bios设置)
- [3. /EFI/OC/config.plist文件配置](#3-efiocconfigplist文件配置)
- [4. 功能支持详情](#4-功能支持详情)
- [5. 版本详情](#5-版本详情)
- [6. 启动参数说明](#6-启动参数说明)

<!-- /TOC -->

## 1. 硬件配置

|   硬件    | 型号                                                         |
| :-------: | :----------------------------------------------------------- |
|    CPU    | Intel i7-9750H                                               |
|  显示器   | UHD 4K                                                       |
| WiFi/蓝牙 | Intel AX200<br />黑苹果支持不完美，已更换为DW1820A，并且屏蔽前三后二总共五个引脚 |
|   硬盘    | 三星PM981A<br />黑苹果大量读写数据时异常，基本无解，已更换为铠侠RD10（原东芝RD500）1TB SSD<br />ACPI中会加入屏蔽第二个磁盘位的SSD补丁，所以安装黑苹果的磁盘应位于第一个磁盘位（靠近风扇、远离电池的磁盘位） |

EFI测试环境

|     类型     | 配置                                       |
| :----------: | :----------------------------------------- |
|   操作系统   | macOS 10.15.5                              |
| 显示器分辨率 | 4K                                         |
|   固态硬盘   | 铠侠RD10 1TB，并且屏蔽第二个磁盘位的PM981A |
|  WiFi/蓝牙   | 博通DW1820A                                |

## 2. BIOS设置

- :white_check_mark: 磁盘模式设置为AHCI模式`Configuration -> Storage -> Controller Mode -> AHCI mode`
- :white_check_mark: 关闭安全启动`Security -> Secure Boot -> Disabled`
- :white_check_mark: 安装完成后将黑苹果所在的磁盘调节为第一启动顺序，如此每次开机默认进入黑苹果系统

## 3. /EFI/OC/config.plist文件配置

:warning:仓库默认配置是清除了三码的，所以需要使用者填充自己的三码，仓库目前最新的Release版本里config.plist是用[OpenCore Configurator v2.6.0.2](https://mackie100projects.altervista.org/occ-changelog-version-2-6-0-0/)版本编辑的，想用OpenCore Configurator编辑并且生成三码的，则也必需用同样版本的OpenCore Configurator。

```diff
diff --git a/EFI/OC/config.plist b/EFI/OC/config.plist
index 12596e4..45d83e3 100644
--- a/EFI/OC/config.plist
+++ b/EFI/OC/config.plist
@@ -1285,17 +1301,17 @@
                        <key>AdviseWindows</key>
                        <false/>
                        <key>MLB</key>
-                       <string></string>
+                       <string>*****************</string>
                        <key>ROM</key>
-                       <data></data>
+                       <data>********</data>
                        <key>SpoofVendor</key>
                        <true/>
                        <key>SystemProductName</key>
                        <string>MacBookPro16,1</string>
                        <key>SystemSerialNumber</key>
-                       <string></string>
+                       <string>************</string>
                        <key>SystemUUID</key>
-                       <string></string>
+                       <string>********-****-****-****-************</string>
                </dict>
                <key>UpdateDataHub</key>
                <true/>
```

OpenCore Configurator中填充三码：

![image-20200705010739620](README/image-20200705010739620.png)

## 4. 功能支持详情

- :white_check_mark: 解锁CFG Lock
- :white_check_mark: 4K内置显示器
- :white_check_mark: 电池管理、电池电量显示
- :warning: 合盖休眠、开盖唤醒
- :white_check_mark: CPU温度传感器
- :warning: 3.5mm耳机
- :warning: 内置麦克风
- :white_check_mark: 摄像头
- :warning: 显示器亮度调节（映射到`Fn+F11`、`Fn+F12`）
- :warning: 触控板（GPIO中断方式、支持多指手势）
- :white_check_mark: WiFi
- :white_check_mark: 蓝牙
- :white_check_mark: AirDrop
- :white_check_mark: Type-C PD充电
- :white_check_mark: Type-A USB接口*2
- :white_check_mark: Type-C USB接口*1
- :white_check_mark: Type-C扩展坞（USB、以太网、HDMI2.0、DP）
- :white_check_mark: Type-C雷电接口
- :white_check_mark: 电源按钮环灯及logo灯开关控制（快捷键`Fn+L`）
- :white_check_mark: 键盘背光灯亮度调节（快捷键`Fn+Space`）

------

- :red_circle: 指纹识别（因macOS系统安全性限制，此生无望）
- :red_circle: 除上述已声明可用之外的Fn组合快捷键均不可用
- :red_circle: 内置扬声器

## 5. 版本详情

|                          类型                          |                             模块                             |         版本          | 描述                             |
| :----------------------------------------------------: | :----------------------------------------------------------: | :-----------------: | :------------------------------- |
| [OpenCore](https://github.com/acidanthera/OpenCorePkg) |                              ==                              |       v0.5.9        | ==                               |
|                          ACPI（高级配置及电源接口）                          | SSDT-SBUS.aml |                     |                                  |
|                           ..                           |                                                              |                     |                                  |
|                           ..                           |                                                              |                     |                                  |
|                        Drivers（OpenCore驱动）                        | OpenRuntime.efi | OpenCore内置 | 内存寻址补丁 |
|                           ..                           | HfsPlus.efi | macOS 10.15.5镜像提取 | HFS格式支持 |
|                           ..                           | ApfsDriverLoader.efi | macOS 10.15.5镜像提取 | APFS格式支持 |
| .. | OpenUsbKbDxe.efi | OpenCore内置 | 键盘组合键支持 |
|                        Kexts（内核扩展）                        |       [Lilu.kext](https://github.com/acidanthera/Lilu)       |       v1.4.5        | Acidanthera驱动全家桶的底层依赖  |
|                           ..                           | [VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC) |       V1.1.4        | 传感器驱动依赖                   |
|                           ..                           | [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen) |       V1.4.0        | 核显&显卡驱动                    |
|                           ..                           | [USBInjectAll.kext](https://github.com/Sniki/OS-X-USB-Inject-All) |       v0.7.1        | USB万能驱动（可使用自定制USB补丁） |
|                           ..                           |   [VoodooI2C.kext](https://github.com/VoodooI2C/VoodooI2C)   |       v2.4.3        | I2C总线设备驱动                  |
|                           ..                           |                      VoodooI2CHID.kext                       | VoodooI2C.kext内置  | I2C-HID设备驱动                  |
|                           ..                           |                      SMCProcessor.kext                       | VirtualSMC.kext内置 | CPU核温度传感器驱动              |
|                           ..                           |                       SMCSuperIO.kext                        | VirtualSMC.kext内置 | IO传感器驱动                     |
|                           ..                           | [IntelMausi.kext](https://github.com/acidanthera/IntelMausi) |       V1.0.3        | Intel类千兆网卡驱动              |
|                           ..                           |   [AppleALC.kext](https://github.com/acidanthera/AppleALC)   |       V1.5.0        | 万能声卡驱动                     |
|                           ..                           |    [NVMeFix.kext](https://github.com/acidanthera/NVMeFix)    |       v1.0.2        | 为NVME硬盘增加ASPT属性来保证节电 |
|                           ..                           |  [NoTouchID.kext](https://github.com/al3xtjames/NoTouchID)   |       v1.0.3        | 指纹屏蔽驱动                     |
|                           ..                           | [VoodooPS2Controller.kext](https://github.com/acidanthera/VoodooPS2) |       v2.1.5        | 触控板驱动                       |
|                           ..                           | [AirportBrcmFixup.kext](https://github.com/acidanthera/AirportBrcmFixup) |       v2.0.7        | 非原生博通WiFi模块Airport驱动<br />添加[属性值](https://github.com/acidanthera/AirportBrcmFixup#boot-args)配置brcmfx-country=#a，以支持特殊信道及提升传输速率 |
|                           ..                           | [HibernationFixup](https://github.com/acidanthera/HibernationFixup) |       v1.3.3        | 休眠驱动                         |
|                           ..                           |  [CPUFriend.kext](https://github.com/acidanthera/CPUFriend)  |       V1.2.0        | Lilu动态电源管理数据注入插件     |
|                           ..                           |                        USBPorts.kext                         |                     |                                  |
|                           ..                           |                        USBPower.kext                         |                     |                                  |
|                           ..                           |                      [BrcmPatchRAM.kext](https://github.com/acidanthera/BrcmPatchRAM)                      | v2.5.3 |                                  |
| .. | BrcmFirmwareData.kext | BrcmPatchRAM.kext内置 | |
|                           ..                           |                  BrcmBluetoothInjector.kext                  | BrcmPatchRAM.kext内置 |                                  |
|                           ..                           |  [VoodooInput](https://github.com/acidanthera/VoodooInput)   |                     |                                  |

## 6. 启动参数说明

- 
