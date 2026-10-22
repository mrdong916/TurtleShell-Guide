# 小米 / 红米原厂车联组件

适用于原本具备 CarWith / 小米 CarLife 能力的小米、红米 ROM。3.8.3 来自小米 17；4.0.6 与 CarLife 8.6.6 来自 K70 Pro。小米链路已有实测，不代表所有型号通用。CarWith 3.0.1 是用户提供的旧版样本，已做静态适配，实机待测；该版本没有 CarlifeAdbActivity，不能按新版非 Root 自动入口使用。三个 CarWith 版本包名相同，按 ROM 选用一个，不需全部安装。

[返回总目录](../README.md)

| 组件 | 版本 / versionCode | 包名 | APK 大小 | 下载 |
| --- | --- | --- | --- | --- |
| CarWith | 3.8.3-20260203 / 103008003 | `com.miui.carlink` | 59.4 MiB | [下载](https://github.com/mrdong916/TurtleShell-Guide/releases/download/oem-components-2026-10-22/xiaomi-redmi__CarWith_3.8.3-20260203_103008003.apk) |
| CarWith | 3.0.1-20240722 / 103000001 | `com.miui.carlink` | 55.8 MiB | [下载](https://github.com/mrdong916/TurtleShell-Guide/releases/download/oem-components-2026-10-22/xiaomi-redmi__CarWith_3.0.1-20240722_103000001.apk) |
| CarWith | 4.0.6-20260603 / 104000006 | `com.miui.carlink` | 61.0 MiB | [下载](https://github.com/mrdong916/TurtleShell-Guide/releases/download/oem-components-2026-10-22/xiaomi-redmi__CarWith_4.0.6-20260603_104000006.apk) |
| CarLife | 8.6.6 / 258 | `com.baidu.carlife.xiaomi` | 79.1 MiB | [下载](https://github.com/mrdong916/TurtleShell-Guide/releases/download/oem-components-2026-10-22/xiaomi-redmi__CarLife_8.6.6_258.apk) |

APK 原样保存，未修改包名、内容或签名。安装范围由 ROM、宿主接口和签名决定；同一百度证书不代表各厂商插件可以互换。系统预装包可能限制升级或降级。

## 校验

机器可读信息见 [manifest.json](manifest.json)，包含文件大小、SHA-256 与签名证书 SHA-256。

```text
81608377d4d32256d7c0f4b1ff1070267ca889b49791eebd188827135cbbc8ba  xiaomi-redmi__CarWith_3.8.3-20260203_103008003.apk
e3f4ec221417000c25312c8ec2643094da7e53f529a89cee73d9058912b516a4  xiaomi-redmi__CarWith_3.0.1-20240722_103000001.apk
1cb9dd4f9ddf2424cd32313853f46561ea54ca6ea0465ba20752dcf35ec65394  xiaomi-redmi__CarWith_4.0.6-20260603_104000006.apk
d93b3c8c5fd8978de7219bc4d84cbe2dcb970698c7e48f27dd603300f74d0b20  xiaomi-redmi__CarLife_8.6.6_258.apk
```
