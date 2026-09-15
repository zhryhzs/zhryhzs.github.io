---
layout: post
title: "如何写一篇新文章（Markdown 速查）"
date: 2026-09-15 18:30:00 +0800
categories: 教程
tags: [markdown, jekyll]
---

这篇文章既是一篇示例，也是我给自己留的备忘。

## 一、新建文件的规则

在 `_posts` 文件夹里新建一个文件，文件名必须严格是：

```
年-月-日-英文标题.md
```

例如：`2026-09-15-how-to-write-a-post.md`

> 标题建议用英文或拼音，别用中文和空格，避免链接出问题。

## 二、文件开头必须写 Front Matter

就是文件最上面被两行 `---` 夹住的那一小段：

```yaml
---
layout: post
title: "文章标题（中文没问题）"
date: 2026-09-15 18:30:00 +0800
categories: 教程
tags: [markdown, jekyll]
---
```

- `layout: post` —— 固定写法，代表这是一篇文章
- `date` —— 决定文章顺序，格式 `年-月-日 时:分:秒 +0800`
- `categories` / `tags` —— 分类和标签，可以留空

## 三、Markdown 常用语法

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
  - 缩进两个空格就是子项

1. 有序列表
2. 第二项
```

### 链接和图片

```markdown
[链接文字](https://example.com)
![图片说明](/assets/images/pic.png)
```

图片放在仓库的 `assets/images/` 目录下，用 `/assets/images/文件名` 引用。

### 引用

```markdown
> 这是一段引用。
```

### 代码块

用三个反引号包起来，并在开头写上语言名可以得到语法高亮：

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
| Jekyll | 静态站点生成器 |
| Minima | 默认主题 |
```

| 名称 | 说明 |
| --- | --- |
| Jekyll | 静态站点生成器 |
| Minima | 默认主题 |

### 分割线

```markdown
---
```

---

## 四、发布

保存文件后提交并推送：

```bash
git add .
git commit -m "新增文章"
git push
```

等一两分钟刷新 <https://zhryhzs.github.io> 就能看到了。
