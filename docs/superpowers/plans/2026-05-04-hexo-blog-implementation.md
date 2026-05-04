# Hexo 个人技术博客实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 搭建基于 Hexo + Butterfly 主题的全功能个人技术博客，托管于 GitHub Pages 并通过 GitHub Actions 自动部署。

**Architecture:** Hexo 作为静态网站生成器，Butterfly 主题提供 UI 和功能（暗色模式、搜索、评论集成等），npm 管理依赖，GitHub Actions 在 push 时自动构建并部署到 gh-pages 分支。

**Tech Stack:** Hexo 7.x, Butterfly 4.x, Node.js 24, GitHub Pages, GitHub Actions, Giscus, 不蒜子

---

### Task 1: 初始化 Hexo 项目

**Files:**
- Create: `package.json`, `_config.yml`, `source/`, `scaffolds/`, `themes/`

- [ ] **Step 1: 通过 npm 初始化 Hexo 项目**

```bash
cd /d/myproject
npx hexo init . --no-install
```

- [ ] **Step 2: 安装依赖**

```bash
npm install
```

- [ ] **Step 3: 验证 Hexo 可用**

```bash
npx hexo version
```

Expected: 显示 hexo-cli 和 hexo 版本号。

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "chore: init hexo project"
```

---

### Task 2: 安装 Butterfly 主题与插件

**Files:**
- Modify: `package.json`
- Create: `_config.butterfly.yml`

- [ ] **Step 1: 安装 Butterfly 主题**

```bash
npm install hexo-theme-butterfly
```

- [ ] **Step 2: 安装必备插件**

```bash
npm install hexo-generator-feed hexo-generator-search hexo-wordcount hexo-generator-sitemap hexo-generator-index-pin-top hexo-abbrlink
```

- [ ] **Step 3: 复制 Butterfly 默认配置到项目根目录**

```bash
cp node_modules/hexo-theme-butterfly/_config.yml ./_config.butterfly.yml
```

- [ ] **Step 4: Commit**

```bash
git add package.json package-lock.json _config.butterfly.yml
git commit -m "chore: add butterfly theme and plugins"
```

---

### Task 3: 配置站点基础信息

**Files:**
- Modify: `_config.yml`

- [ ] **Step 1: 编辑 `_config.yml` 核心配置**

将以下字段修改为你自己的信息（替换 `YourName`、`yourgithub` 等占位符）：

```yaml
# Site
title: YourName's Blog
subtitle: ''
description: '技术博客，记录学习与思考'
keywords: '技术,编程,后端,Go,Python'
author: YourName
language: zh-CN
timezone: Asia/Shanghai

# URL
url: https://yourgithub.github.io
root: /
permalink: posts/:abbrlink.html
permalink_defaults:
pretty_urls:
  trailing_index: false
  trailing_html: false

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories

# Writing
new_post_name: :title.md
default_layout: post
titlecase: false
filename_case: 0

# Theme
theme: butterfly

# Deployment (由 GitHub Actions 处理，这里留空)
deploy:
  type: ''
```

- [ ] **Step 2: Commit**

```bash
git add _config.yml
git commit -m "feat: configure site basics"
```

---

### Task 4: 配置导航菜单与外观

**Files:**
- Modify: `_config.butterfly.yml`

- [ ] **Step 1: 配置导航菜单**

在 `_config.butterfly.yml` 中定位到 `menu` 部分，替换为：

```yaml
menu:
  首页: / || fas fa-home
  归档: /archives/ || fas fa-archive
  分类: /categories/ || fas fa-folder-open
  标签: /tags/ || fas fa-tags
  友链: /links/ || fas fa-link
  关于: /about/ || fas fa-user
```

- [ ] **Step 2: 配置顶栏社交图标**

定位到 `social` 部分：

```yaml
social:
  fas fa-envelope: mailto:your-email@example.com || Email
  fab fa-github: https://github.com/yourgithub || Github
  fab fa-rss: /atom.xml || RSS
```

- [ ] **Step 3: 配置外观设置**

```yaml
# 头像
avatar:
  img: /img/avatar.png
  effect: true

# 暗色模式：跟随系统
darkmode:
  enable: true
  button: true
  autoChangeMode: 1  # 1=跟随系统, 2=跟随时间

# 首页设置
index_post_list:
  direction: column
  content_radius: 12
