# ZJL's Blog

用 Hugo 构建、托管在 GitHub Pages 上的个人博客。线上地址：https://zjlzju.github.io/

## 这个仓库是怎么工作的

- `content/posts/` 下的每个 `.md` 文件就是一篇文章
- 每次把改动提交到 `master` 分支，GitHub Actions 会自动构建并发布（约 1 分钟）
- 不需要在本地安装 Hugo 或 Git，在 GitHub 网页上编辑即可

## 怎么写一篇新文章

1. 在 GitHub 网页打开 `content/posts/` 目录，点 "Add file" → "Create new file"
2. 文件名随意，比如 `my-first-post.md`（要用英文，不能有空格）
3. 文件内容按这个格式写：

```
---
title: 文章标题
date: 2026-11-25
categories: [随笔]
tags: [标签一, 标签二]
draft: false
---

正文写在这里，支持 Markdown 语法：

- **加粗**、*斜体*、[链接](https://example.com)
- 列表、代码块、引用
- 图片等等
```

4. 拉到页面底部，点 "Commit changes" 提交
5. 等约 1 分钟，打开 https://zjlzju.github.io/ 就能看到新文章

## 字段说明

| 字段 | 作用 | 示例 |
|------|------|------|
| `title` | 文章标题（必填） | `写在开头` |
| `date` | 发布日期 | `2026-11-25` |
| `categories` | 分类，会归入分类页 | `[随笔]` |
| `tags` | 标签，会归入标签页 | `[想法, 记录]` |
| `draft` | `true` 表示草稿不发布 | `false` |

## 常用页面

- 首页：https://zjlzju.github.io/
- 所有文章：https://zjlzju.github.io/posts/
- 分类：https://zjlzju.github.io/categories/
- 标签：https://zjlzju.github.io/tags/

## 目录结构

```
.
├── .github/workflows/deploy.yml   # 自动构建发布的工作流
├── content/
│   ├── _index.md                  # 首页内容
│   └── posts/                     # 文章目录（在这里写文章）
├── layouts/                       # 页面模板（一般不用动）
├── static/
│   ├── css/style.css              # 样式（莫兰迪风格）
│   └── Ryoma.jpg                  # 头像
├── hugo.toml                      # 站点配置（标题、友情链接等）
└── README.md                      # 本说明
```

想改友情链接或站点标题，编辑根目录的 `hugo.toml` 即可。
