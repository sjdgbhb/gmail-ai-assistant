# 测试邮件样本(aged care 场景)

用法:挑几封发到你绑定的 Gmail,然后在 n8n 里 Execute workflow。
每封标注了**预期行为**,用来验证分类/优先级/草稿是否符合预期。

---

## ① 负责人咨询 — 预期:needs_reply=true,normal,生成草稿

```
Subject: Enquiry about respite care for my mother

Hi,

My mother is 82 and living at home in Frankston. I care for her full-time
but I need a break over Christmas - roughly 2 weeks from 20 December.

Do you offer respite care, and do you have availability around then?
Also, is she eligible for government funding or would this be private?

Thanks,
Sarah Mitchell
```

## ② 家属投诉 — 预期:needs_reply=true,**urgent**,草稿顶部应有 URGENT 标记

```
Subject: Carer did not show up today

Hello,

I'm writing because the carer scheduled for my father (John Cooper) did not
arrive this morning. This is the second time this month. He missed his
morning medication prompt and we had to scramble.

I want to know what you're going to do about this. If it happens again we
will be looking at other providers.

Regards,
Michael Cooper
```

## ③ 员工请假 — 预期:needs_reply=true,staff 类,生成草稿

```
Subject: Leave request - 15 to 22 November

Hi,

I'd like to request annual leave from 15 to 22 November. My rostered
clients for that week are Mrs. Patterson, Mr. Singh and Mrs. Okafor.

Happy to help hand over their care notes to whoever covers.

Thanks,
Emma (Support Worker)
```

## ④ 供应商发票 — 预期:needs_reply=true 或边界,supplier 类

```
Subject: Invoice INV-2847 - Medical supplies order

Dear accounts team,

Please find attached invoice INV-2847 for the October medical supplies
order ($1,240.00). Payment terms are 14 days.

Could you confirm receipt and advise expected payment date?

Best,
MedSupply Co. Accounts
```

## ⑤ 监管/政府 — 预期:needs_reply=true,**urgent**,草稿应为保守安抚回复

```
Subject: Re: Documentation request - Care records audit

Dear Provider,

Following our quality review notification, we require submission of
care documentation for the sample period July-September 2026 by
5 November 2026.

Please confirm receipt of this request and your nominated contact
person for the audit.

Regards,
Aged Care Quality Commission
```

## ⑥ Newsletter — 预期:needs_reply=**false**,走 Notify Skipped 分支

```
Subject: This week in aged care - industry news digest

Hi subscriber,

Here are this week's top stories:
- New funding model announced for home care packages
- Workforce survey results released
- Upcoming webinar: dementia care best practices

Read more on our website. You're receiving this because you subscribed.

Industry News Weekly
```

## ⑦ 自动通知 — 预期:needs_reply=**false**,跳过

```
Subject: Your booking is confirmed - Ref #4471

This is an automated message. Please do not reply.

Your booking for 28 October 2026, 10:00 AM has been confirmed.
Reference: #4471
```

## ⑧ 明确不需要回复的 FYI — 预期:needs_reply=**false**

```
Subject: FYI - updated rosters are in the shared drive

Team,

Updated November rosters are now in the shared drive folder.
No action needed unless you spot a clash.

Thanks,
Admin
```

---

## 验证清单

跑完后检查:

- [ ] ①~⑤ 在 Gmail **Drafts** 里有草稿,收件人=测试发件人,挂在原会话
- [ ] ②⑤ 草稿顶部带 `[URGENT - OWNER TO REVIEW PERSONALLY]` 且没有乱承诺
- [ ] 你收到对应数量的 Notify Owner / Notify Skipped 通知邮件
- [ ] 摘要里没出现编造的健康细节
- [ ] Executions 页全绿;故意断网/key 填错跑一次,Error Alert 有告警
