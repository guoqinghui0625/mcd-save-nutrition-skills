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

### 示例 1：查券领券（真实接口行为）

用户：今天能领哪些麦麦省优惠券？顺手帮我领了。

AI 调用 `available-coupons` 返回你账户里的券（每张带领取状态），再调用 `auto-bind-coupons` 一键领取「待领」券：

```
你券包里现有这些券（状态：已领取）：
- 免费脆薯饼
- 人气麦旋风买一送一
- 9.9 元中杯冰美式
- 巧克力厚松饼猪柳蛋套餐（早餐）

一键领取新券结果：暂无可领取的优惠券（待领的都已领过）。
```

> 注：`auto-bind-coupons` 领的是「待领」券，不会凭空生成新券；若已领完会提示「暂无可领取」，属正常结果。

### 示例 2：按预算凑单

用户：我 30 块预算，帮我凑最划算的。

AI 调用 `query-meals`、`query-store-coupons`、`calculate-price` 后返回 2-3 套方案，并标出券后实付价。

### 示例 3：按热量推荐（真实实测数据）

用户：减脂期，帮我配一套 500 大卡以内的麦当劳。

AI 调用 `list-nutrition-foods`（取真实热量）→ 映射到 `query-meals` 的 productCode → 调 `calculate-price` 验算，返回：

```
减脂方案（实测 401 大卡 / ¥31.5）：
- 麦香鸡 369 kcal
- 苹果片一袋 32 kcal
- 纯悦 0 kcal

对比常点的经典组合（实测 806 大卡 / ¥43.5）：
- 巨无霸 + 可乐 + 脆薯饼

减脂期推荐第一套：热量少一半，还便宜 12 块。
```

---

## 真实实测记录（非虚构，可复现）

2026-10-10 已用真实麦当劳 MCP 跑通「按预算智能凑单」全流程，门店：**麦当劳海口国贸餐厅（storeCode 1420187，到店自取）**。

**查到的门店券（query-store-coupons）：**
- `9.9元中杯冰美式` — couponId `264B97DBE02BE6CF996AA6914151C4A9`，couponCode `MCD6001169060R1830CKU`，适用「麦咖啡™冰美式中杯」，有效期 2026-10-10 ~ 10-16

**算价结果（calculate-price，单位：元）：**

| 方案 | 组合 | 是否用券 | 实付价 |
|---|---|---|---|
| A | 麦辣鸡腿汉堡 + 可乐 | 否 | **32.0** |
| B | 麦咖啡冰美式中杯 | 是（9.9 券） | **9.9**（券前 11.9，省 2.0） |
| C | 麦香鸡 + 中薯条 | 否 | **30.5** |

**实测结论：** 四个工具（查门店 / 查菜单 / 查门店券 / 算价）链路全部跑通，返回均为麦当劳官方实时数据。期间发现并修复一个接口细节：`calculate-price` 用券必须同时传 `couponId` + `couponCode`，已在 `SKILL.md` 标注。

### 实测补充：一键领券 + 营养配餐（2026-10-10）

**① 一键领券（auto-bind-coupons）**
- 调用 `available-coupons`：返回账户券包，全部为「已领取」状态（免费脆薯饼、人气麦旋风买一送一、9.9 元中杯冰美式、巧克力厚松饼猪柳蛋套餐等）。
- 调用 `auto-bind-coupons`：**返回「暂无可领取的优惠券」**。说明该接口领的是账户「待领」券而非凭空生成，券领完后属正常结果，已在 `SKILL.md` 流程 A 标注。

**② 营养配餐（list-nutrition-foods + calculate-price）**
- `list-nutrition-foods` 返回 **160 个餐品**的真实营养数据（能量/蛋白质/脂肪/碳水/钠/钙），但**只含产品名、不含 productCode**。
- 建立「营养产品名 → 菜单 productCode」映射后，算价实测：

| 方案 | 组合（productCode） | 热量（营养表） | 实付价（calculate-price） |
|---|---|---|---|
| 减脂方案 | 麦香鸡`1450`+苹果片`6102`+纯悦`3757` | 369+32+0 = **401 kcal** | **¥31.5** ✅ |
| 经典对照 | 巨无霸`1100`+可乐`9900008751`+脆薯饼`521741` | 513+147+146 = **806 kcal** | **¥43.5** ✅ |

- 价格与菜单现价完全吻合，热量与营养表完全吻合，流程 C 链路跑通。

**全部 6 个核心工具实测状态：**

| 工具 | 实测结果 |
|---|---|
| `query-nearby-stores` | ✅ 海口 5 家门店 |
| `query-meals` | ✅ 全菜单实时价 |
| `query-store-coupons` | ✅ 门店可用券 |
| `calculate-price` | ✅ 算价 + 用券双字段坑已修复 |
| `available-coupons` / `auto-bind-coupons` | ✅ 查券 + 一键领（返回「暂无可领取」正常） |
| `list-nutrition-foods` | ✅ 160 餐品营养，需映射 productCode |

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