```

- [ ] **Step 4: Commit**

```bash
git add _config.butterfly.yml
git commit -m "feat: configure navigation, social, and appearance"
```

---

### Task 5: 配置功能特性（搜索、评论、RSS、统计、代码块）

**Files:**
- Modify: `_config.butterfly.yml`

- [ ] **Step 1: 配置本地搜索**

```yaml
local_search:
  enable: true
  preload: true
  top_n_per_article: 1
  unescape: false
  CDN:
```

- [ ] **Step 2: 配置 Giscus 评论**

需要先去 https://giscus.app 按引导配置好 GitHub Discussions 仓库，获取 `repo`、`repo-id`、`category`、`category-id`：

```yaml
comments:
  use: Giscus
  text: true
  lazyload: true
  count: true
  card_post_count: false

giscus:
  repo: yourgithub/yourgithub.github.io
  repo_id: ''
  category_id: ''
  category: Announcements
  theme:
    light: light
    dark: dark
  option:
    reactions: '1'
    metadata: '0'
    input_position: bottom
    lang: zh-CN
```

- [ ] **Step 3: 配置 RSS**

```yaml
# 在文件末尾添加
feed:
  type: atom
  path: atom.xml
  limit: 20
  content: true
  order_by: -date
```

- [ ] **Step 4: 配置不蒜子统计**

```yaml
busuanzi:
  site_uv: true
  site_pv: true
  page_pv: true
```

- [ ] **Step 5: 配置代码块**

```yaml
highlight_theme: mac
code_word_wrap: false

highlight_copy: true
highlight_lang: true
highlight_height_limit: 350

code_blocks:
  theme: mac
  macStyle: true
  height_limit: 350
  word_wrap: false
  # PrismJS (Butterfly 4.x)
  highlightjs: true
```

- [ ] **Step 6: Commit**

```bash
git add _config.butterfly.yml
git commit -m "feat: configure search, comments, RSS, stats, code blocks"
```

---

### Task 6: 配置文章页面（TOC、版权、赞赏、系列）

**Files:**
- Modify: `_config.butterfly.yml`

- [ ] **Step 1: 配置文章元数据与 TOC**

```yaml
post_meta:
  page:
    date_type: created
    categories: true
    tags: true
    label: true

# 文章目录
toc:
  post: true
  page: true
  number: true
  expand: false
  style_simple: false
  scroll_percent: true
```

- [ ] **Step 2: 配置版权信息**

```yaml
post_copyright:
  enable: true
  decode: false
  author_href: https://github.com/yourgithub
  license: CC BY-NC-SA 4.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/4.0/
```

- [ ] **Step 3: 配置赞赏**

```yaml
reward:
  enable: true
  text: 如果这篇文章对你有帮助，欢迎赞赏~
  QR_code:
    - img: /img/wechat-reward.png
    - img: /img/alipay-reward.png
```

- [ ] **Step 4: 配置文章底部导航与相关文章**

```yaml
post_pagination: 1

related_post:
  enable: true
  limit: 6
  date_type: created
```

- [ ] **Step 5: 启用文章系列（Series）**

```yaml
series:
  enable: true
  orderBy: -date
  number: true
```

- [ ] **Step 6: Commit**

```bash
git add _config.butterfly.yml
git commit -m "feat: configure post page features (TOC, copyright, reward, series)"
```

---

### Task 7: 创建页面（关于、友链）

**Files:**
- Create: `source/about/index.md`
- Create: `source/links/index.md`

- [ ] **Step 1: 创建关于页**

写入 `source/about/index.md`：

```markdown
---
title: 关于
date: 2026-05-04
type: about
---

## 关于我

一名后端开发工程师，专注于 Go/Python 技术栈。

热爱技术分享，这个博客用于记录我的学习笔记与技术思考。

### 联系方式

