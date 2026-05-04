# Hexo 个人技术博客设计文档

**日期**: 2026-05-04
**状态**: 已确认

## 概述

基于 Hexo + Butterfly 主题的个人技术博客，托管于 GitHub Pages，通过 GitHub Actions 自动部署。全功能覆盖：分类/标签、搜索、RSS、评论（Giscus）、暗色模式、文章系列、代码高亮、SEO、访问统计。

## 技术栈

| 层面 | 选型 | 说明 |
|------|------|------|
| SSG | Hexo 7.x | Node.js 静态网站生成器 |
| 主题 | Butterfly | 中文社区最成熟的全功能主题 |
| 托管 | GitHub Pages | 免费托管，自带 CDN |
| CI/CD | GitHub Actions | push 自动构建部署 |
| 评论 | Giscus | 基于 GitHub Discussions |
| 搜索 | 本地搜索 | Butterfly 主题内置 |
| RSS | hexo-gen-feed | 标准 RSS/Atom |
| 统计 | 不蒜子 | 一行脚本，无需后端 |
| SEO | 主题内置 | sitemap、OG、结构化数据 |
| 代码高亮 | highlight.js | 主题内置，200+ 语言 |

## 页面结构

- 首页（文章列表 + 大图 Banner + 头像 + 社交图标 + 分页）
- 分类页（/categories）
- 标签页（/tags）
- 归档页（/archives）
- 搜索（全局搜索弹窗）
- 文章系列/专题（/series）
- 关于页（/about）
- 友链页（/links）

### 文章页

- 标题 + 发布时间 + 分类/标签
- 目录（TOC）侧边栏
- 代码块（行号 + 一键复制 + 语言标签）
- 文末：版权声明 + 赞赏 + 上一篇/下一篇导航
- 评论（Giscus）
- 亮色/暗色自动切换（跟随系统）

## 语言

中文为主，不考虑英文翻译。

## 项目结构

```
blog/
├── _config.yml
├── _config.butterfly.yml
├── package.json
├── .github/workflows/
│   └── deploy.yml
├── source/
│   ├── _posts/
│   │   └── YYYY-MM-DD-title.md
│   ├── _drafts/
│   ├── about/index.md
│   ├── links/index.md
│   └── img/
├── scaffolds/
├── themes/butterfly/
└── public/
```

## 发布流程

1. `hexo new "文章标题"` 生成文章
2. 在 `source/_posts/` 下编写 Markdown
3. `git add && git commit && git push`
4. GitHub Actions 自动 `hexo generate` 并部署到 gh-pages 分支
5. GitHub Pages 自动更新

## 特色功能清单

- 标签/分类体系
- RSS 订阅
- Giscus 评论
- 本地全文搜索
- 暗色/亮色模式切换
- 文章系列（series）
- 代码高亮（行号 + 复制按钮）
- SEO（sitemap + OG 标签）
- 不蒜子访问统计
- 友链页面
- 赞赏支持
