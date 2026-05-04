---
title: Claude Code 接入 DeepSeek API 完整教程
date: 2026-05-04 22:01:43
updated: 2026-05-04
tags:
  - Claude Code
  - DeepSeek
  - AI 工具
categories:
  - 工具教程
keywords: Claude Code,DeepSeek,API 配置,AI 编程
description: 详细教程：如何将 Claude Code 接入 DeepSeek API，大幅降低使用成本（约 50-95 倍），同时保持强大的编程辅助能力。
abbrlink: claude-code-deepseek
---

## 前言

Claude Code 是 Anthropic 推出的命令行 AI 编程助手，原生使用 Claude 系列模型。但每月 $200 的订阅费用对个人开发者来说成本偏高。好消息是，Claude Code 支持自定义 API 端点，可以接入 DeepSeek 等性价比更高的模型。

本文将详细介绍三种接入方式，帮助你以极低的成本享受 AI 编程体验。

## 为什么选择 DeepSeek

| 对比维度 | Claude Opus/Sonnet 官方 | DeepSeek V3.2/V4 |
|---------|------------------------|-------------------|
| 月费 | $200（Pro 计划） | 按量计费，约 $5-10/月 |
| 百万 Token 单价 | ~$15.00 | ~$0.30 |
| 编程能力 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| 中文支持 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| 上下文长度 | 200K | 128K-1M |

**节省幅度约 50-95 倍**，对于个人开发者而言极具吸引力。

## 前置准备

1. **安装 Claude Code**：通过 npm 全局安装
   ```bash
   npm install -g @anthropic-ai/claude-code
   ```

2. **获取 DeepSeek API Key**：
   - 前往 [platform.deepseek.com](https://platform.deepseek.com) 注册账号
   - 在 API Keys 页面创建密钥
   - 新用户赠送 $10 额度，足够体验一段时间

## 方案一：直接配置环境变量

DeepSeek 提供了兼容 Anthropic API 格式的端点，配置最简单。

编辑 Claude Code 配置文件 `~/.claude/settings.json`（Windows 为 `%USERPROFILE%\.claude\settings.json`）：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "sk-你的DeepSeek密钥",
    "ANTHROPIC_MODEL": "deepseek-chat",
    "ANTHROPIC_SMALL_FAST_MODEL": "deepseek-chat",
    "API_TIMEOUT_MS": "600000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1"
  }
}
```

各配置项说明：

| 配置项 | 说明 |
|--------|------|
| `ANTHROPIC_BASE_URL` | DeepSeek 的 Anthropic 兼容端点地址 |
| `ANTHROPIC_AUTH_TOKEN` | 替换为你的 DeepSeek API Key |
| `ANTHROPIC_MODEL` | 使用的模型名称 |
| `API_TIMEOUT_MS` | API 超时时间（毫秒），DeepSeek 推理模式响应较慢，建议设 10 分钟 |
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | 禁用非必要流量，减少不必要的 API 调用 |

启动时使用 `--bare` 参数绕过默认认证：

```bash
claude --settings ~/.claude/settings.json --model sonnet
```

## 方案二：使用火山方舟（国内用户推荐）

火山方舟提供了 DeepSeek 模型的国内托管，无需翻墙，按量付费。

1. 注册 [火山方舟](https://console.volcengine.com/ark) 账号
2. 在模型广场搜索 DeepSeek 模型并开通
3. 创建 API Key

配置文件：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://ark.cn-beijing.volces.com/api/coding",
    "ANTHROPIC_AUTH_TOKEN": "你的方舟API_KEY",
    "ANTHROPIC_MODEL": "deepseek-v3.2",
    "ANTHROPIC_SMALL_FAST_MODEL": "deepseek-v3.2",
    "API_TIMEOUT_MS": "600000"
  }
}
```

## 方案三：使用代理工具

适合需要更灵活控制的用户。推荐开源项目 `deepseek-claude-proxy`：

```bash
# 安装
npm install -g deepseek-claude-proxy

# 初始化配置
deepseek-claude-proxy init

# 启动代理（默认监听 localhost:1849）
deepseek-claude-proxy start
```

然后在 VS Code 的 Claude Code 扩展设置中配置环境变量：

```json
{
  "claudeCode.environmentVariables": [
    { "name": "ANTHROPIC_BASE_URL", "value": "http://localhost:1849" },
    { "name": "ANTHROPIC_DEFAULT_HAIKU_MODEL", "value": "deepseek-chat" }
  ]
}
```

