# ohscrcpy Releases

`ohscrcpy` 是 OpenHarmony 和 Android 设备的 Windows 投屏与远程控制工具。

Copyright 2026 求同存异。开发者：求同存异。
联系邮箱：`brendenaudrina6287@gmail.com`。

本仓库提供项目说明、使用方法、公开安装包、校验值和发布说明，不包含开发源码。

## 下载方式

在 [Releases](../../releases) 中下载对应版本：

- `ohscrcpy-windows-x64-<version>.zip`：解压后运行 `ohscrcpy.cmd`。
- `ohscrcpy.<version>.nupkg`：自包含 Chocolatey 离线安装包。
- `SHA256SUMS-v<version>.txt`：安装包校验值。

## Chocolatey 离线安装和更新

下载 `.nupkg` 后，在文件所在目录以管理员权限打开 PowerShell：

```powershell
choco install ohscrcpy --version <version> --source . --yes
choco upgrade ohscrcpy --version <version> --source . --yes
```

安装预览版本时追加 `--prerelease`。也可下载 ZIP，解压后直接运行
`ohscrcpy.cmd`。

安装包包含完整 Windows ZIP，安装和更新过程不访问网络。

## 使用方法

默认直接指定设备即可，启动器会自动识别 Android 或 OpenHarmony：

```powershell
ohscrcpy -s 192.168.30.82
ohscrcpy -s 192.168.20.182:5555
ohscrcpy -m "192.168.20.182:5555,192.168.20.196:5555"
ohscrcpy -m "192.168.30.82,192.168.30.91" -q Clear
```

相同 Serial 在 ADB 与 HDC 中都可用时优先 Android；ADB 为 `offline` 或
`unauthorized` 而 HDC 在线时选择 OpenHarmony。也可显式指定
`-Platform Android` 或 `-Platform OHOS`。

Android `-Quality Clear` 默认使用最大长边 1920、20 fps 和 8 Mbps。低分辨率
设备保持原生尺寸，通过更高码率减少压缩损失。

## 常用参数

| 简写 | 完整参数 | 说明 |
| --- | --- | --- |
| `-s` | `-Serial` | 单个设备 Serial |
| `-m` | `-Serials` | 最多 9 个逗号分隔的设备 |
| `-p` | `-Platform` | `Auto`、`OHOS` 或 `Android` |
| `-q` | `-Quality` | `Balanced` 或 `Clear` |
| `-c` | `-Codec` | `h264` 或 `h265` |
| `-f` | `-Fps` | 帧率 |
| `-b` | `-Bitrate` | 视频码率，单位 bps |
| `-v` | `--version` | 显示版本和开发者信息 |
| `-h` | `--help` | 显示全部参数和示例 |

完整帮助：

```powershell
ohscrcpy -h
```

## 环境要求

- Windows 10/11 x64。
- OpenHarmony 设备需要 `hdc.exe` 位于 `PATH`。
- Android 设备需要 Android SDK Platform Tools 的 `adb.exe` 位于 `PATH`。

## 安全说明

发行包会裁剪调试符号并提供 SHA-256 校验，但客户端、设备端程序和 PowerShell
脚本仍可能被反汇编、调试或逆向分析。符号裁剪只能增加分析成本，不能保证软件
无法被破解。SHA-256 用于验证下载文件完整性，不是防逆向机制。

## License

软件使用 Apache License 2.0。安装包内包含完整许可证、版权信息和第三方组件
归属文件。
