# OPPO / 一加原厂车联组件

Car+ 与 CarLink 由一加手机提取；CarLife 包身份为 OPPO 定制版。供具备对应原厂车联服务的 OPPO / 一加 ROM 适配对照；其他同系设备并未验证。Car+ 是宿主，CarLink 是协作组件，CarLife 是插件，三者职责不同。龟壳入口已做适配，完整音画与控制尚待实机验证。

[返回总目录](../README.md)

| 组件 | 版本 / versionCode | 包名 | APK 大小 | 下载 |
| --- | --- | --- | --- | --- |
| CarPlus | 16.28.5_02f8330_202607101915 / 16028005 | `com.oplus.ocar` | 40.6 MiB | [下载](https://github.com/mrdong916/TurtleShell-Guide/releases/download/oem-components-2026-10-22/oppo-oneplus__CarPlus_16.28.5_02f8330_202607101915_16028005.apk) |
| CarLink | 16.4.2 / 1604002 | `com.heytap.opluscarlink` | 17.2 MiB | [下载](https://github.com/mrdong916/TurtleShell-Guide/releases/download/oem-components-2026-10-22/oppo-oneplus__CarLink_16.4.2_1604002.apk) |
| CarLife | 8.6.8 / 260 | `com.baidu.carlife.oppo` | 79.1 MiB | [下载](https://github.com/mrdong916/TurtleShell-Guide/releases/download/oem-components-2026-10-22/oppo-oneplus__CarLife_8.6.8_260.apk) |

APK 原样保存，未修改包名、内容或签名。安装范围由 ROM、宿主接口和签名决定；同一百度证书不代表各厂商插件可以互换。系统预装包可能限制升级或降级。

## 校验

机器可读信息见 [manifest.json](manifest.json)，包含文件大小、SHA-256 与签名证书 SHA-256。

```text
58c3138ca3a5a13bf89813166e169786e68d45b15948095bc4b6f4d2f204f54d  oppo-oneplus__CarPlus_16.28.5_02f8330_202607101915_16028005.apk
2f0d7f0a628104dc89e4c472bc748045f08effa382b074f95c26b9d6cb22d112  oppo-oneplus__CarLink_16.4.2_1604002.apk
34ba512ff2b5d88cb8a8870f3e1c86771d7452a3354bfec62aae69ec2348c781  oppo-oneplus__CarLife_8.6.8_260.apk
```
