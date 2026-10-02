# linnguo.github.io —— 开发者网站（GitHub Pages **用户站点**）

> 线上地址：**https://linnguo.github.io/**
> 用途：① Google Play 开发者网站 + 隐私政策 URL；② AdMob `app-ads.txt`（广告变现合规）。
> **本仓库 = 唯一真源**（2026-09-29 起从 `Block0814/store/site/` 迁入本地 clone —— 直接本地改 → commit → push，不再走网页编辑）。
> 关联项目：`d:\gitee\jystudio\Block0814`（Fillful: Block Puzzle）。

## 文件

| 文件 | 线上 URL | 用途 |
|---|---|---|
| `index.html` | `https://linnguo.github.io/` | 开发者网站首页（Play「网站」字段要能打开） |
| `privacy/index.html` | `https://linnguo.github.io/privacy/` | **隐私政策**（Fillful: Block Puzzle，无 slug，保持不动） |
| `privacy/carful/index.html` | `https://linnguo.github.io/privacy/carful/` | **隐私政策**（Carful: Car Puzzle，包名 `com.jystudio.parkingpuzzle`） |
| `app-ads.txt` | `https://linnguo.github.io/app-ads.txt` | AdMob 合规文件（**必须站点根目录**） |

`app-ads.txt` 内容 = `google.com, pub-7687550857369489, DIRECT, f08c47fec0942fa0`
（`pub-7687550857369489` = AdMob 账号 ID；**以后台「应用 → app-ads.txt」页生成的内容为准**。）

## ⚠️ 硬约束（别再踩）

- **一个域名只有一份 `app-ads.txt`，且必须在域名根**：项目仓库的 Pages 站点根永远带 `/仓库名/` 子路径（如 `linnguo.github.io/blog/app-ads.txt`）→ **AdMob 爬不到**。所以必须用本仓库（`<用户名>.github.io` 用户站点仓库）或绑自定义域名。
- 改 `app-ads.txt`：**只追加、不删旧行、永不留空 / 404**（空文件 = 声明「无授权卖家」，影响 Google 广告需求）。改完去 AdMob 后台点验证并观察转绿（爬虫约每天一次）。
- 联系邮箱须与 Play 商店列表「联系方式」一致：**`linn.guo@qq.com`**。
- 内容口径须与 Play「数据安全」表单一致（广告 ID + 大致位置 + 共享 Google AdMob + 用法数据：Firebase Analytics 关卡埋点 / Crashlytics 崩溃日志，保留 14 个月）；不写时效词 / 促销词；语言 = en-US。

## 改内容的流程（本地）

```powershell
cd D:\github\linnguo.github.io
# 编辑文件…
git add -A; git commit -m "…"; git push
# 约 1 分钟自动重发布
```

验收（**实测 HTTP 200 + 内容，不看后台状态**）：

```powershell
curl.exe -sS  https://linnguo.github.io/app-ads.txt    # 应返回上面那一行
curl.exe -sSI https://linnguo.github.io/privacy/       # 应 200
curl.exe -sS  https://linnguo.github.io/               # 首页
```

## 多项目 / 多 App 怎么共用（2026-09-29 结论）

- **能共用一个 github.io，而且同账号下只能共用**（一个 GitHub 账号只有一个 `<用户名>.github.io` 用户站点仓库 = 唯一的域名根）；Google 把本域名当「开发者网站」，同账号所有 App 共用它。
- **`app-ads.txt` 天生是「开发者级」，只有一份**：
  - 同 AdMob 账号多 App → **不用改**（`pub-…` 是**账号级** ID，一行覆盖全部 App）；
  - 多个 AdMob 账号 / 加了中介网络 → 在**同一文件里并列追加多行**（一行 = 一个「网络 + 账号」，取自各 AdMob 后台并人工拼接）；
  - 不同开发者实体 → 各开 GitHub 组织（`<org>.github.io`）或各绑自定义域名，各自一份。
- **隐私政策**：Play 允许一个 URL 给多个 App 用，但文案必须覆盖每个填它的 App；推荐 = **一 App 一子目录**（`/privacy/<app-slug>/`），改一个不影响另一个，UMP 消息里各填各的。

## 新增一个游戏（清单）

1. **隐私政策页**：复制 `privacy/index.html` → `privacy/<slug>/index.html`，替换 ① `<title>` 与 `<h1>`；② `Applies to the Android game …` 行；③ 正文 / 页脚里的游戏名与包名；④ `Last updated`；⑤ 仅当 SDK 数据实践不同才改第 2/3 节（广告 / 分析段落）。
2. **首页**：`index.html` → `Games` 区复制一张 `.game` 卡片，改名字 / 简介 / 链接到 `privacy/<slug>/`（应用上线后可加 Play 商店链接）。
3. **Play Console**：新 App → 商店列表 → 隐私政策 URL = `https://linnguo.github.io/privacy/<slug>/`。「网站」字段**不用动**（开发者账号级，全部 App 共用）。
4. **AdMob**：新 App 建广告单元；确认隐私消息覆盖到新 App，且其中的隐私政策 URL 指向它自己的页面。`app-ads.txt` **不用动**（同账号 `pub-…` 一行覆盖全部 App）。
5. **Firebase**：新 App 注册（同一项目加 App 或新项目均可）；SDK 口径不变 → 政策文案无需改。
6. **验收**：`curl.exe -sSI https://linnguo.github.io/privacy/<slug>/` → 200；老 URL（`/`、`/privacy/`、`/app-ads.txt`）仍 200。
7. **发布**：`git add -A; git commit -m "…"; git push`。

> 现阶段 `privacy/index.html`（无 slug）= **Fillful 的页面，保持不动** —— 避免改 Play / UMP 里的 URL（会触发商店列表重审）。
> 若将来想让 `/privacy/` 变成索引页、Fillful 迁到 `/privacy/fillful/`：需**同时**改 Play 的隐私政策 URL + AdMob UMP 消息里的 URL（一次性、会触发重审），可选、可推迟。

## 上线后登记（归档）

1. Play Console → 商店设置 → 联系方式 → **网站 = `https://linnguo.github.io/`**（Google 的 app-ads.txt 爬虫从这里出发）；
2. AdMob → 应用 → `app-ads.txt` 卡片确认抓取状态（应用公开发布前一直 Not Found，属正常，不阻塞投放）；
3. AdMob → 隐私权和消息 → GDPR 消息里填 `https://linnguo.github.io/privacy/`。

## 来源

- 原源文件 = `Block0814/store/site/`（2026-09-26 上线时入库）；**2026-09-29 起本仓库为唯一真源**，`store/site/` 已从项目仓库删除。
