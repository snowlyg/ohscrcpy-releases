# 设备检测、平台路由与 WebDebug

[返回首页](../README.md)

## 设备检测与路由

ohscrcpy 会合并已保存的设备目录与 ADB/HDC 实时探测结果。设备名称、分组、排序和收藏属于用户配置，动态探测只更新在线状态，不会覆盖这些信息。

自动路由规则：

1. 设备只在 ADB 中在线时，使用 Android。
2. 设备只在 HDC 中在线时，使用 OpenHarmony。
3. 相同 Serial 在 ADB 与 HDC 中都可用时，优先使用 Android。
4. ADB 状态为 `offline` 或 `unauthorized`、但 HDC 在线时，使用 OpenHarmony。

需要固定平台时，可在图形界面选择平台，或使用 `-Platform Android` / `-Platform OHOS`。自动检测到“在线”只代表传输工具看到了设备；实际远程还需要设备 shell、服务端、端口转发和首帧链路均正常。

## 添加和编辑设备

- 可输入设备 IP、`IP:port` 或 USB serial。
- 可设置名称、已有分组或新分组；名称允许重复。
- 网络设备地址全局唯一，避免同一设备重复保存。
- 远程会话进行中仍可修改名称和分组，但地址保持锁定直到会话结束。

## Web 远程调试

Electron 控制界面会启动或复用 OpenHarmony WebDebug bridge，并在 HDC 设备行显示可调试页面状态。bridge 只监听本机 `127.0.0.1:9222`。

Chrome 或兼容浏览器的 DevTools discovery 配置由用户手动维护，程序不会自动修改浏览器设置。WebDebug 属于可选功能；其失败不会阻止投屏首帧，也不应触发远程会话重启。

## SSH 与文件传输

使用 SSH 命令行、SFTP 文件传输或设备重启前，需要在设备侧启用 SSH，并在“设置 > SSH”配置账号、端口和密码。公开问题报告中不得包含这些凭据或真实设备地址。