- GitHub: [@yourgithub](https://github.com/yourgithub)
- Email: your-email@example.com
```

- [ ] **Step 2: 创建友链页**

写入 `source/links/index.md`：

```markdown
---
title: 友链
date: 2026-05-04
type: links
---

欢迎交换友链！可以在下方评论区留言，格式：

> 名称：你的博客名
> 链接：https://yourblog.com
> 头像：https://yourblog.com/avatar.png
> 简介：一句话描述
```

- [ ] **Step 3: Commit**

```bash
git add source/about/ source/links/
git commit -m "feat: add about and links pages"
```

---

### Task 8: 创建文章脚手架模板

**Files:**
- Create: `scaffolds/post.md`

- [ ] **Step 1: 创建自定义文章模板**

写入 `scaffolds/post.md`：

```markdown
---
title: {{ title }}
date: {{ date }}
updated:
tags:
categories:
keywords:
description:
top_img:
cover:
abbrlink:
series:
---
```

- [ ] **Step 2: Commit**

```bash
git add scaffolds/post.md
git commit -m "feat: add post scaffold template"
```

---

### Task 9: 配置 GitHub Actions 自动部署

**Files:**
- Create: `.github/workflows/deploy.yml`
- Create: `.gitignore`（补充）

- [ ] **Step 1: 创建 GitHub Actions 工作流**

写入 `.github/workflows/deploy.yml`：

```yaml
name: Deploy Blog

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22'

      - name: Cache node modules
        uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-

      - name: Install dependencies
        run: npm ci

      - name: Generate static files
        run: npx hexo generate

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          publish_branch: gh-pages
          force_orphan: true
          user_name: 'github-actions[bot]'
          user_email: 'github-actions[bot]@users.noreply.github.com'
          commit_message: 'deploy: ${{ github.event.head_commit.message }}'
```

- [ ] **Step 2: 确保 .gitignore 正确**

检查 `.gitignore` 至少包含：

```
node_modules/
public/
.deploy_git/
db.json
```

- [ ] **Step 3: Commit**

```bash
git add .github/ .gitignore
git commit -m "ci: add GitHub Actions deploy workflow"
```

---

### Task 10: 编写示例文章并验证本地构建

**Files:**
- Create: `source/_posts/2026-05-04-hello-world.md`

- [ ] **Step 1: 生成并编写示例文章**

```bash
cd /d/myproject
npx hexo new "Hello World"
```

编辑 `source/_posts/2026-05-04-hello-world.md`：

```markdown
---
title: Hello World
date: 2026-05-04
tags:
  - 博客
categories:
  - 随笔
description: 我的第一篇技术博客
abbrlink: hello-world
---

## 欢迎来到我的技术博客

这是我的第一篇博客文章。在这里我将分享：

- **Go 语言**后端开发经验
- **Python** 数据处理技巧
- 系统设计与架构思考
- 开源项目实践

### 代码示例

​```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Blog!")
}
​```

> 路漫漫其修远兮，吾将上下而求索。
```

- [ ] **Step 2: 本地构建验证**

```bash
npx hexo generate
```

Expected: 无错误，`public/` 目录生成静态文件。

- [ ] **Step 3: 启动本地服务器预览**

```bash
npx hexo server
```

访问 `http://localhost:4000` 检查首页、文章页、搜索、暗色模式切换等功能。

- [ ] **Step 4: 确认无误后停止服务器（Ctrl+C），Commit**

```bash
git add source/_posts/
git commit -m "feat: add hello world post"
```

---

### Task 11: 创建 GitHub 仓库并推送

**Prerequisite:** 需要先在 GitHub 上创建仓库 `yourgithub.github.io`（用户名 + `.github.io`）。

- [ ] **Step 1: 初始化 Git（如尚未初始化）并设置远程仓库**

```bash
cd /d/myproject
git init  # 如已初始化则跳过
git remote add origin https://github.com/yourgithub/yourgithub.github.io.git
```

- [ ] **Step 2: 推送到 GitHub**

```bash
git branch -M main
git push -u origin main
```

- [ ] **Step 3: 在 GitHub 仓库 Settings > Pages 中**

将 Source 设置为 `Deploy from a branch`，分支选择 `gh-pages`，目录 `/ (root)`，点击 Save。

- [ ] **Step 4: 等待 Actions 完成**

在仓库 `Actions` 标签页查看构建部署进度。完成后访问 `https://yourgithub.github.io` 确认博客上线。

---

### Task 12: 启用 Giscus 评论

**Prerequisite:** 仓库需是公开的，且已启用 Discussions 功能。

- [ ] **Step 1: 在仓库 Settings > Features 中启用 Discussions**

- [ ] **Step 2: 访问 https://giscus.app/zh-CN**

填入仓库名 `yourgithub/yourgithub.github.io`，按指引完成配置。

- [ ] **Step 3: 获取 repo_id、category_id**

将获取到的 ID 填入 `_config.butterfly.yml` 中 `giscus` 的 `repo_id` 和 `category_id` 字段。

- [ ] **Step 4: Commit 并推送**

```bash
git add _config.butterfly.yml
git commit -m "feat: enable giscus comments"
git push
```
