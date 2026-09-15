# zhryhzs.github.io · 从零到上线的完整手册

这是 **zhryhzs** 的个人博客源码，用 **Jekyll + Minima** 主题写，托管在 **GitHub Pages** 上。
全部免费，不需要买服务器、不需要备案。

| 项目 | 值 |
| --- | --- |
| GitHub 用户名 | `zhryhzs` |
| 仓库名 | **`zhryhzs.github.io`**（「用户站点」，网址就是域名根目录） |
| 仓库地址 | <https://github.com/zhryhzs/zhryhzs.github.io> |
| **上线后的网址** | **<https://zhryhzs.github.io/>** |
| `_config.yml` 的 `url` | `https://zhryhzs.github.io` |
| `_config.yml` 的 `baseurl` | `""` ← 用户站点必须留空 |

> 仓库原本叫 `zhryhzs-one`，2026-09-15 改名为 `zhryhzs.github.io`。
> 改名的效果是：网址从 `https://zhryhzs.github.io/` 变成干净的
> `https://zhryhzs.github.io/`。
>
> ⚠️ **改名必须同时改两处**：仓库名 + `_config.yml` 的 `baseurl`（`/zhryhzs-one` → `""`）。
> 只改一个的话，网页能打开但**样式全丢**（白底黑字），因为 CSS 链接还指向已经不存在的旧路径。

> 这份 README 是给自己看的操作手册，所以写得非常啰嗦——每一步都写了「点哪里、看到什么、
> 卡住了怎么办」。第一次照着做一遍，以后只需要看第 7 章（写文章）。

---

## 目录

