# loon-adblock

为 god 定制的 Loon 去广告插件，由 人工智障 (Muse) 维护。

## 当前版本：GodAdBlock.plugin v2.2.1（2026-10-23）

插件图标与 X 工作台同款（仓库根目录 icon.png，取自工作台 apple-touch-icon，180×180）。

### 结构

**第一层 · 域名规则（2126 条，不需要 MITM）**

- v1 手工精选 45 条 + fmz200/wool_scripts 的 rejectAd 域名部分合并
- 合并后构成：DOMAIN 245 + DOMAIN-SUFFIX 1737 + DOMAIN-KEYWORD 144
  （其中 46 条关键词为 v2.1 从 blackmatrix7 Advertising 精选补强：
  admob / adserver / supersonicads / securepubads 等）
- 处理流水线：去重 → 覆盖去冗（441 条子域被父后缀覆盖后删除）→ 安全过滤
  （剔除 Adobe 激活/安全浏览类 27 条、上游误收的 temu.com 本体、格式非法 5 条）
- 受保护的正文大域（qq.com / baidu.com / bilibili.com / snssdk.com 等）本体一条不拦，
  只拦它们的广告子域名

**第二层 · 单 App 净化脚本（92 条，16 个开关，需要 MITM）**

| 开关 | 内容 | 来源 |
| --- | --- | --- |
| startup_enable | B站/爱奇艺/美团外卖/全家/嘀嗒/多点等开屏 | blackmatrix7 startup.js |
| weibo_enable | 微博信息流/发现/热搜/评论净化 + 18 条 Rewrite | fmz200 weibo 系列 |
| xhs_enable | 小红书开屏 + 搜索页净化 | fmz200 xiaohongshu.js |
| zhihu_enable | 知乎首页/信息流/回答页净化 | fmz200 zhihu.js |
| iqiyi_enable | 爱奇艺首页/播放页/搜索净化（13 条） | fmz200 cnftp.js |
| smzdm_enable | 什么值得买净化（13 条） | fmz200 smzdm 系列 |
| didi_enable | 滴滴出行净化（10 条） | fmz200 didiAds.js |
| youku_enable | 优酷净化（6 条） | fmz200 cnftp.js |
| cainiao_enable | 菜鸟裹裹净化（4 条） | fmz200 cainiao.js |
| coolapk_enable | 酷安净化（3 条） | fmz200 coolapk.js |
| taobao_enable | 淘宝开屏/弹窗净化（2 条） | fmz200 myBlockAds.js |
| jd_enable | 京东净化（1 条） | fmz200 jingdong.js |
| baidumap_enable | 百度地图净化（1 条） | fmz200 baidumap.js |
| quark_enable | 夸克净化（1 条） | fmz200 quark.js |
| umetrip_enable | 航旅纵横净化（1 条） | fmz200 umetrip_ads.js |
| x_enable | X(Twitter) **网页版**时间线/评论区去广告（1 条，实验组） | fmz200 X_ads.js |

另有 12306 开屏 Rewrite 一条。不用的 App 把对应开关关掉即可，脚本不会触发。

X 组说明：X_ads.js 是 fmz200 为网页版所写，按接口操作名
（HomeTimeline/HomeLatestTimeline/TweetDetail）与响应结构过滤广告条目。
v2.2 曾尝试把同一脚本挂到 iOS App 的 api.twitter.com / api.x.com 接口，
2026-10-23 实测无效（广告照常出现）且干扰 App 加载（时间线报
「帖子暂时无法载入」），v2.2.1 已回滚为网页版专用、App 接口移出
脚本与 MITM 名单。X 的广告与遥测域名（ads-twitter.com、p.twitter.com、
scribe.twitter.com、syndication.twitter.com）仍在域名层覆盖。

### 数据来源与取舍

- fmz200/wool_scripts（rejectAd + 插件脚本）— 主力来源，人工维护、面向 App
- blackmatrix7/ios_rule_script（startup 开屏脚本）— 其 Advertising 域名集有 29 万条，
  为多工具通用生成集，未整表导入
- privacy-protection-tools/anti-AD（11.2 万条）— 为 DNS 解析场景调优，未整表导入
  （手机规则引擎的性能与误伤权衡），仅作交叉核对：本插件 2080 条中 1467 条与之重合

## 安装（Loon）

插件 → 右上角 + → 粘贴下面的链接 → 添加并启用：

```
https://raw.githubusercontent.com/YikR/loon-adblock/main/GodAdBlock.plugin
```

第二层脚本需要 MITM：Loon 里生成 CA 证书 → iPhone 设置安装描述文件 →
通用 → 关于本机 → 证书信任设置里打开信任 → 回 Loon 开启 MITM。
没开 MITM 时第二层不生效，第一层域名规则照常工作。

更新：仓库发布新版后，在 Loon 插件页对本插件点更新即可。

## 注意

- 「看广告得奖励」的激励视频也会被拦，这是拦截原理决定的。
- 某个 App 异常时：先关对应脚本开关，再关整个插件定位；确认与某条域名规则
  相关后，把那一行行首加 `#` 注释即可单独停用。
- 抖音、今日头条、银行类 App 的广告大多无法靠域名拦截去除（域名禁止 MITM
  或与正文同域），不在本插件范围内。

## 更新记录

- 2026-10-23 v2.2.1（热修复）：X 组回滚为网页版专用——iOS App 接口
  （api.twitter.com / api.x.com）移出脚本匹配与 MITM 名单（MITM 49 → 47）。
  原因：v2.2 实测 App 广告未被过滤且时间线加载受干扰。
- 2026-10-23 v2.2.0：新增 X(Twitter) 去广告实验组（x_enable）：fmz200 X_ads.js
  同时挂 iOS App（api.twitter.com / api.x.com）与网页版时间线接口，
  过滤推广推文；MITM 主机 45 → 49。域名规则不变（2126 条）。
- 2026-10-23 v2.1.0：脚本层 36 → 91 条，新增 11 个 App 组（爱奇艺/什么值得买/
  滴滴/优酷/菜鸟/酷安/淘宝/京东/百度地图/夸克/航旅纵横，脚本均取 fmz200 仓库内
  文件，第三方脚本不收）；域名关键词 +46 条（blackmatrix7 精选）；MITM 主机 20 → 45。
- 2026-10-23 v2.0.1：插件图标换成 X 工作台同款（icon.png）；规则与脚本无变化。
- 2026-10-23 v2.0.0：规则 45 → 2080 条（多源合并+安全过滤）；新增脚本净化层
  （开屏/微博/小红书/知乎，带开关）；新增 18 条微博 Rewrite 与 MITM 主机表。
- 2026-10-23 v1.0.0 首版：45 条手工精选专用广告域名 + 12306 开屏 Rewrite。
