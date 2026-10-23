# loon-adblock

为 god 定制的 Loon 去广告插件（轻量版），由 人工智障 (Muse) 维护。

## 插件

- **GodAdBlock.plugin** — v1.0.0（2026-10-23）
  - 45 条域名拦截，只拦专用广告网络：国内穿山甲、优量汇、百度联盟、快手/小米广告，海外 AdMob、Unity、AppLovin、Mintegral 等
  - 绝不拦截同时承载正文的大域（qq.com、snssdk.com、bilibili.com 等一个都没写）
  - 纯域名规则不需要 MITM 证书即可生效；另附一条 12306 开屏 Rewrite（需 MITM，未开也不报错）
  - 附带拦截 AppsFlyer、Adjust 两个归因追踪

## 安装（Loon）

插件 → 右上角 + → 粘贴下面的链接 → 添加并启用：

```
https://raw.githubusercontent.com/YikR/loon-adblock/main/GodAdBlock.plugin
```

本仓库为私人仓库时，raw 链接需要登录 GitHub 才能访问；若 Loon 拉取失败，
把仓库改公开，或直接用本地文件导入（把 GodAdBlock.plugin 发到手机，用 Loon 打开）。

## 注意

- 「看广告得奖励」的激励视频也会被拦，这是拦截原理决定的。
- 某个 App 异常时：先关掉本插件验证；确认相关后，把对应分组的行首加 `#` 注释即可单独停用。
- 抖音、今日头条、银行类 App 的广告大多无法靠域名拦截去除（域名禁止 MITM 或与正文同域），不在本插件范围内。

## 更新记录

- 2026-10-23 v1.0.0 首版：45 条规则 + 12306 开屏 Rewrite。
