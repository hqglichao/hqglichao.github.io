---
layout: post
title: Android studio wifi 连手机
date: 2022-4-20 20:00:00 +0800
categories: 技术文章
tag: [Android studio Wifi connect device]
---

## 通过 Wi-Fi 连接到设备（Android 11 及更高版本）

Android 11 及更高版本支持使用 Android 调试桥 (adb) 从工作站以无线方式部署和调试应用。例如，您可以将可调试应用部署到多台远程设备，而无需通过 USB 实际连接设备。这样就可以避免常见的 USB 连接问题，例如驱动程序安装方面的问题。

在开始使用无线调试功能之前，您必须先完成以下步骤：

1. 确保您的工作站和设备已连接到同一无线网络。
2. 确保您的设备搭载的是 Android 11 或更高版本。如需了解详情，请参阅[查看和更新 Android 版本](https://support.google.com/android/answer/7680439)。
3. 确保您具有 Android Studio Bumblebee。您可以在[此处](https://developer.android.com/studio)下载。
4. 在工作站上，更新到最新版本的 [SDK 平台工具](https://developer.android.com/studio/releases/platform-tools)。

如需使用无线调试功能，您必须使用二维码或配对码将设备与工作站配对。您的工作站和设备必须连接到同一无线网络。如需连接到您的设备，请按以下步骤操作：

1. 在设备上启用开发者选项：

   1. 在设备上，找到**版本号**选项。在以下设备上，您可以在下面这些位置找到此选项：

      | 设备                         | 设置                                                         |
      | :--------------------------- | :----------------------------------------------------------- |
      | Google Pixel                 | **设置** > **关于手机** > **版本号**                         |
      | Samsung Galaxy S8 及更高版本 | **设置** > **关于手机** > **软件信息** > **版本号**          |
      | LG G6 及更高版本             | **设置** > **关于手机** > **软件信息** > **版本号**          |
      | HTC U11 及更高版本           | **设置** > **关于** > **软件信息** > **更多** > **版本号**或**设置** > **系统** > **关于手机** > **软件信息** > **更多** > **版本号** |
      | 一加 5T 及更高版本           | **设置** > **关于手机** > **版本号**                         |

   2. 连续点按**版本号**选项七次，直到您看到消息 `You are now a developer!` 这会在您的手机上启用开发者选项。

2. 在您的设备上启用通过 Wi-Fi 调试：

   1. 在设备上找到**开发者选项**。在以下设备上，您可以在下面这些位置找到此选项：

      | 设备                                                         | 设置                                 |
      | :----------------------------------------------------------- | :----------------------------------- |
      | Google Pixel、一加 5T 及更高版本                             | **设置** > **系统** > **开发者选项** |
      | Samsung Galaxy S8 及更高版本、LG G6 及更高版本、HTC U11 及更高版本 | **设置** > **开发者选项**            |

   2. 在**开发者选项**中，向下滚动到**调试**部分，然后开启**无线调试**。在**要允许通过此网络进行无线调试吗？**弹出式窗口中，选择**允许**。

   3. 打开 Android Studio，然后从运行配置下拉菜单中选择 **Pair Devices Using Wi-Fi**。

      <img src="https://picgo-1307686581.cos.ap-shanghai.myqcloud.com/github/hqglichao/imagesimage-20220421204519681.png" style="zoom: 50%;" />

   4. 在您的设备上，点按**无线调试**（开发者选项->无线调试），然后配对您的设备：

      <img src="https://picgo-1307686581.cos.ap-shanghai.myqcloud.com/github/hqglichao/imagesimage-20220421204826659.png" style="zoom:30%;" />  
      

## 通过 Wi-Fi 连接到设备（Android 10 及更低版本）

一般情况下，adb 通过 USB 与设备进行通信，但您也可以通过 Wi-Fi 使用 adb。如要连接到搭载 Android 10 或更低版本的设备，您必须通过 USB 执行一些初始步骤，如下所述：

1. 将 Android 设备和 adb 主机连接到这两者都可以访问的同一 Wi-Fi 网络。请注意，并非所有接入点都适用；您可能需要使用防火墙已正确配置为支持 adb 的接入点。

2. 如果您要连接到 Wear OS 设备，请关闭手机上与该设备配对的蓝牙。

3. 使用 USB 线将设备连接到主机。

4. 设置目标设备以监听端口 5555 上的 TCP/IP 连接。

   ```
   adb tcpip 5555
   ```

5. 拔掉连接目标设备的 USB 线。

6. 找到 Android 设备的 IP 地址。例如，对于 Nexus 设备，您可以在**设置** > **关于平板电脑**（或**关于手机**）> **状态** > **IP 地址**下找到 IP 地址。或者，对于 Wear OS 设备，您可以在**设置** > **WLAN 设置** > **高级** > **IP 地址**下找到 IP 地址。

7. 通过 IP 地址连接到设备。

   ```
   adb connect device_ip_address:5555
   ```

8. 确认主机已连接到目标设备：

   ```
   $ adb devices
   List of devices attached
   device_ip_address:5555 device
   ```

现在，您可以开始操作了！

如果 adb 连接断开：

1. 确保主机仍与 Android 设备连接到同一个 WLAN 网络。

2. 通过再次执行 `adb connect` 步骤重新连接。

3. 如果上述操作未解决问题，重置 adb 主机：

   ```
   adb kill-server
   ```

   然后，从头开始操作。

> 参考文献：[https://developer.android.com/studio/command-line/adb](https://developer.android.com/studio/command-line/adb)