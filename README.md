# agedcare-automation

澳大利亚养老护理公司的 AI 邮件自动化（n8n 工作流）。自动分类收件箱邮件、按负责人风格生成回复草稿存入 Gmail Drafts，负责人审核后才发送——不自动外发。

## Features

- **邮件分类**：LLM 输出类别 / 优先级 / 是否需回复 / 跟进项（JSON）
- **草稿生成**：按负责人风格指南生成回复，写入同一线程的 Gmail Draft
- **人工兜底**：LLM 输出解析失败或无需回复时，均发通知提醒人工处理
- **错误告警**：任一工作流失败自动邮件通知负责人

## Tech Stack

- **n8n**（self-hosted Docker 或 n8n Cloud），已在 n8n 2.44.5 验证
- **Gemini 2.5 Flash**（经 OpenAI 兼容端点接入，可换 2.5 Pro）
- **Gmail API**（OAuth2）

## Prerequisites

- Docker Desktop（本地部署）或 n8n Cloud 账号
- Google Cloud 项目：启用 Gmail API 并创建 OAuth2 凭据
- Gemini API key

## Quick Start

```powershell
# 1. 启动 n8n（本地数据挂载在 .n8n/）
docker run -d --name n8n-agedcare -p 5678:5678 `
  -v "<repo-path>/.n8n:/home/node/.n8n" `
  n8nio/n8n:latest

# 2. 打开 http://localhost:5678，创建 owner 账号
# 3. 导入 workflows/ 下两个 JSON 工作流
# 4. 按下表完成 Configuration
```

## Workflow

```
Gmail Trigger (每5分钟轮询, INBOX+unread, 排除promotions/social/chat)
  → Classify Email (LLM: 类别/优先级/是否需回复/跟进项, 输出JSON)
  → Parse Classification (容错解析, 解析失败默认人工审核)
  → Needs Reply?
     ├─ true  → Generate Draft Reply (风格指南 prompt)
     │          → Create Gmail Draft (同线程, 不发送)
     │          → Notify Owner (邮件通知优先级+摘要)
     └─ false → Notify Skipped (通知跳过的邮件及原因, 防止漏看)
```

## Configuration

上线前必须完成：

| 位置 | 配置项 |
|---|---|
| 所有 Gmail 节点 | 绑定负责人的 Gmail OAuth2 凭据（Google Cloud 开 Gmail API） |
| 两个 OpenAI 模型节点 | 创建 OpenAI credential：API key 填 Gemini key，Base URL 覆盖为 `https://generativelanguage.googleapis.com/v1beta/openai/`；模型 `gemini-2.5-flash` |
| `Generate Draft Reply` 的 system prompt | 把 STYLE GUIDE 占位符换成负责人 5-10 封真实邮件样例，署名改真实姓名 |
| `Notify Owner` / `Notify Skipped` 的 `sendTo` | `owner@example.com` → 负责人邮箱 |
| 主 workflow 设置 | Settings → Error Workflow 选 `Error Alert` |

## Deployment

1. **首选 n8n Cloud**：负责人订阅（账号归负责人），搭建方导入
2. **次选本地部署**：负责人公司常开电脑 + Docker + Tailscale 远程维护（数据不出公司，隐私卖点）

## Project Structure

```
workflows/               n8n 工作流 JSON（可直接 import）
test-samples/            邮件分类/草稿生成测试样例
.n8n/                    本地 n8n 数据（gitignored，含凭据勿提交）
```

> 需求文档为本地工作文件，不入库。