代理工具的优势在于可以自由切换后端，也便于添加日志监控和请求过滤。

## 方案四：使用 CC-Switch 一键切换（强烈推荐）

如果觉得手动编辑 JSON 配置文件繁琐，**CC-Switch** 是专为 Claude Code 设计的一键切换工具，内置了 17+ 主流模型供应商的预设配置。

### CC-Switch 的优势

| 特性 | 手动配置 | CC-Switch |
|------|---------|-----------|
| 配置方式 | 手动编辑 settings.json | 图形界面点选 |
| 模型切换 | 改文件 + 重启 | 一键切换，即时生效 |
| API Key 管理 | 明文写在 JSON 里 | 本地加密存储 |
| MCP/Skills 管理 | 手动命令行 | 图形界面一键安装/卸载 |
| 内置供应商 | 需自行查找端点 | DeepSeek、GLM、Kimi、Gemini、通义千问等 17+ |

### 安装 CC-Switch

**macOS：**
```bash
brew tap farion1231/ccswitch
brew install --cask cc-switch
```

**Windows / Linux：**
前往 [GitHub Releases](https://github.com/farion1231/cc-switch/releases) 下载对应安装包。

**命令行版（ccenv-cli）：**
```bash
npm install -g ccenv-cli
```

### 使用 CC-Switch 接入 DeepSeek

**图形界面版：**

1. 启动 CC-Switch，系统托盘会出现图标
2. 点击托盘图标 → **供应商管理**
3. 选择 **DeepSeek** 预设（或点击自定义添加）
4. 填入 API Key，CC-Switch 会自动填充端点地址和模型名称
5. 在模型列表中点击 **启用**，切换即时生效
6. 终端里直接使用 `claude`，无需额外参数

CC-Switch 通过在本地 `127.0.0.1:15721` 启动代理的方式实现无缝切换。切换模型时不需要重启终端，所有请求自动路由到当前启用的供应商。

**命令行版：**
```bash
# 交互式配置向导
ccx setup

# 创建 DeepSeek 配置
ccx create ds --template deepseek --api-key sk-你的密钥

# 激活配置
eval "$(ccx use ds)"

# 常规使用
claude
```

### 快速切换多个供应商

CC-Switch 的核心价值在于**随时切换**，不同场景用不同模型：

```
写业务代码  → 切到 Claude Sonnet（质量优先）
写脚本工具  → 切到 DeepSeek V4（性价比高）
中文文档    → 切到 通义千问（中文最优）
免费额度    → 切到 Gemini（有免费层）
```

切换后所有打开的终端即时生效，不需要重启窗口。

## 常见问题

### 1. 启动时报认证错误

使用 `--bare` 参数启动，绕过 OAuth/Keychain 的优先级冲突：

```bash
claude --settings ~/.claude/settings.json --model sonnet --bare
```

### 2. 模型名不被识别

Claude Code 内部只认 `sonnet`、`opus`、`haiku` 等 Anthropic 模型名。DeepSeek 端点会在服务端自动映射，所以你仍然传 `--model sonnet` 即可。

### 3. 响应时间过长

DeepSeek 推理模式可能响应较慢，确保 `API_TIMEOUT_MS` 设置足够大（建议 600000ms），避免超时中断。

### 4. 图片/截图无法识别

DeepSeek V4 为纯文本模型，暂不支持 Vision 模式。涉及截图分析的场景会自动降级为纯文本理解。

### 5. 子代理模型配置

如需为子代理指定不同模型，可添加：
```json
{
  "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-chat"
}
```

## 总结

通过以上配置，你可以用不到官方十分之一的价格享受 Claude Code 的编程辅助能力。对于个人开发者和学习用途，这无疑是最具性价比的选择。

**方案选择建议：**

- **新手首选** → 方案四（CC-Switch），图形界面一键切换，零门槛
- **日常使用** → 方案一（DeepSeek 官方 API），简单直接
- **国内网络** → 方案二（火山方舟），延迟低、无需代理
- **高度定制** → 方案三（代理工具），灵活切换后端

---

> 参考来源：[CSDN: CC-Switch + Claude Code 接入 DeepSeek 教程](https://deepseek.csdn.net/69f49f9b0a2f6a37c5a737ec.html)
> GitHub 项目：[CC-Switch](https://github.com/farion1231/cc-switch) | [claudecode-deepseek-stack](https://github.com/MG-Cafe/claudecode-deepseek-stack) | [ccenv-cli](https://www.npmjs.com/package/ccenv-cli)
