# loon-adblock

为 god 定制的 Loon 去广告插件，由 人工智障 (Muse) 维护。

## 当前版本：GodAdBlock.plugin v2.0.0（2026-10-23）

### 结构

**第一层 · 域名规则（2080 条，不需要 MITM）**

- v1 手工精选 45 条 + fmz200/wool_scripts 的 rejectAd 域名部分合并
- 合并后构成：DOMAIN 245 + DOMAIN-SUFFIX 1737 + DOMAIN-KEYWORD 98
- 处理流水线：去重 → 覆盖去冗（441 条子域被父后缀覆盖后删除）→ 安全过滤
  （剔除 Adobe 激活/安全浏览类 27 条、上游误收的 temu.com 本体、格式非法 5 条）
- 受保护的正文大域（qq.com / baidu.com / bilibili.com / snssdk.com 等）本体一条不拦，
  只拦它们的广告子域名

**第二层 · 单 App 净化脚本（36 条，带开关，需要 MITM）**

| 开关 | 内容 | 来源 |
| --- | --- | --- |
| startup_enable | B站/爱奇艺/美团外卖/全家/嘀嗒/多点等开屏 | blackmatrix7 startup.js |
| weibo_enable | 微博信息流/发现/热搜/评论净化 + 18 条 Rewrite | fmz200 weibo 系列 |
| xhs_enable | 小红书开屏 + 搜索页净化 | fmz200 xiaohongshu.js |
| zhihu_enable | 知乎首页/信息流/回答页净化 | fmz200 zhihu.js |

另有 12306 开屏 Rewrite 一条。

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

- 2026-10-23 v2.0.0：规则 45 → 2080 条（多源合并+安全过滤）；新增脚本净化层
  （开屏/微博/小红书/知乎，带开关）；新增 18 条微博 Rewrite 与 MITM 主机表。
- 2026-10-23 v1.0.0 首版：45 条手工精选专用广告域名 + 12306 开屏 Rewrite。
