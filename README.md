# zhryhzs 的博客

基于 **Hexo 8** + **[LostStar](https://github.com/thatnghiepdev/hexo-theme-lostar)** 主题（MIT 开源），
托管在 **GitHub Pages**，由 **GitHub Actions** 自动构建发布。

🌐 **在线地址：<https://zhryhzs.github.io>**

---

## 这个博客长什么样

- 首页全屏大图 + 星空 canvas 背景（星星闪烁、偶尔划过流星）
- 右侧个人卡片：头像、昵称、简介、友链
- 文章卡片带分类配色，鼠标悬停有动画
- 代码高亮（Highlight.js）、图片点击放大（灯箱）
- 顶栏导航：首页 / 关于 / 归档 / 分类 / 标签
- 手机端自适应，带侧滑菜单

---

## 目录结构

```
.
├── _config.yml              ★ 站点总配置（标题、网址、分页、高亮开关）
├── package.json             依赖清单 + Hexo 版本标记
├── package-lock.json        依赖版本锁定
├── scaffolds/               新建文章的模板
│   ├── post.md
│   └── page.md
├── source/                  ★ 你写的东西都放这里
│   ├── _posts/              ★★ 所有文章
│   │   ├── welcome.md
│   │   ├── hello-world.md
│   │   └── how-to-write-a-post.md
│   ├── about/index.md       「关于」页面
│   └── images/              图片放这里（用 /images/xxx.png 引用）
├── themes/lostar/           主题（已内置，可以直接改）
│   ├── _config.yml          ★ 主题配置：头像、背景、导航、文案、评论
│   ├── layout/*.ejs         页面模板
│   ├── scripts/*.js         构建期脚本（分类配色等）
│   └── source/              主题自带的 css / js / 图片
└── .github/workflows/deploy.yml   自动构建部署脚本
```

> `node_modules/` 和 `public/` 是本地生成物，不会上传到仓库。

---

## 怎么写一篇新文章

### 方式一：在 GitHub 网页上写（不用装任何东西）

1. 打开仓库 → 进入 `source/_posts/` 目录
2. 右上角 **Add file** → **Create new file**
3. 文件名用**英文**，例如 `my-first-note.md`
4. 内容按下面模板写：

```markdown
---
title: 文章的中文标题
date: 2026-10-01 20:30:00
categories:
  - 随笔
tags:
  - 标签一
  - 标签二
---

这里写摘要，会显示在首页卡片上。

<!-- more -->

下面开始是完整正文。
```

5. 底部 **Commit changes** → 等 1~2 分钟，网站自动更新

### 方式二：本地写（需要装 Node.js）

```bash
npm install          # 第一次先装依赖
npx hexo new "文章标题"   # 新建文章
npx hexo server      # 本地预览 http://localhost:4000
npx hexo generate    # 生成静态文件到 public/
```

---

## 想改外观改哪里

| 想改什么 | 改哪个文件 | 怎么改 |
| --- | --- | --- |
| 博客标题 / 副标题 / 简介 | `_config.yml` | `title` / `subtitle` / `description` |
| 头像 | `themes/lostar/_config.yml` | `avatar:` 指向 `/images/xxx` |
| 首页大图 | `themes/lostar/_config.yml` | `background:` 可以写多张，每次刷新随机 |
| 导航栏文字 | `themes/lostar/_config.yml` | `menu:` 下面的键名就是显示文字 |
| 个人卡片简介 / 友链 | `themes/lostar/_config.yml` | `card:` 段 |
| 「阅读全文」按钮文字 | `themes/lostar/_config.yml` | `i18n.read_more` |
| 加载页文字 | `themes/lostar/_config.yml` | `i18n.loading_message` |
| 分类的颜色 | `themes/lostar/_config.yml` | `taxonomy_styles.categories` |
| 星空背景开关 | `themes/lostar/_config.yml` | `stars.enable` |
| 图片灯箱开关 | `themes/lostar/_config.yml` | `preview.enable` |
| 整体样式 | `themes/lostar/source/css/main.css` | 直接改 CSS |

> 改完提交，Actions 会自动重新构建。

### 换自己的头像和背景图

1. 把图片传到 `source/images/` 目录（文件名用英文，例如 `my-avatar.jpg`）
2. 修改 `themes/lostar/_config.yml`：

```yaml
avatar: /images/my-avatar.jpg
background:
  - /images/my-bg.jpg
```

> 注意路径开头的 `/` 必须保留。仓库里没有 `source/images/` 目录的话，
> 在 GitHub 网页上新建文件时输入 `source/images/xxx.jpg` 会自动建目录。

---

## 开启评论（giscus，免费）

1. 打开仓库 **Settings → General → Features**，勾上 **Discussions**
2. 去 <https://giscus.app/zh-CN> 按提示授权并填仓库信息，拿到 `repo` / `repoID` / `categoryID`
3. 填进 `themes/lostar/_config.yml` 的 `giscus` 段，把 `enable` 改成 `true`

---

## 关于主题

主题 [hexo-theme-lostar](https://github.com/thatnghiepdev/hexo-theme-lostar) 由
[thatnghiep-dev](https://github.com/thatnghiep-dev) 开发，**MIT 许可证**，
继承自 [ParticleX](https://github.com/theme-particlex/hexo-theme-particlex) 与
[Particle](https://github.com/korilin/hexo-theme-particle)。

按 MIT 许可证要求，页脚的作者署名（`layout/footer.ejs`）予以保留，
`themes/lostar/LICENSE` 也一并保留。

---

**最后更新：2026-09-15**
