# 麦麦省钱营养师 🍔

基于 **麦当劳中国 MCP** + **WorkBuddy** 开发的创意 Skill，帮打工人省钱吃麦当劳。

> 参赛作品：2026 麦当劳程序员创意开发大赛
> 仓库地址：https://github.com/guoqinghui0625/mcd-save-nutrition-skills

---

## 项目简介

每次想吃麦当劳，又怕点贵了？

「麦麦省钱营养师」用 AI 调用麦当劳官方 MCP 接口，帮你：
- 一键查/领可优惠券
- 按预算智能凑出最划算套餐
- 按热量/营养目标推荐餐品

所有数据来自麦当劳官方 MCP，不是网页抓取，也不是 AI 瞎编。

---

## 核心功能

| 功能 | 场景 | 用到的麦当劳 MCP 工具 |
|---|---|---|
| **查券领券** | 「今天能领啥券？」 | `available-coupons`、`query-my-coupons`、`auto-bind-coupons` |
| **智能凑单** | 「30 块能吃啥？」 | `query-nearby-stores`、`query-meals`、`query-store-coupons`、`calculate-price` |
| **营养推荐** | 「减脂期怎么吃？」 | `list-nutrition-foods`、`query-meals`、`calculate-price` |

---

## 目标用户

- 经常吃麦当劳的上班族、学生党
- 想省钱但又不想费脑子凑单的普通消费者
- 健身/减脂期还想偶尔吃顿麦当劳的人

---

## 安装与使用

### 1. 申请麦当劳 MCP Token

访问 https://open.mcd.cn/mcp
- 用手机号登录 → 控制台 → 激活 → 复制 Token

### 2. 在 WorkBuddy 配置麦当劳 MCP

打开 WorkBuddy → 左侧「专家·技能·连接器」→「连接器」→「自定义连接器」→「配置 MCP」

填入 `mcp-config.example.json` 中的配置（把 `${MCD_MCP_TOKEN}` 替换成你的真实 Token），保存并启用 mcd-mcp。

### 3. 导入本 Skill

将本项目中的 `SKILL.md` 内容作为 WorkBuddy Skill 使用，即可在对话中直接说：

> 「帮我查一下今天能领哪些券」
> 
> 「我预算 30 块，帮我凑一单麦当劳」
> 
> 「减脂期想吃麦当劳，给我推荐一套 500 大卡以内的」

---

## 使用示例

### 示例 1：查券领券

用户：今天能领哪些麦麦省优惠券？

AI 调用 `available-coupons` 和 `query-my-coupons` 后返回：
```
你现在可领 4 张券：
- 免费脆薯饼 ×1
- 人气麦旋风买一送一 ×1
- 9.9 元中杯冰美式 ×1
- 巧克力厚松饼猪柳蛋套餐（早餐）×1

卡包里暂无，要不要一键全领了？
```

### 示例 2：按预算凑单

用户：我 30 块预算，帮我凑最划算的。

AI 调用 `query-meals`、`query-store-coupons`、`calculate-price` 后返回 2-3 套方案，并标出券后实付价。

### 示例 3：按热量推荐

用户：减脂期，帮我配一套 500 大卡以内的麦当劳。

AI 调用 `list-nutrition-foods` 和 `query-meals` 后推荐「板烧鸡腿堡（去酱）+ 玉米杯 + 无糖可乐」等组合。

---

## 项目文件说明

| 文件 | 说明 |
|---|---|
| `SKILL.md` | WorkBuddy Skill 本体，定义 AI 调用逻辑和输出风格 |
| `MCP_INTEGRATION.md` | 麦当劳 MCP 接入说明，含工具链路和业务价值 |
| `mcp-config.example.json` | 脱敏的 MCP 配置示例（仅含占位符） |
| `CONTEST_DECLARATION.md` | 官方参赛声明，原样未改 |
| `workbuddy.md` | 使用 WorkBuddy 开发本项目的对话上下文摘要 |

---

## 参赛声明

详见 [CONTEST_DECLARATION.md](./CONTEST_DECLARATION.md)。

本项目为 2026 麦当劳程序员创意开发大赛参赛作品，非麦当劳官方产品。餐品信息、价格、优惠以麦当劳官方渠道实时结果为准。
