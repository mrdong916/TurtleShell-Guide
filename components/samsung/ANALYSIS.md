# 三星车联 3.2.00.38 + CarLife 8.6.7 静态分析

日期：2026-10-22。结论：找到可优先适配的原厂本地启动链，现有辅助入口仍不完整；尚无三星实机音画、输入、分辨率及重连验收。本轮只做原厂组件归档与分析，未更改龟壳运行代码、未安装手机。

## 样本及系统范围

| 项目 | 三星车联宿主 | 百度 CarLife+ 三星插件 |
| --- | --- | --- |
| 包名 | `com.samsung.android.carlink` | `com.baidu.carlife.samsung` |
| 版本 / versionCode | 3.2.00.38 / 320000038 | 8.6.7 / 259 |
| 最低 Android / target SDK | Android 15（35）/ 36 | Android 6（23）/ 30 |
| 角色 | 权限引导、车联显示、系统音频与输入、连接管理 | 百度协议、编码/传输及宿主回调 |
| 构建类型 | 三星原厂宿主 | `sdk.build.samsungsdk` |

两个 APK 均通过签名校验，原始文件摘要见 [下载与校验清单](README.md)。插件证书为百度同源证书 `509e03e3…eea78667`，宿主在代码中同时核对插件包名与这个证书。插件最低版本较低不表示新宿主可安装到旧 Android；三星系统框架和原有权限也不能由普通侧载自动补齐。手机具体型号与地区 ROM 尚未提供，不能列为全部三星机型可用。

## 已确认的原厂启动链

### 本地 / ADB 路径：建议优先验证

清单导出 `com.samsung.android.carlink.carlife.CarLifeUsbConnectionActivity`，声明动作 `com.samsung.android.carlink.carlife.USB_ADB_ATTACHED`。该 Activity 读取的是字符串 `usb_type="adb"`。

在完成原厂首次使用/权限确认、插件存在、蓝牙权限及当前连接状态检查后，`CarLifeUsbConnectionActivity.z0()` 调用原厂 `startADBConnect`。当前混淆样本的 `p044e3.a.s()` 负责构造插件请求：

| 字段 | 原厂实际值 |
| --- | --- |
| 目标 | `com.baidu.carlife.samsung/com.baidu.carlife.service.CarlifeConnectService` |
| `bindPackageName` | `com.samsung.android.carlink` |
| `bindClassName` | `com.samsung.android.carlink.carlife2.CarLifeMixingService` |
| `bindVersion` | 从已安装宿主读取的版本字符串 |
| `usb_type` | **整数 12**，与外层 Activity 的字符串不同 |
| `usb_accessory` | 字符串 `null` |
| `manufacturer` | `samsung` |

宿主通过 `startForegroundService` 启动插件，并记录 ADB 连接状态。某些状态分支会改走旧 `com.baidu.carlife` 路径或显示推荐/权限页面，因此不能只检查 Activity 返回成功。

这里的 ADB 是原厂连接模式命名；是否能不插 USB、由龟壳本机六路 TCP 直接接通，仍需在三星上验证，不能由名称推断“必须 Root”或“已经免 Shizuku 可用”。

### 原厂无线入口

另一个已导出的入口是 `com.samsung.android.carlink.connection.activity.CarLinkWirelessConnectActivity`，声明动作 `com.samsung.android.carlink.carlife.WIRELESS_CONNECT`。它同样补齐上述宿主绑定字段，但 `usb_type` 默认整数 **1**，插件据此走 Wi-Fi Direct / 蓝牙监听及发现流程；可选盒子连接参数包括 `is_box`、`ble_name`、`ble_mac`、`ble_scan_record`。

龟壳的外侧无线 CarPlay 与这个原厂无线 CarLife 流程是两段不同连接。建议先验证本机 ADB/TCP 入口，避免为了本地取流启动额外的无线发现。不要猜测、伪造盒子参数。

## 为什么只打开插件主页不够

插件 `CarlifeConnectService.connectStart()` 要求存在 `bindPackageName` 和 `usb_type`，再由 `MixPhoneStrategy.connectCastManager()` 读取宿主类和版本、绑定显示服务。`CarlifeActivity` 的普通启动则会进入 `connectVivoAndDefault()`，没有明确类型时落到 13；这不等于上述完整宿主启动链。

这份插件是 **samsungsdk**，不是旧 **samsung** 构建；旧分支自动绑定 `com.samsung.android.carlink.mixing.CarLifeMixingService` 的逻辑不能直接套用。新宿主使用的是 `carlife2.CarLifeMixingService`。

现有龟壳 `CarLifeHostApps` 对三星没有明确宿主 Activity，已知插件主页在 launcher 后备之前被选中，可能绕过原厂绑定流程。这是静态可确认的接入缺口；三星是否因此零帧仍待实测，不能直接套用 vivo 故障结论。

## 显示、声音、触摸和权限边界

`carlife2.CarLifeMixingService` 提供 `com.baidu.carlife.mixing.aidl.ICarLifeCastManager`。实现含 `prepareCast`、停止投屏、画面回调、系统音频回调和方法调用。`prepareCast` 使用插件提供的 Surface、宽、高、DPI 构建原厂显示；宿主另有虚拟显示、系统音频与触摸回调实现。插件 `MixConfig.recordFromMix()` 包含 samsungsdk，说明该路线依靠原厂宿主提供音频，而不是普通插件天然获得全局音频权限。

