# ohscrcpy

[![Latest Release](https://img.shields.io/github/v/release/snowlyg/ohscrcpy-releases?display_name=tag&sort=semver)](https://github.com/snowlyg/ohscrcpy-releases/releases/latest)
![Windows](https://img.shields.io/badge/Windows-10%20%7C%2011-0078D4?logo=windows)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

**在一个 Windows 工作台中投屏、控制和管理 OpenHarmony 与 Android 设备。**<br>
One Windows workspace for remote display and control across OpenHarmony and Android devices.

[GitCode 国内下载](https://gitcode.com/snowlyg/ohscrcpy-releases/releases/latest) ·
[GitHub 下载](https://github.com/snowlyg/ohscrcpy-releases/releases/latest) ·
[English](README.en.md) ·
[更新记录](https://github.com/snowlyg/ohscrcpy-releases/releases) ·
[问题反馈](https://github.com/snowlyg/ohscrcpy-releases/issues)

![ohscrcpy 多设备投屏与控制界面](assets/ohscrcpy-overview.png)

> 基于真实 ohscrcpy 界面与远程会话制作；设备身份和设备画面已通过 GPT Image 替换为虚构演示数据，并经过人工隐私复核。

## 为什么选择 ohscrcpy

| 一套工作台 | 多设备并行 | 面向真实调试流程 |
| --- | --- | --- |
| 统一连接 HDC 与 ADB 设备，自动识别可用平台，也支持手动指定。 | 同时观察和控制多台设备，支持清晰度、旋转、铺满和键鼠输入。 | 内置运行日志、WebDebug 状态、脱敏诊断包、应用安装、SSH 与 SFTP 工具。 |

- **局域网投屏与控制**：减少数据线切换，适合测试台、演示和多设备状态对照。
- **设备目录管理**：名称、分组、收藏、排序与在线状态相互独立，避免动态探测覆盖用户配置。
- **可靠更新**：应用内下载 ZIP、校验 SHA-256、备份并替换；GitCode 与 GitHub 提供相同发布文件。
- **兼容优先**：支持 H.264/H.265，并为视频兼容问题提供截图模式后备链路。

## 真实操作预览

![ohscrcpy 双设备动态演示](assets/ohscrcpy-multi-device-demo.gif)

> GIF 展示基于真实界面的两种脱敏会话状态；设备身份和画面均为虚构演示数据，不包含真实设备信息。

## 三步开始

1. 从 [GitCode Releases](https://gitcode.com/snowlyg/ohscrcpy-releases/releases/latest) 或 [GitHub Releases](https://github.com/snowlyg/ohscrcpy-releases/releases/latest) 下载 `ohscrcpy-windows-x64-<version>.zip` 和对应的 `SHA256SUMS-v<version>.txt`。
2. 解压 ZIP，运行 `ohscrcpy-start.exe`。Android 设备准备 ADB，OpenHarmony 设备准备 HDC，并在设备侧开启相应调试能力。
3. 在设备列表中选择在线设备并开始远程；需要时可在设置中配置工具路径、更新源和 SSH 连接信息。

PowerShell 校验下载文件：

```powershell
Get-FileHash .\ohscrcpy-windows-x64-<version>.zip -Algorithm SHA256
```

输出应与 `SHA256SUMS-v<version>.txt` 中的值完全一致。

## 常用场景

- 同时投屏多台设备，对比系统、分辨率或应用版本下的界面状态。
- 在同一流程中切换 OpenHarmony 与 Android 真机，无需维护两套控制工具。
- 通过鼠标和电脑物理键盘操作远端设备，并按需旋转本地显示。
- 使用运行日志、WebDebug 状态和脱敏诊断 ZIP 定位连接、准备或首帧问题。
- 从设备菜单安装 HAP/APK、打开 SSH 命令行或通过 SFTP 传输测试文件。

## 下载与更新

当前公开发布采用 **Windows x64 ZIP + SHA256SUMS**：

- GitCode 与 GitHub 发布相同的 ZIP 和校验清单。
- 应用内更新默认使用 GitCode 国内源，失败时可尝试 GitHub；也可在“设置 > 更新与日志”中手动切换。
- 内置更新器会先校验 SHA-256，再备份、替换并重启程序。
- 当前版本不依赖 Chocolatey、nupkg 或其他包管理器。

## 环境要求

- Windows 10/11 x64 为主要支持环境；Windows 8.1、Windows Server 2012 R2 使用兼容运行时。
- 使用 Android 时需要可用的 ADB；使用 OpenHarmony 时需要可用的 HDC。
- OpenHarmony API 12 及以上；设备服务端支持 armv7 与 aarch64。
- SSH、SFTP 和远程重启功能需要设备端已启用 SSH，并配置有效连接信息。

## 文档

- [命令行参数与启动示例](docs/cli.md)
- [设备检测、平台路由与 WebDebug](docs/routing-and-debugging.md)
- [故障排查与诊断信息](docs/troubleshooting.md)
- `ohscrcpy --help`：查看当前版本实际支持的完整参数。

## 获取帮助

先查看 [最新版本说明](https://github.com/snowlyg/ohscrcpy-releases/releases/latest) 和[故障排查](docs/troubleshooting.md)。如仍可复现，请在 [Issues](https://github.com/snowlyg/ohscrcpy-releases/issues) 中提供版本、平台、复现步骤和界面导出的脱敏诊断 ZIP。

> ⚠️ **隐私提醒：不要在 Issues 中提交账号、密码、设备地址、未脱敏截图或诊断原始数据。**

## License

发行内容使用 [Apache License 2.0](LICENSE)。程序包内包含许可证、版权信息和第三方组件归属文件。

Copyright 2026 求同存异。开发者：求同存异。<br>
联系邮箱：`brendenaudrina6287@gmail.com`
