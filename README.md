# ohscrcpy

[![Latest Release](https://img.shields.io/github/v/release/snowlyg/ohscrcpy-releases?display_name=tag&sort=semver)](../../releases/latest)
![Windows](https://img.shields.io/badge/Windows-10%20%7C%2011-0078D4?logo=windows)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

`ohscrcpy` 是面向 Windows 的 OpenHarmony 与 Android 设备投屏和远程控制工具，
适用于真机调试、多设备状态对照和局域网设备操作。

[下载最新版](../../releases/latest) ·
[产品介绍](https://www.lodan.me/zh-cn/products/ohscrcpy/) ·
[查看更新记录](../../releases)

## 核心能力

- **双平台统一管理**：在同一个 Windows 工具中连接 Android（ADB）和
  OpenHarmony（HDC）设备。
- **多设备投屏与控制**：同时观察和控制多台设备，支持清晰度切换、本地旋转、
  铺满显示以及鼠标和物理键盘输入。
- **自动设备路由**：根据设备在线状态自动选择 ADB 或 HDC，也可手动指定平台。
- **设备操作工具**：支持安装 HAP/APK、打开 SSH 命令行、使用 SFTP 传输文件和
  重启设备。
- **调试与诊断**：提供 Runtime logs、OHOS WebDebug 状态；远程会话启动失败时
  可一键导出脱敏诊断 ZIP。
- **本地网络工作流**：可通过局域网连接设备，减少频繁插拔数据线。
- **内置更新**：支持在应用内下载 ZIP、校验 SHA-256，并完成备份、替换和重启。

## 快速开始

### 1. 下载并启动

从 [Releases](../../releases/latest) 下载：

- `ohscrcpy-windows-x64-<version>.zip`：Windows x64 程序包。
- `SHA256SUMS-v<version>.txt`：程序包的 SHA-256 校验值。

解压 ZIP 后，推荐运行 `ohscrcpy-start.exe` 进入 Electron 控制界面。发行包还
提供以下入口：

- `ohscrcpy.exe`：控制台 launcher，适合查看设备准备、端口转发和错误日志。
- `ohscrcpyw.exe`：无控制台窗口的兼容 launcher，适合快捷方式或插件调用。

如需校验下载文件，可在 PowerShell 中执行：

```powershell
Get-FileHash .\ohscrcpy-windows-x64-<version>.zip -Algorithm SHA256
```

将输出结果与 `SHA256SUMS-v<version>.txt` 中的值进行比较。

### 2. 准备设备工具

- Android 设备：可在设置页选择托管 ADB 版本、配置 `adb.exe` 手工路径，或
  使用系统 `PATH` 中的 Android SDK Platform Tools。
- OpenHarmony 设备：可在设置页选择系统 `PATH` 中的 HDC、配置固定
  `hdc.exe` 路径，或使用发行包内提供的 `hdc\hdc.exe`。

同时确认目标设备已开启相应的调试能力，且可被 ADB 或 HDC 正常识别。

### 3. 连接设备

启动 ohscrcpy 后，可从已连接设备列表选择设备，也可以手动输入设备 IP、
`IP:port` 或 USB serial。程序会先显示已保存的设备信息，再由 ADB/HDC 在后台
刷新实时状态。

使用 SSH 命令行、SFTP 文件传输或设备重启前，需要先在设备侧启用 SSH，并在
“设置 > SSH”中配置可用的账号、端口和密码。

投屏画面获得焦点后，可以直接使用鼠标和电脑物理键盘控制当前设备。Android
支持 UTF-8 文本输入；OpenHarmony 支持英文、数字、常用符号以及 Enter、
Backspace、方向键等控制键。

## 典型场景

- 在一套工作流中调试 OpenHarmony 和 Android 真机。
- 同时投屏多台设备，对比不同系统、分辨率或版本下的 UI 状态。
- 通过局域网连接测试设备，减少 USB 数据线切换。
- 从设备操作菜单安装应用、进入远程命令行或传输测试文件。
- 通过 Runtime logs、WebDebug 状态和脱敏诊断包定位连接或启动问题。

## 设备检测与路由

默认情况下，ohscrcpy 会根据设备状态自动判断使用 Android 还是 OpenHarmony：

1. 设备只在 ADB 中在线时，使用 Android。
2. 设备只在 HDC 中在线时，使用 OpenHarmony。
3. 相同 Serial 在 ADB 与 HDC 中都可用时，优先使用 Android。
4. ADB 状态为 `offline` 或 `unauthorized`、但 HDC 在线时，使用 OpenHarmony。

也可以在启动参数中使用 `-Platform Android` 或 `-Platform OHOS` 强制指定平台。

> 下图为设备路由和多设备投屏的概念示意，不代表特定版本的实际界面。

![ohscrcpy device routing](assets/ohscrcpy-device-routing.png)

## 命令行用法

需要脚本化启动时，可以直接指定单台或多台设备：

```powershell
ohscrcpy -s <ohos-device-serial>
ohscrcpy -s <android-device-serial>:5555
ohscrcpy -m "<android-device-a>:5555,<android-device-b>:5555"
ohscrcpy -m "<ohos-device-a>,<ohos-device-b>" -q Clear
ohscrcpy -s <ohos-device-serial> -dr 270
```

常用参数：

| 简写 | 完整参数 | 说明 |
| --- | --- | --- |
| `-s` | `-Serial` | 单个设备 Serial |
| `-m` | `-Serials` | 最多 9 个逗号分隔的设备 |
| `-p` | `-Platform` | `Auto`、`OHOS` 或 `Android` |
| `-q` | `-Quality` | `Balanced` 或 `Clear` |
| `-c` | `-Codec` | `h264` 或 `h265` |
| `-cm` | `-CaptureMode` | OpenHarmony `Auto`、`Video` 或 `Screenshot` |
| `-dr` | `-DisplayRotation` | 本地显示旋转：`0`、`90`、`180` 或 `270` |
| `-f` | `-Fps` | 视频帧率 |
| `-b` | `-Bitrate` | 视频码率，单位 bps |
| `-v` | `--version` | 显示版本和开发者信息 |
| `-h` | `--help` | 显示完整帮助和示例 |

OpenHarmony 默认使用 `-CaptureMode Auto`。遇到视频兼容问题时，可以在控制
界面切换截图模式，或使用 `-CaptureMode Screenshot` 强制截图兼容链路。

`-DisplayRotation` 或 `-dr` 只旋转 Windows 本地显示，并同步修正鼠标点击、
长按和拖动的坐标映射，不会改变设备端采集方向。

Android 使用 `-Quality Clear` 时，默认最大长边为 1920、帧率为 20 fps、码率为
8 Mbps。低分辨率设备保持原生尺寸，并通过更高码率减少压缩损失。

`ohscrcpy.exe` 兼容常用 scrcpy 参数写法：数字 `-m`、带 `M/K/G` 后缀的
`-b`、`--max-fps` 和 `--window-title`。无法识别的 scrcpy 参数会静默跳过；
非数字 `-m` 仍表示 ohscrcpy 多设备列表。

## Web 远程调试

Electron 控制界面会启动或复用 OHOS WebDebug bridge，并在 HDC 设备行显示
可调试页面状态。bridge 只监听本机 `127.0.0.1:9222`；Chrome 或 DevChromeOH
的 DevTools discovery 配置由用户手动维护，程序不会自动修改浏览器配置。

## 更新

从 v0.1.5 开始，应用检测到新版本后会在左下角显示“立即更新”。内置更新器会
下载 ZIP、校验 SHA-256，并在备份当前版本后完成替换和重启。也可以始终从
[Releases](../../releases) 手动下载并解压新版本。

当前发布只支持 ZIP + SHA256SUMS。历史发布说明中可能出现 Chocolatey/nupkg
等已废弃分发方式，不代表当前版本仍支持。

## 环境要求

- Windows 10/11 x64 为主要支持环境；Windows 8.1、Windows Server 2012 R2
  使用兼容运行时。
- Android SDK Platform Tools 或设置页配置的可用 ADB（使用 Android 设备时）。
- OpenHarmony API 12 及以上，支持 armv7 32 位和 aarch64 64 位设备服务端；
  使用时需要可用的 HDC 工具。
- 支持 H.264 和 H.265 视频解码；Android 使用发行包内固定且校验过的
  scrcpy-server 3.3.3。
- SSH 服务与有效连接信息（使用 SSH、SFTP 或远程重启时）。

## 获取帮助

- 查看 [最新版本说明](../../releases/latest) 了解功能变化和已知要求。
- 使用 `ohscrcpy --help` 查看当前版本支持的完整命令行参数。
- 远程会话启动失败时，使用界面的“导出诊断包”生成脱敏诊断 ZIP。
- 通过 [Issues](../../issues) 反馈可复现的问题或功能建议。

## License

本项目发行内容使用 [Apache License 2.0](LICENSE)。安装包内包含许可证、版权信息
和第三方组件归属文件。

Copyright 2026 求同存异。开发者：求同存异。<br>
联系邮箱：`brendenaudrina6287@gmail.com`。