- [0. 三分钟总览](#0-三分钟总览)
- [1. 原理：这个博客是怎么工作的](#1-原理这个博客是怎么工作的)
- [2. 名词表（看不懂就回来查）](#2-名词表看不懂就回来查)
- [3. 准备清单](#3-准备清单)
- [4. 路线 A：分支部署（推荐，零配置，5 分钟）](#4-路线-a分支部署推荐零配置5-分钟)
- [5. 路线 B：GitHub Actions 部署（可选升级）](#5-路线-bgithub-actions-部署可选升级)
- [6. 每个文件是干什么的](#6-每个文件是干什么的)
- [7. 写一篇文章（最常用的一章）](#7-写一篇文章最常用的一章)
- [8. 个性化改造（可直接复制代码）](#8-个性化改造可直接复制代码)
- [9. 本地预览（可选，需要装 Ruby）](#9-本地预览可选需要装-ruby)
- [10. 排错手册](#10-排错手册)
- [11. Git 命令速查](#11-git-命令速查)
- [12. 费用与限制](#12-费用与限制)
- [13. 上线检查清单](#13-上线检查清单)

---

## 0. 三分钟总览

整个流程就 5 步：

| 步骤 | 做什么 | 大概耗时 |
| --- | --- | --- |
| 1 | 确认 GitHub 用户名是 `zhryhzs` | ✅ 已完成 |
| 2 | 新建仓库 `zhryhzs.github.io`（Public） | ✅ 已完成 |
| 3 | 把本文件夹里的文件全部上传上去 | 2 分钟 |
| 4 | Settings → Pages → Source 选 `Deploy from a branch` → `main` → `/(root)` → Save | 1 分钟 |
| 5 | 等 1~2 分钟，打开 <https://zhryhzs.github.io/> | — |

**如果你只想最快看到效果**：照上面 5 步做，然后跳到第 7 章学写文章。
第 4 章是这 5 步的详细展开版。

---

## 1. 原理：这个博客是怎么工作的

### 1.1 它不是「网站程序」，而是一堆提前做好的静态网页

普通博客（比如 WordPress）需要一台一直开着的服务器 + 数据库，有人访问就现场查数据库、
拼页面。这套东西要花钱、要维护、还容易被攻击。

GitHub Pages 走的是另一条路：**在你自己电脑上（或 GitHub 的服务器上）把 Markdown 文章
提前"翻译"成 HTML 网页，然后把一堆 `.html` 文件直接放到网上。** 访客打开网页时，服务器
只是把做好的文件递过去，什么都不用算。

好处：极快、极稳定、几乎不可能被黑、完全免费。
代价：不能有评论/登录/后台这类"动态"功能（要用的话得接第三方服务，见 8.5、8.6）。

### 1.2 数据是怎么流动的

```
你写 Markdown（_posts/2026-09-15-hello-world.md）
        │
        │  ① 上传 / git push
        ▼
GitHub 仓库（github.com/zhryhzs/zhryhzs.github.io）
        │
        │  ② GitHub 自动运行 Jekyll：套主题、生成 HTML
        ▼
        _site/  （生成出来的纯静态网页，你不用管这个目录）
        │
        │  ③ 自动发布
        ▼
https://zhryhzs.github.io/  ← 全世界的访客看到的页面
```

**关键点：你永远只编辑下划线开头的目录（`_posts`）、`_config.yml`、几个 `.md` 页面。
其他东西都是自动生成的，不要手动改。**

### 1.3 用户站点 vs 项目站点（我们的仓库是用户站点）

GitHub Pages 有两种站点：

| 类型 | 仓库名 | 网址 | `baseurl` 怎么填 |
| --- | --- | --- | --- |
| **用户站点**（我们在用） | 必须正好是 `zhryhzs.github.io`（和用户名完全一致） | `https://zhryhzs.github.io/` | 留空 `""` |
| 项目站点 | 任意名字，例如 `my-notes` | `https://zhryhzs.github.io/my-notes/` | 必须是 `/my-notes` |

**两者唯一的区别：项目站点的网址多了一段仓库名。** 仓库原来叫 `zhryhzs-one` 时属于
项目站点，必须写 `baseurl: "/zhryhzs-one"`；改名成 `zhryhzs.github.io` 之后变成用户站点，
`baseurl` 必须**改回空字符串**，否则 CSS、导航、图片的链接会指向
`/zhryhzs-one/...` 这个已经不存在的路径——典型症状就是**网页能打开，但完全没有样式**。

每个账号**只能有一个**用户站点仓库，所以这个仓库名不能再给别的项目用了。

---

## 2. 名词表（看不懂就回来查）

| 名词 | 一句话解释 |
| --- | --- |
| **GitHub** | 存放代码和文件的网站，也提供免费网页托管（Pages） |
| **仓库 Repository** | 一个项目文件夹，存在 GitHub 上。这里就是整个博客 |
| **Public / Private** | 公开 / 私有。免费账号的 Pages **必须**用 Public 仓库 |
| **commit（提交）** | 「保存一次改动」，相当于游戏存档点，带说明文字 |
| **branch（分支）** | 平行版本线。默认主分支现在叫 `main` |
| **push（推送）** | 把本地提交上传到 GitHub |
| **GitHub Pages** | GitHub 提供的静态网页托管服务，就是本博客用的 |
| **Jekyll** | 把 Markdown「翻译」成 HTML 的生成器，GitHub 内置支持 |
| **主题 Theme** | 一套现成的网页外观。本项目用 `minima`（Jekyll 官方默认主题） |
| **Markdown** | 用纯文本写排版的语法，比如 `**加粗**`、`# 标题` |
| **Front Matter** | 文件最上面被两行 `---` 夹住的配置区，告诉 Jekyll 这是什么 |
| **GitHub Actions** | GitHub 的自动化机器人。路线 B 用它来构建部署 |
| **`_config.yml`** | 站点总配置文件（YAML 格式，缩进敏感） |

---

## 3. 准备清单

开始之前确认：

- [ ] 一个 GitHub 账号，**用户名是 `zhryhzs`**
- [ ] 网络能正常访问 github.com（打不开的话见 10.9）
- [ ] 本文件夹 `zhryhzs.github.io` 里的文件都在（不用 Git，网页上传就够了）

**不需要**：服务器、域名、备案、Ruby、Git、Node.js。
**可选安装**：Git（方便以后更新文章）、Ruby（本地预览）。

> 如果你的 GitHub 用户名不是 `zhryhzs`：把 `_config.yml`、`about.md`、本 README 和
> `.github/workflows/jekyll.yml` 里出现的 `zhryhzs` 全部替换成你的真实用户名，
> 并且仓库名也用 `<你的用户名>.github.io`。

---

## 4. 路线 A：分支部署（推荐，零配置，5 分钟）

> 这是 GitHub 官方文档对普通 Jekyll 站点的推荐做法：不用管构建脚本，推上去就自动发布。
> 参考：<https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site>

### A1. 注册或登录，并核对用户名

1. 打开 <https://github.com>，登录（没有账号就点 **Sign up** 注册）。
2. 右上角头像 → **Your profile**，看浏览器地址栏：`https://github.com/` 后面那串就是你的用户名。
3. **确认它就是 `zhryhzs`**（全小写）。如果不一样，先按第 3 章末尾的说明替换所有文件里的用户名。

### A2. 新建仓库

1. 打开 <https://github.com/new>（或在任意页面右上角 **+** → **New repository**）。
2. **Repository name** 填：

   ```
   zhryhzs.github.io
   ```

   必须和用户名完全一致（全小写、结尾不能有空格）。仓库已建好且命名正确，这一步无需再做。
3. **Description**（可选）填：`我的个人博客`。
4. 可见性选 **Public**。
   ⚠️ 选 Private 的话，免费账号开启 Pages 会失败（GitHub Pro 才支持私有仓库发布）。
5. **Initialize this repository with** 下面的三个勾（Add a README file / Add .gitignore /
   Choose a license）**一个都不要勾**。我们要上传自己的文件，勾了反而要先删。
6. 点绿色按钮 **Create repository**。

创建成功后你会看到一个空仓库页面，写着 "Quick setup — if you've done this kind of thing before"，
并且有一个蓝色的 **uploading an existing file** 链接。下一步就用它。

### A3. 上传文件

1. 在仓库首页点 **uploading an existing file** 链接。
   （找不到就点 **Add file** ▾ → **Upload files**。）
2. 打开 Windows 资源管理器，进入：

   ```
   D:\开山鼻祖\DeepSeek Harness_files\zhryhzs.github.io
   ```
3. **全选里面的所有内容**（Ctrl+A），拖到浏览器中间那个虚线上传区域。
   ⚠️ 注意两点：
   - 拖的是**文件夹里面的内容**（`_posts`、`assets`、`index.md`、`_config.yml` …），
     **不是** `zhryhzs.github.io` 这个文件夹本身。拖错了会变成
     `仓库/zhryhzs.github.io/index.md`，网站就打不开。
   - `_posts`、`assets` 这两个子文件夹要一起拖进去，里面的文章和样式才会跟着上传。
   - 本项目的 `.github` 文件夹里放的是 Actions 脚本。走**路线 A 的话可以不上传它**（见 A5）。
4. 等文件列表全部出现、进度条走完（12 个文件都传完）。
5. 页面下方的 **Commit changes** 区域：
   - 第一个输入框填提交说明，例如：`初始化博客`
   - 第二个框（Extended description）留空
   - 保持 **Commit directly to the `main` branch** 选中
6. 点绿色按钮 **Commit changes**。

上传完成后，仓库首页应该能看到这些文件：

```
_posts/     assets/     _config.yml     Gemfile     index.md
about.md    404.html    README.md       .gitignore
```

> **`.github` 文件夹在浏览器里看不到？** 不用管。文件夹名以 `.` 开头，GitHub 网页默认
> 折叠显示，只要你上传时确实拖进去了，它就存在。

### A4. 开启 GitHub Pages

1. 在仓库页面，点上方菜单栏最右边的 **Settings**（齿轮图标）。
   如果没看到，点 `...` 展开菜单再找。
2. 左侧边栏往下滚，找到 **Code, planning, and automation** 分区，点 **Pages**。
3. 右侧 **Build and deployment** → **Source** 下拉框：
   - 选 **Deploy from a branch**（不要选 GitHub Actions）
4. 下面出现两个新的下拉框：
   - **Branch**：选 **`main`**
   - 右边的文件夹框：选 **`/(root)`**
5. 点 **Save**。
6. 页面顶部会出现一条蓝色提示，几秒后刷新，会变成：

   > **Your site is live at https://zhryhzs.github.io/**

   以及一个 **Visit site** 按钮。

### A5. （走路线 A 必做）删掉 Actions 脚本，免得出现红色失败

本仓库里有一个 `.github/workflows/jekyll.yml`，它是"路线 B"的自动构建脚本。
如果你已经把 Source 设成 `Deploy from a branch`，这个脚本运行时找不到 Actions 版的 Pages
配置，会在 Actions 页面留下一堆红色 ❌，看着很慌（其实不影响网站显示）。

要么忽略它，要么干脆删掉：

1. 仓库首页点进 `.github` 文件夹 → `workflows` → `jekyll.yml`
2. 打开文件后，点右上角 **⋯**（三个点）→ **Delete file**
3. 下方 **Commit changes** → 点绿色按钮确认

（以后想升级成路线 B，照第 5 章把文件重新加回来即可。）

### A6. 验证是否真的上线

1. 打开 <https://zhryhzs.github.io/>
2. 应该看到「成功了」标题、两篇文章《你好，世界》和《如何写一篇新文章（Markdown 速查）》、
   顶部有「关于」导航。
3. 点进文章 → 能正常显示中文和代码块 → 说明一切正常。

**第一次可能要等 1~2 分钟**，如果还是 404，别急着改配置，先等 2 分钟强制刷新
（Ctrl+F5），再去看第 10 章。

---

## 5. 路线 B：GitHub Actions 部署（可选升级）

什么时候需要路线 B？

- 想用 GitHub Pages 官方**不内置**的 Jekyll 插件（比如 `jekyll-paginate-v2`、`jekyll-archives`）
- 想换成 VitePress / Hugo / Next.js 这类别的生成器
- 想看详细的构建日志、想控制构建过程

普通写博客**不需要**，路线 A 更省心。

如果确实要切换：

1. 确认仓库里有 `.github/workflows/jekyll.yml`（本项目自带，若已删除就按内容重新创建：
   仓库首页 **Add file** → **Create new file**，文件名输入框里直接打
   `.github/workflows/jekyll.yml`（打 `/` 会自动建目录），把本地文件内容粘进去，提交）。
2. 仓库 **Settings** → **Pages** → **Source** 改成 **GitHub Actions**，无需点 Save（自动生效）。
3. 上方 **Actions** 标签页 → 左侧选「构建并部署 Jekyll 博客」→ 看最新一次运行：
   - 绿色 ✅ = 成功，点进去能看到 `build` 和 `deploy` 两个作业
   - 红色 ❌ = 失败，点进失败的那一步看日志（把报错贴给我也能帮你看）
4. 如果是先上传文件、后改 Source，需要再触发一次：**Actions** → 左侧工作流 →
   右侧 **Run workflow** ▾ → **Run workflow**。

两条路线对比：

| | 路线 A（分支） | 路线 B（Actions） |
| --- | --- | --- |
| 配置难度 | 一个下拉框 | 需要一个 YAML 脚本 |
| 构建耗时 | 较快（GitHub 内部） | 1~2 分钟（要装 Ruby 依赖） |
| 插件限制 | 只能用官方白名单 | 无限制 |
| 出错排查 | 只能看 Pages 构建报错邮件 | 有完整日志，好排查 |
| 推荐场景 | **个人博客（就用这个）** | 复杂站点 / 自定义构建 |

工作流文件里用到的 Action 都是官方维护的：
`actions/checkout@v4`、`ruby/setup-ruby@v1`、`actions/configure-pages@v5`、
`actions/upload-pages-artifact@v3`、`actions/deploy-pages@v4`。

---

## 6. 每个文件是干什么的

```
zhryhzs.github.io/
├── _config.yml                  ★ 站点总配置：标题、网址、主题、插件
├── index.md                     ★ 首页（自动列出所有文章，一般不用改）
├── about.md                     ★ 「关于」页面
├── 404.html                     ★ 访问不存在的网址时显示的页面
├── _posts/                      ★★★ 所有文章放这里，写博客只动这个目录
│   ├── 2026-09-15-hello-world.md
│   └── 2026-09-15-how-to-write-a-post.md
├── assets/
│   ├── main.scss                ★ 自定义样式（字体、字号、颜色）
│   └── images/                  文章配图放这里
├── Gemfile                      本地预览用的依赖清单（线上不需要）
├── .gitignore                   告诉 Git 哪些临时文件不用上传
├── README.md                    本文件
└── .github/workflows/jekyll.yml 路线 B 的自动部署脚本（路线 A 可删）
```

加 ★ 的是你会经常动到的，其余的基本不用碰。

### 6.1 `_config.yml` 逐行讲解

YAML 格式对**缩进非常敏感**：只能用空格（不能用 Tab），同级缩进必须对齐。

```yaml
title: 成功了                    # 显示在网页左上角、浏览器标签页
description: zhryhzs 的个人博客  # 搜索引擎里显示的站点简介
author: zhryhzs                  # 作者名
lang: zh-CN                      # 网页语言，中文站固定写这个

url: "https://zhryhzs.github.io" # 站点完整地址，末尾不要加 /
baseurl: ""                      # ★ 用户站点留空；若仓库是项目站点，这里写 "/仓库名"

timezone: Asia/Shanghai          # 时区，保证文章日期不跑偏

theme: minima                    # 使用的主题

plugins:                         # 启用的插件（这三个 GitHub Pages 原生支持）
  - jekyll-feed                  #   → 生成 /feed.xml 订阅源
  - jekyll-seo-tag               #   → 自动生成 SEO/分享卡片标签
  - jekyll-sitemap               #   → 生成 /sitemap.xml，方便被搜索引擎收录

show_excerpts: true              # 首页显示每篇文章的摘要
permalink: /:year/:month/:day/:title/   # 文章网址格式

header_pages:                    # 顶部导航栏显示哪些页面
  - about.md
```

改完 `_config.yml` **必须重新部署**才生效：路线 A 下就是重新提交一次文件（哪怕只改一个字），
或者 Settings → Pages 里随便重存一下。

### 6.2 `assets/main.scss` 是最容易出效果的改造点

它长这样：

```scss
---
---            ← 这两行横杠不能删，Jekyll 靠它才知道要编译这个文件

@import "minima";   ← 引入主题自带样式

/* 你的自定义 CSS 写在这下面，会覆盖上面的默认样式 */
```

改字体、字号、颜色都在这一行后面加 CSS（见 8.3、8.4）。

---

## 7. 写一篇文章（最常用的一章）

### 7.1 命名规则（最容易出错的地方）

在 `_posts/` 里新建文件，文件名**必须**是：

```
YYYY-MM-DD-英文小写标题.md
```

对照表：

| 文件名 | 结果 |
| --- | --- |
| `2026-10-01-my-first-note.md` | ✅ 正确 |
| `2026-10-1-my-note.md` | ❌ 月份必须两位数（`01`） |
| `2026-10-01-我的笔记.md` | ⚠️ 能跑，但网址会出现一串乱码百分号编码，不推荐 |
| `2026-10-01 my note.md` | ❌ 不能有空格 |
| `my-note.md` | ❌ 缺日期，Jekyll 直接忽略这个文件 |
| `_posts/2026-10-01-note.markdown` | ✅ `.markdown` 后缀也可以 |

**中文标题写在文件内容里的 `title:` 字段**，不要写进文件名。

### 7.2 Front Matter（文件开头那段配置）

每篇文章最上面必须有这一段，两行 `---` 一个都不能少：

```yaml
---
layout: post
title: "文章的中文标题"
date: 2026-10-01 20:30:00 +0800
categories: 学习笔记
tags: [Jekyll, 教程]
---
```

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `layout` | ✅ | 固定写 `post`（文章用）。页面用 `page`，首页用 `home` |
| `title` | ✅ | 文章标题，中文随意，建议用英文引号包起来 |
| `date` | ✅ | 决定文章在列表里的排序。`+0800` 是北京时间，别漏 |
| `categories` | ❌ | 分类，会出现在文章网址里（用中文会产生编码，介意就写英文） |
| `tags` | ❌ | 标签，写在方括号里，逗号分隔 |

**没有 Front Matter 的 `.md` 文件不会被当成文章**，只会被原样复制到网站上。
**日期写错（比如写成 2027 年）文章会排在列表最前面**，看起来像"置顶"。

### 7.3 方式一：网页上写（不用装任何软件）

1. 打开仓库 → 进 `_posts` 文件夹
2. 右上角 **Add file** ▾ → **Create new file**
3. 在最上面的文件名框里输入完整文件名，例如：

   ```
   2026-10-01-my-first-note.md
   ```

   注意 `_posts/` 已经在路径里了，这里只填文件名。
4. 下面的大文本框里，粘贴 7.2 的 Front Matter 模板，然后接着写正文。
5. 拉到页面底部 **Commit changes** → 填说明（比如 `新增文章：我的第一篇笔记`）→
   点绿色 **Commit changes**。
6. 等 1~2 分钟，刷新博客首页即可看到。

> 想改图片：先在本地把图片准备好，用 **Add file → Upload files** 传进
> `assets/images/`，然后在文章里写 `![说明](/assets/images/图片名.png)`。

### 7.4 方式二：命令行写（装了 Git 之后更顺手）

```bash
# 1. 进入项目目录
cd "D:\开山鼻祖\DeepSeek Harness_files\zhryhzs.github.io"

# 2. 用记事本新建文章（或直接用编辑器）
notepad _posts\2026-10-01-my-first-note.md

# 3. 看一下改了什么
git status

# 4. 保存这次改动
git add .
git commit -m "新增文章：我的第一篇笔记"

# 5. 上传到 GitHub（会自动触发重新部署）
git push
```

### 7.5 修改 / 删除文章

- **改**：打开 `_posts/` 里对应文件 → 点右上角铅笔图标 ✏️ → 改完 Commit changes
- **删**：打开文件 → 右上角 **⋯** → **Delete file** → Commit changes
- **改网址**：`permalink` 由日期和文件名决定，改了文件名等于换了网址，旧链接会失效

---

## 8. 个性化改造（可直接复制代码）

### 8.1 改站点标题、简介

编辑 `_config.yml` 的 `title` 和 `description`：

```yaml
title: 开山鼻祖的博客
description: 记录学习、折腾与所得。
```

### 8.2 加一个新页面（比如「归档」「友情链接」）

1. 在仓库根目录新建 `links.md`：

   ```markdown
   ---
   layout: page
   title: 友情链接
   permalink: /links/
   ---

   这里写页面内容，支持 Markdown。
   ```

2. 编辑 `_config.yml`，把它加到导航栏：

   ```yaml
   header_pages:
     - about.md
     - links.md
   ```

### 8.3 深色模式（跟随系统自动切换）

编辑 `assets/main.scss`，在 `@import "minima";` **下面**追加：

```scss
@media (prefers-color-scheme: dark) {
  body        { background-color: #16181d; color: #d6d8dc; }
  a           { color: #6cb6ff; }
  a:visited   { color: #b28cff; }
  .site-header{ background-color: #1c1f26; border-top-color: #2a2e37; border-bottom-color: #2a2e37; }
  .site-title, .site-title:visited { color: #e8eaed; }
  .site-nav   { background-color: #1c1f26; border-color: #2a2e37; }
  .site-nav .page-link { color: #d6d8dc; }
  .post-content h1, .post-content h2, .post-content h3 { color: #e8eaed; }
  blockquote  { color: #a8adb7; border-left-color: #3a3f4b; }
  pre, code   { background-color: #22262f; border-color: #2a2e37; color: #d6d8dc; }
  table       { color: #d6d8dc; }
  table th    { background-color: #1c1f26; }
  hr          { border-color: #2a2e37; }
}
```

### 8.4 换主题配色

Minima 主题自带的几个预设配色（`classic` / `dark` / `auto` / `solarized` 等）在较新版本里
可以用 `_config.yml` 的 `minima: skin:` 切换：

```yaml
minima:
  skin: auto       # 跟随系统深浅色；也可写 classic / dark
```

> 如果你的 Pages 构建日志报错说 `skin` 不认识，说明线上用的是旧版 Minima（2.5.x），
> 删掉这一行即可，用 8.3 的 CSS 方案达到同样的效果。

改主色（链接、标题颜色）：

```scss
$brand-color: #2a7ae2;   /* 把这行放在 @import "minima"; 的上面才能生效 */
@import "minima";
```

### 8.5 加图片

1. 把图片传到 `assets/images/` 目录
2. 文章里引用：

   ```markdown
   ![图片说明](/assets/images/screenshot.png)
   ```

   路径开头的 `/` 表示从网站根目录算起，**必须加**。

### 8.6 加评论（giscus，免费、无需服务器）

1. 去 <https://giscus.app/zh-CN>，按提示用 GitHub 账号授权，拿到一段 `<script>` 代码
2. 在仓库根目录新建 `_includes/comments.html`，粘贴那段脚本
3. 编辑 `_layouts/post.html`（没有就新建，内容从主题复制）加入 `{% include comments.html %}`
4. 需要打开仓库 **Settings → General → Features → Discussions**

> 这一步稍微进阶，需要时再到网上搜「Jekyll minima 加 giscus」，或直接找我帮你接。

---

## 9. 本地预览（可选，需要装 Ruby）

好处：改完立刻在浏览器看到效果，不用等线上部署、不占用提交记录。

1. 装 Ruby：打开 <https://rubyinstaller.org/downloads/>，下载 **WITH DEVKIT** 的
   `Ruby+Devkit 3.x.x (x64)`。安装时一路 Next，最后弹出的命令行窗口里按一次回车，
   让它装 MSYS2 组件。
2. 验证（重开一个新的 PowerShell 窗口）：

   ```powershell
   ruby -v
   gem -v
   ```

3. 换国内镜像源（否则 `bundle install` 大概率超时）：

   ```powershell
   gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
   ```

4. 安装依赖并启动：

   ```powershell
   cd "D:\开山鼻祖\DeepSeek Harness_files\zhryhzs.github.io"
   bundle install
   bundle exec jekyll serve
   ```

5. 浏览器打开 <http://127.0.0.1:4000/>。改文件后保存，刷新页面即可看到变化
   （加 `--livereload` 可以自动刷新）。

常见报错：

| 报错 | 解决 |
| --- | --- |
| `bundle install` 卡住/超时 | 执行第 3 步换源，或挂代理 |
| `Could not find gem 'wdm'` | 无关紧要；可把 Gemfile 里 wdm 那行删掉 |
| 端口被占用 `Address already in use` | 改端口：`bundle exec jekyll serve --port 4001` |
| 中文乱码 | 确认编辑器保存为 UTF-8 编码 |
| `Liquid Exception: ... did not have a valid date` | Front Matter 里 `date` 格式写错了 |

---

## 10. 排错手册

### 10.1 打开网址是 404

按顺序排查：

1. **等 2 分钟**。首次开启 Pages 后要构建，期间就是 404。
2. 网址对不对？本博客是**用户站点**，正确网址就是 <https://zhryhzs.github.io/>，
   后面**不能**再带仓库名。
3. `_config.yml` 里 `baseurl` 必须是 `""`（空字符串）。如果还写着 `"/zhryhzs-one"`，
   页面能打开但样式全丢——这是仓库改名后最容易忘记改的地方。
4. Settings → Pages → Source 是不是 `Deploy from a branch` + `main` + `/(root)`？
5. 仓库是 Public 吗？
6. 仓库根目录直接就是 `index.md` 吗？如果变成了 `zhryhzs.github.io/index.md`
   这样多套了一层文件夹，说明上传时把整个文件夹拖进去了，需要把里面的文件挪到根目录。
6. Settings → Pages 顶部会显示最近一次构建的状态；失败会有一条红字，点进去看详情。

### 10.2 首页出来了，但文章列表是空的

- `_posts` 文件夹名是不是写成了 `posts` / `_post`（少了 s）？
- 文件名是不是 `YYYY-MM-DD-xxx.md` 格式？
- 文件开头有没有完整的 Front Matter（两行 `---`）？
- `date` 是不是写成了未来时间？（未来日期的文章默认不显示）

### 10.3 文章打开是乱码或者显示成一堆 `--- layout: post ---`

说明这个文件没有被 Jekyll 处理：Front Matter 少了开头或结尾的 `---`。

### 10.4 页面样式全丢了（白底黑字，没有排版）

- `assets/main.scss` 是否还在，且**第一行就是** `---`、第二行 `---`？
- `_config.yml` 里 `theme: minima` 有没有写错（拼成 `minma` 就找不到主题）？
- 是否不小心删掉了 `assets` 整个目录？

### 10.5 改了配置但网站没变化

`_config.yml` 改动需要重新触发构建。走路线 A：随便改一个字再提交一次；
或者 Settings → Pages 里把 Source 重选一遍再 Save。

### 10.6 Actions 一直红叉

- 若 Source 是 `Deploy from a branch`：这是预期的（见 A5），删掉 `jekyll.yml` 即可。
- 若 Source 是 `GitHub Actions`：点进失败的作业看日志。常见原因是 `bundle install`
  网络抖动，重新运行一次通常就好：右上角 **Re-run all jobs**。

### 10.7 网址里出现 `%E4%BD%A0%E5%A5%BD` 这种百分号

因为分类或文件名用了中文。想让网址干净，就把 `categories` 和文件名改成英文。

### 10.8 想改绑自己的域名

1. 买域名（阿里云/腾讯云/Cloudflare 都行），解析里加一条 CNAME 指向 `zhryhzs.github.io`
2. 仓库 Settings → Pages → **Custom domain** 填入域名 → Save → 勾选 **Enforce HTTPS**
3. 同时在仓库根目录加一个名为 `CNAME` 的文件，内容只有一行域名（不要带 `https://`）

### 10.9 打不开 github.com

需要自备网络代理工具。这是环境问题，不是配置问题。

---

## 11. Git 命令速查

前提：已装 Git（<https://git-scm.com/download/win>），并且在项目目录里执行过
`git init` / `git remote add origin ...`（一次性配置，见下方"首次配置"）。

**首次配置（只做一次）**

```bash
cd "D:\开山鼻祖\DeepSeek Harness_files\zhryhzs.github.io"
git init
git branch -M main
git remote add origin https://github.com/zhryhzs/zhryhzs.github.io.git
git config --global user.name "zhryhzs"
git config --global user.email "你的邮箱@example.com"
```

**日常三连**

```bash
git add .                        # 暂存所有改动
git commit -m "说明这次改了什么"   # 存成一个提交
git push                         # 推到 GitHub（会自动重新部署）
```

| 命令 | 作用 |
| --- | --- |
| `git status` | 看哪些文件改了、还没提交 |
| `git log --oneline -10` | 看最近 10 次提交 |
| `git pull` | 把 GitHub 上的改动拉到本地（在网页上改过文件时需要先执行） |
| `git diff` | 看具体改了什么内容 |
| `git restore 文件名` | 放弃某个文件的未提交改动 |
| `git remote -v` | 确认远程仓库地址对不对 |

> ⚠️ 如果你**同时在网页上**改过文件，本地 `git push` 前先 `git pull`，否则会被拒绝
> （报错 `non-fast-forward`）。

---

## 12. 费用与限制

| 项目 | 说明 |
| --- | --- |
| 费用 | 公开仓库 + GitHub Pages **完全免费**，不限流量（有软性限制） |
| 仓库大小 | 建议 1 GB 以内，单文件不超过 100 MB |
| 月流量 | 软限制 100 GB/月（个人博客远远用不到） |
| 构建次数 | 软限制 10 次/小时（频繁提交要注意） |
| 支持内容 | 只托管**静态**网页，不支持 PHP/数据库/后端接口 |
| 商业用途 | 免费账号的 Pages 不允许用于纯商业站点（个人博客没问题） |
| 备案 | 用 `github.io` 域名不需要备案；绑定国内服务器/域名才涉及 |

---

## 13. 上线检查清单

发布前的最后一遍对照：

- [x] 仓库名 = `zhryhzs.github.io`，可见性 = Public
- [x] 仓库根目录能看到 `_config.yml`、`index.md`、`_posts/`、`assets/`
- [x] `_config.yml` 里 `url: "https://zhryhzs.github.io"` 且 `baseurl: ""`
- [ ] Settings → Pages → Source：`Deploy from a branch` / `main` / `/(root)` / 已 Save
- [ ] （路线 A）`.github/workflows/jekyll.yml` 已删除
- [ ] 访问 <https://zhryhzs.github.io/> 正常，文章能点开
- [ ] `/about/` 页面正常，`/feed.xml` 有内容（RSS 订阅）
- [ ] 手机浏览器上也打开看一眼（主题自带响应式布局）

---

**最后更新：2026-09-15**
