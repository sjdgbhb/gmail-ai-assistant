# agedcare-automation

澳大利亚养老护理公司 AI 自动化。

第一阶段交付：**AI 邮件助手** —— 自动分类收件箱邮件、按负责人风格生成回复草稿存进 Gmail Drafts，负责人审核后才发送。

## 文件

```
job.md                        本地需求笔记
workflows/
  ai-email-assistant.json     主工作流(12 节点)
  error-alert.json            错误告警工作流(任何流程失败→发邮件通知)
.n8n/                         本地 n8n 数据(挂载进容器,勿提交)
```

## 本地运行(已完成验证)

```powershell
docker run -d --name n8n-agedcare -p 5678:5678 `
  -v "D:/workspace/clients/agedcare-automation/.n8n:/home/node/.n8n" `
  n8nio/n8n:latest
```

打开 http://localhost:5678,首次访问需创建 owner 账号。
两个 workflow 已通过 `n8n import:workflow` 验证可在 n8n 2.44.5 正常导入。

## 工作流逻辑

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

## 上线前需要配置

| 位置 | 要改什么 |
|---|---|
| 所有 Gmail 节点 | 绑定负责人的 Gmail OAuth2 凭据(Google Cloud 开 Gmail API) |
| 两个 OpenAI 模型节点 | 创建 OpenAI credential:API key 填 Gemini key,Base URL 覆盖为 `https://generativelanguage.googleapis.com/v1beta/openai/`;模型 gemini-2.5-flash,草稿质量不够可换 2.5-pro |
| `Generate Draft Reply` 的 system prompt | 把 STYLE GUIDE 占位符换成负责人 5-10 封真实邮件样例,署名改真实姓名 |
| `Notify Owner` / `Notify Skipped` 的 `sendTo` | `owner@example.com` → 负责人邮箱 |
| 主 workflow 设置 | Settings → Error Workflow 选 `Error Alert` |

## 交付部署(已定)

1. **首选 n8n Cloud**:负责人订阅(账号归负责人),搭建方导入
2. **次选本地部署**:负责人公司常开电脑 + Docker + Tailscale 远程维护(数据不出公司,隐私卖点)
