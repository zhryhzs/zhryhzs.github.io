---
title: 如何写一篇新文章（Markdown 速查）
date: 2026-09-15 22:00:00
updated: 2026-09-15 22:00:00
categories:
  - 教程
tags:
  - Markdown
  - Hexo
---

这篇文章既是示例，也是给自己留的备忘。

<!-- more -->

## 一、新建文章

有两种方式。

**方式一：命令行**

```bash
hexo new "文章的中文标题"
```

会在 `source/_posts/` 下生成一个 `.md` 文件，文件名自动用标题命名。

**方式二：直接在网页上建文件**

进仓库的 `source/_posts/` 目录 → `Add file` → `Create new file`，
文件名用英文（比如 `my-first-note.md`），提交后会自动重新构建。

## 二、开头必须写的部分

文件最上面被两行 `---` 夹住的叫 Front Matter：

```yaml
---
title: 文章标题
date: 2026-09-15 22:00:00
categories:
  - 教程
tags:
  - 标签一
  - 标签二
---
```

- `title` —— 文章标题，中文随意
- `date` —— 决定排序，格式 `年-月-日 时:分:秒`
- `categories` / `tags` —— 分类和标签，可以留空

## 三、摘要分割线

在正文里插入 `<!-- more -->`，它上面的内容会作为首页卡片上的摘要，
下面才是完整正文。不写的话默认截取开头一段。

## 四、Markdown 常用语法

### 标题

```markdown
# 一级标题
## 二级标题
### 三级标题
```

### 文字强调

```markdown
**加粗**、*斜体*、`行内代码`、~~删除线~~
```

效果：**加粗**、*斜体*、`行内代码`、~~删除线~~

### 列表

```markdown
- 无序列表项
- 第二项

1. 有序列表
2. 第二项
```

### 链接与图片

```markdown
[链接文字](https://example.com)
![图片说明](/images/pic.png)
```

图片放在 `source/images/` 目录下，用 `/images/文件名` 引用。
点击图片会自动放大（灯箱效果）。

### 引用

```markdown
> 这是一段引用。
```

### 代码块

用三个反引号包起来，开头写上语言名就有语法高亮：

````markdown
```python
print("hello world")
```
````

```python
print("hello world")
```

### 表格

```markdown
| 名称 | 说明 |
| --- | --- |
| Hexo | 静态站点生成器 |
| LostStar | 当前使用的主题 |
```

| 名称 | 说明 |
| --- | --- |
| Hexo | 静态站点生成器 |
| LostStar | 当前使用的主题 |

## 五、发布

提交并推送到 GitHub 后，Actions 会自动重新构建，等一两分钟刷新即可看到。

---

写完记得在本地跑一次 `hexo server` 预览，避免格式出错。