关键 Binder 方法通过 `Binder.getCallingUid()` 检查调用方：只允许宿主自身，或包名恰为 `com.baidu.carlife.samsung` 且证书匹配的百度插件。龟壳直接绑定后调用不能据此视为合法可用；Root / Shizuku 的存在也不能直接推导该检查会放行。优先让原厂插件正常调用原厂宿主，复用公共 CarLife 协议。

宿主在 prepareCast 中还设置插件自启动/Doze 白名单。这有助于理解原厂生命周期，但不能代替对龟壳自身后台、锁屏、暗幕的验证。

## 公共协议与授权问题

插件保留与现有公共接收通道一致的端口常量：7240 控制、8240 视频、9240 媒体音频、9241 TTS、9242 VR、9340 触摸；另有 9440 数据通道。端口常量匹配只是复用协议的依据，不代表监听已经启动或视频已到达。

`VehicleChannelUtils.checkChannel()` 要求渠道匹配 `2` 开头的 8 位数字，并排除 `20000000` 和 `20000001`。`CarlifeConnectHsgHandler.sendMDAuthenResult()` 将渠道结果与包签名检查共同用于授权结果。新版与旧版握手分支都有调用点。

因此三星也可能遇到“车机未授权”，不能因为入口补齐就宣称解决。应记录真实握手、授权结果、首帧及音频情况，不靠更换成任意车厂渠道号、伪造身份或只吞掉失败状态来宣布成功。是否触发断开、是否还有其它授权条件，需实测。

## 状态接口与可补日志

已有只读接口 `content://com.samsung.android.carlink.carlife.openprovider` 的 `get_connection_status` 仍存在，返回 0/1；该方法在受限方法身份判断之前执行，当前只读诊断可以保留。

新宿主还有 `content://com.samsung.android.carlink.common.carlinkprovider`，同名方法返回更细状态，并在部分已连接状态附带 `display_id`、`work_mode`。两个 Provider 数值语义不同，不应混用。显示 ID 只作为排查线索，后续使用显示还需核对 owner、uniqueId、displayId，不能回退手机主屏。

建议后续适配记录：宿主/插件真实版本、入口 enabled/exported/permission、选择分支和启动结果、原厂授权页是否完成、服务绑定结果、六路连接耗时、授权消息、首视频与实际编码尺寸、首 PCM/累计帧数、触摸响应、退出与重连原因。不要把完整蓝牙地址等个人信息直接写进公开报告。

## 建议实施顺序

1. 设置页优先打开已确认导出的 `CarConnectSettingActivity` 或 `CarLifeConnectCarActivity`，完成原厂准备；不要默认打开插件主屏。
2. 连接阶段优先验证原厂 `CarLifeUsbConnectionActivity` 的 ADB 请求；保留入口/权限检查、取消归属及明确失败信息，不盲启私有组件。
3. 复用龟壳现有本机六路连接、1420×480 请求及音视频链，分别确认服务就绪、授权、首帧、原厂实际输出尺寸和输入，避免改变 CarPlay 握手/退出时序。
4. 三星实机验证首次连接、拒绝/允许权限、无首帧、连续重连、锁屏、45 秒暗幕、音乐和导航播报。未完成前标记“待验证”。

不依赖 CarLife 的 ICCOA/原生路线在宿主中也有实现，但本轮未证明可供普通第三方应用调用；原厂 ICCOA 连接实现还明确记录不支持 ADB 接入。不能由“有 CarLink”推导可以直接复用小米 Root Hook。

## 证据范围

依据两个原始签名 APK 的清单及 JADX 静态调用链。宿主反编译报告 46 项错误，插件 134 项错误；以上关键入口、字段、包名/证书检查和渠道判断可读取，但不意味着完整源码已可靠恢复，动态加载、原生库和实际 ROM 行为仍需设备日志。没有三星实机测试结果。

主要定位（路径相对各自反编译 sources 根目录，混淆类名只适用于这份样本）：

- 宿主 `com/samsung/android/carlink/carlife/CarLifeUsbConnectionActivity.java`：读取字符串模式、授权及 ADB 分派。
- 宿主 `p044e3/a.java`：`s()` 构造完整插件服务请求。
- 宿主 `com/samsung/android/carlink/connection/activity/CarLinkWirelessConnectActivity.java`：无线入口和绑定参数。
- 宿主 `p057g2/c.java` 与 `p001a/a.java:m`：显示/音频 Binder 与调用方检查。
- 宿主两个 `CarLifeOpenProvider` / `CarLinkProvider`：只读状态语义。
- 插件 `BuildConfig.java`、`core/mix/MixConfig.java`、`MixPhoneStrategy.java`、`MixConnectManager.java`：构建类型、宿主绑定及连接模式。
- 插件 `service/CarlifeConnectService.java`、`core/base/config/VehicleChannelUtils.java`、`handler/CarlifeConnectHsgHandler.java`：服务请求、渠道及授权结果。
