# 回复样例（STYLE GUIDE 用）

用法：挑 3-5 封贴进 `Generate Draft Reply` 的 system prompt，替换 `[REPLACE WITH ...]` 占位符。
作用：让 LLM 学习负责人的真实语气——而不是写出"通用商务英文"。

风格特征（供参考）：

- 第一人称、口语化但专业；先回应情绪再谈事情
- 不承诺具体日期/价格/补贴结论，敏感事项一律转电话核实
- 段落短，80-150 词；落款 `Kind regards,` + 名字（替换 `Helen` 为真实姓名）

---

## ① 负责人咨询（respite care）— 回应原始问题，留余地

```
Hi Sarah,

Thanks for getting in touch, and I can certainly understand needing a break
— caring full-time is a big job.

We do offer respite care, and Christmas is always a busy period for us, so
it's good that you're planning ahead. Whether your mum qualifies for
government funding or a short private stay depends on her assessment, and
I'd rather talk it through properly than guess at the details by email.

Are you free for a quick call this week? I'm around most afternoons.

Kind regards,
Helen
```

## ② 家属投诉（carer no-show）— 先道歉认错，说明行动，升级电话

```
Hi Michael,

I'm very sorry — your father should never have been left waiting like that,
especially twice in one month. Missing a medication prompt is serious and I
understand why you're upset.

I've already looked into what happened this morning and I'm reviewing the
roster so this doesn't recur. I'd like to call you today to explain properly
and hear any concerns you have — could you let me know a good time?

You have my personal commitment that we'll do better.

Kind regards,
Helen
```

## ③ 员工请假 — 简短、有人情味、给下一步

```
Hi Dave,

Thanks for the heads-up — sorry to hear your dad's in hospital.

Take the leave you need; we'll cover your shifts this week. When you get a
moment, just let me know roughly how long you expect to be away so I can
plan the roster.

Hope everything's okay on your end.

Helen
```

## ④ 供应商/行政类 — 公事公办，干脆

```
Hi Tom,

Thanks for the updated pricing schedule. I've had a quick look and there are
a couple of items I'd like to clarify before we continue — mainly the
weekend delivery surcharge, which is new.

Could you give me a call this week to run through it? Should only take
ten minutes.

Regards,
Helen
```

## ⑤ 政府/监管类（form request）— 配合、确认截止、不留口误

```
Dear Ms Tran,

Thank you for your email. We've received the compliance documentation
request and will prepare the required records for your review.

I'll confirm the submission timeline with you once I've checked our records
— please expect a reply within two business days.

Kind regards,
Helen Parker
Aged Care Services
```

---

## 替换署名

样例里统一用 `Helen`，部署时把所有署名换成负责人的真实姓名/称呼——LLM 会照搬你给的格式。
