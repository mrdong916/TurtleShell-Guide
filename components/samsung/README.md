# 三星原厂车联组件

两个样本均为三星专用：三星车联是原厂宿主，百度 CarLife+ 组件是三星定制插件，应结合原有系统车联环境使用。具体手机型号、地区 ROM 及完整龟壳音画/控制兼容性尚未实机验证；不能只凭安装或原厂连接状态认定可用。

[返回总目录](../README.md)

| 组件 | 版本 / versionCode | 包名 | APK 大小 | 下载 |
| --- | --- | --- | --- | --- |
| SamsungCarLink | 3.2.00.38 / 320000038 | `com.samsung.android.carlink` | 56.4 MiB | [下载](https://github.com/mrdong916/TurtleShell-Guide/releases/download/oem-components-2026-10-22/samsung__SamsungCarLink_3.2.00.38_320000038.apk) |
| CarLife | 8.6.7 / 259 | `com.baidu.carlife.samsung` | 79.2 MiB | [下载](https://github.com/mrdong916/TurtleShell-Guide/releases/download/oem-components-2026-10-22/samsung__CarLife_8.6.7_259.apk) |

APK 原样保存，未修改包名、内容或签名。安装范围由 ROM、宿主接口和签名决定；同一百度证书不代表各厂商插件可以互换。系统预装包可能限制升级或降级。

## 校验

机器可读信息见 [manifest.json](manifest.json)，包含文件大小、SHA-256 与签名证书 SHA-256。

```text
51a0d52ab9300c8aa7d971143c75efbed5cb48ea98de56021242340f8d34a4e9  samsung__SamsungCarLink_3.2.00.38_320000038.apk
2a217e7849118cfe5cabc09a2926375850af6161ee5e47b619eec0ede2fda99e  samsung__CarLife_8.6.7_259.apk
```
