# iOS 自助签名与安装

核查日期为 2026-11-14。本文依据 Apple、AltStore 和爱思助手的官方资料，并核对了本仓库的构建与发布脚本。真机安装、续签和升级保留数据仍待验证。

## 可行性与项目现状

AltStore Classic 和爱思助手均支持用户用自己的 Apple ID 为 IPA 签名并安装。免费个人账号可用于这种安装方式，用户需要定期续签。AltStore Classic 支持导入 IPA，AltStore PAL 使用经过 Apple 公证的应用分发流程。[AltStore 版本说明](https://faq.altstore.io/altstore-pal-v.-altstore-classic)、[爱思助手签名教程](https://www.i4.cn/news_detail_38195.html)

当前 Hisingen 的 `Package.swift` 声明 macOS 15 平台，release 流程生成 macOS 的 `.app`、DMG 和 ZIP。仓库尚无 iOS 构建目标或 IPA 发布产物。本文为后续 iOS 客户端提供分发参考，实际安装需先取得对应版本的 iOS 真机 IPA。[构建配置](../Package.swift)、[发布流程](operations/releases.md)

| 方式 | 所需环境 | 续签方式 | 核查范围 |
| --- | --- | --- | --- |
| AltStore Classic + AltServer | Mac 或 Windows 电脑、iPhone/iPad、Apple ID、IPA | AltStore 尝试后台刷新，也可手动 Refresh All | 官方支持，Hisingen 真机待测 |
| 爱思助手个人签 | 带 IPA 签名工具的电脑端、目标设备、Apple ID、IPA | 到期前重新签名并安装 | 官方教程以 Windows 操作为依据，其他桌面端的签名功能待测 |
| 爱思助手证书签名 | 证书、私钥及匹配的 provisioning profile | 按证书与描述文件有效期重新签名 | 安装范围取决于描述文件，Hisingen 真机待测 |

AltServer 可通过同一 Wi-Fi 或 USB 与设备通信。爱思助手提供 Apple ID 和证书两种签名入口。[AltServer 连接说明](https://faq.altstore.io/altstore-classic/altserver)、[爱思助手签名教程](https://www.i4.cn/news_detail_38195.html)

## 免费账号的限制

- Apple Personal Team 每台设备最多安装 3 个应用，描述文件签发后 7 天过期。AltStore Classic 本身占用一个安装名额。
- 最多注册 10 个 App ID，7 天后到期。应用扩展也可能消耗 App ID，安装前可在 AltStore 的 My Apps 中查看余量。
- 到期后需要续签才能继续打开应用。AltStore 的后台刷新属于尝试性行为，用户应检查剩余天数并在到期前手动刷新。

这些限制分别由 Apple 的个人团队说明和 AltStore 的使用文档列出。[Apple 个人团队限制](https://developer.apple.com/help/account/basics/about-your-developer-account)、[AltStore App IDs](https://faq.altstore.io/altstore-classic/app-ids)、[AltStore 刷新说明](https://faq.altstore.io/altstore-classic/your-altstore)

使用证书签名时，有效期需同时考虑证书、描述文件及撤销状态。爱思助手官方 FAQ 说明，证书被吊销也会导致应用无法打开。[爱思助手签名 FAQ](https://www.i4.cn/news_detail_40956.html)

## 安装前准备

准备面向 iPhone/iPad 真机编译的 IPA，并核对该版本要求的最低系统版本。个人签名工具需要可重新签名的安装包，爱思助手说明 App Store 下载的加密 IPA 无法再次签名安装。[爱思助手签名错误说明](https://www.i4.cn/news_detail_55902.html)

iOS/iPadOS 16 及以上运行开发签名应用时需要开启开发者模式。在设置中的「隐私与安全性 > 开发者模式」启用后，按系统提示重启并确认。安装后如提示开发者尚未受信任，在「通用 > VPN 与设备管理」中确认自己的开发者身份。[Apple 开发者模式说明](https://developer.apple.com/documentation/xcode/enabling-developer-mode-on-a-device)、[AltStore 设备信任步骤](https://faq.altstore.io/altstore-classic/how-to-install-altstore-macos)

## 使用 AltStore Classic

1. 按官方教程在电脑上安装 AltServer。Windows 用户同时按教程配置 Apple 提供的 iTunes 和 iCloud 组件。Mac 用户按当前系统对应步骤配置。
2. 用 USB 连接并解锁设备，完成电脑与设备之间的信任。在 Finder 或 iTunes 中开启 Wi-Fi 同步，以便之后无线续签。
3. 从 AltServer 的 Install AltStore 菜单选择设备，使用自己的 Apple ID 完成安装，再完成设备信任和开发者模式设置。

电脑端安装步骤随工具版本调整，以官方指南为准。[macOS 安装指南](https://faq.altstore.io/altstore-classic/how-to-install-altstore-macos)、[Windows 安装指南](https://faq.altstore.io/altstore-classic/how-to-install-altstore-windows)

4. 将 IPA 保存到设备「文件」中，在 AltStore Classic 的 My Apps 页面通过添加入口选择 IPA，按提示完成签名安装。导入入口的位置需随实际客户端版本核对。
5. 在 My Apps 检查应用与 AltStore 的到期时间。续签时保持 AltServer 运行，并通过同一 Wi-Fi 或 USB 连接电脑，点击 Refresh All。
6. AltStore 已过期时，可使用 AltServer 覆盖安装 AltStore，官方说明支持保留原有 AltStore 安装后恢复刷新。

IPA 分发、刷新与恢复方式参见官方资料。[Classic 分发说明](https://faq.altstore.io/developers/distribute-with-altstore-classic)、[刷新说明](https://faq.altstore.io/altstore-classic/your-altstore)、[AltServer 与到期恢复](https://faq.altstore.io/altstore-classic/altserver)

## 使用爱思助手

1. 打开电脑端爱思助手，连接并解锁设备，确认能读取目标设备标识 UDID。
2. 在工具箱打开「IPA 签名」或「IPA 文件签名」，添加 IPA。
3. 选择「使用 Apple ID 签名」，添加自己的账号，按提示完成双重认证，并选择该设备的 UDID。
4. 勾选 IPA 和账号，开始签名。完成后打开签名文件保存位置，将生成的 IPA 安装到对应设备。

以上步骤根据官方教程整理，实际菜单名称可能随版本变化。证书签名入口支持导入证书和描述文件，签名安装需使用匹配的材料。[爱思助手签名教程](https://www.i4.cn/news_detail_38195.html)

首次运行前完成设备信任和开发者模式设置。个人签到期前重新签名并安装。爱思助手说明个人签绑定所选 UDID，给一台设备签出的包需要为另一台设备重新签名。双重认证账号可直接按提示验证。[爱思助手签名 FAQ](https://www.i4.cn/news_detail_40956.html)、[Apple 开发者模式说明](https://developer.apple.com/documentation/xcode/enabling-developer-mode-on-a-device)

## 续签、升级与故障定位

| 现象 | 检查与处理 |
| --- | --- |
| AltStore 找不到 AltServer | 检查电脑端进程、设备信任与同一 Wi-Fi，必要时改用 USB 连接。 |
| App ID 配额耗尽 | 在 My Apps 查看 App ID 到期时间，等待足够名额到期。 |
| 爱思助手签名成功但安装失败 | 核对目标 UDID，确认选中签名后的 IPA，并查看工具给出的错误。 |
| 应用到期后无法打开 | 使用原账号和原工具刷新或重新签名安装。 |

排查入口来自各工具的官方说明。[AltStore 故障指南](https://faq.altstore.io/altstore-classic/troubleshooting-guide)、[App ID 配额](https://faq.altstore.io/altstore-classic/app-ids)、[爱思助手签名 FAQ](https://www.i4.cn/news_detail_40956.html)

项目后续需要验证同一 Apple ID、同一工具覆盖升级时的历史与设置保留，以及 Keychain 中车辆登录凭据的可读性。切换账号或签名工具后的数据迁移也需要单独实测。当前尚无 Hisingen IPA，因此这些兼容性结论待首次真机测试后补充。

## 后续 iOS 版本的验证与文档维护

首个 iOS 版本需记录 IPA 文件名、最低系统版本、设备型号、工具版本和账号类型，并验证安装、启动、车辆登录、续签、到期恢复及覆盖升级。若应用使用扩展或特殊 entitlement，还需验证个人账号下的能力可用性。AltStore 官方说明 Classic 应用可能受到系统能力限制。[Classic 能力说明](https://faq.altstore.io/altstore-pal-v.-altstore-classic)

维护者在工具安装入口、签名规则或项目 IPA 状态改变时更新本页核查日期与验证结果。GitHub release 发布脚本会自动附上本页链接，用户可从版本发布说明查看最新安装条件。[发布脚本](../Scripts/release-support.py)、[发布维护说明](operations/releases.md)
