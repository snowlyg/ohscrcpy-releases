# 命令行参数与启动示例

[返回首页](../README.md) · [English summary](../README.en.md)

通常推荐运行 `ohscrcpy-start.exe` 使用图形控制界面。需要脚本化启动、固定显示参数或集成其他工具时，可以使用 `ohscrcpy.exe`。

## 启动入口

- `ohscrcpy-start.exe`：Electron 控制界面，适合日常使用。
- `ohscrcpy.exe`：控制台 launcher，适合查看设备准备、端口转发和错误输出。
- `ohscrcpyw.exe`：无控制台窗口的兼容 launcher，适合快捷方式或外部工具调用。

## 示例

```powershell
ohscrcpy -s <ohos-device-serial>
ohscrcpy -s <android-device-serial>:5555
ohscrcpy -m "<android-device-a>:5555,<android-device-b>:5555"
ohscrcpy -m "<ohos-device-a>,<ohos-device-b>" -q Clear
ohscrcpy -s <ohos-device-serial> -dr 270
```

示例中的 Serial、地址和端口均为占位符，请替换为本机工具实际识别到的设备标识。不要把真实设备地址写入公开日志或问题报告。

## 常用参数

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

OpenHarmony 默认使用 `-CaptureMode Auto`。遇到视频兼容问题时，可在控制界面切换截图模式，或使用 `-CaptureMode Screenshot` 强制截图兼容链路。

`-DisplayRotation` 只旋转 Windows 本地显示，并同步修正鼠标点击、长按和拖动的坐标映射，不改变设备端采集方向。

Android 使用 `-Quality Clear` 时，默认最大长边为 1920、帧率为 20 fps、码率为 8 Mbps。低分辨率设备保持原生尺寸，并通过较高码率减少压缩损失。

`ohscrcpy.exe` 兼容常用 scrcpy 参数写法：数字 `-m`、带 `M/K/G` 后缀的 `-b`、`--max-fps` 和 `--window-title`。无法识别的 scrcpy 参数会静默跳过；非数字 `-m` 仍表示 ohscrcpy 多设备列表。

不同版本的参数可能变化，请始终以当前程序的 `ohscrcpy --help` 输出为准。
