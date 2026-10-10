# 麦当劳 MCP 接入说明

## 使用的 MCP Server

- **Server 名称**：mcd-mcp
- **提供方**：麦当劳中国（金拱门）
- **接入地址**：https://mcp.mcd.cn
- **传输协议**：Streamable HTTP
- **认证方式**：Bearer Token（由麦当劳开放平台申请）

## 接入配置

实际配置中，需要在 `headers.Authorization` 填入麦当劳 MCP Token：

```json
{
  "mcpServers": {
    "mcd-mcp": {
      "type": "streamablehttp",
      "url": "https://mcp.mcd.cn",
      "headers": {
        "Authorization": "Bearer ${MCD_MCP_TOKEN}"
      }
    }
  }
}
```

> 仓库中仅保存脱敏示例 `mcp-config.example.json`，真实 Token 由用户在本地 WorkBuddy 中配置，不进入仓库。

---

## 使用的工具清单

| 工具名 | 功能 | 在 Skill 中的作用 |
|---|---|---|
| `available-coupons` | 查询当前可领取优惠券 | 告诉用户今天能领哪些券 |
| `query-my-coupons` | 查询用户卡包已有券 | 避免重复推荐已领取的券 |
| `auto-bind-coupons` | 一键领取所有可领券 | 帮用户批量领券 |
| `query-nearby-stores` | 查询附近麦当劳门店 | 确定用户可选的门店 |
| `query-meals` | 查询门店菜单和价格 | 为凑单/推荐提供数据源 |
| `query-meal-detail` | 查询单个餐品详情 | 查看套餐组成、可替换项 |
| `query-store-coupons` | 查询门店可用券 | 计算券后价 |
| `calculate-price` | 计算订单价格 | 对比多种凑单方案的实付价 |
| `list-nutrition-foods` | 查询餐品营养信息 | 按热量/营养目标推荐 |

---

## 典型调用流程

### 流程 1：查券领券
```
用户：今天能领哪些券？
  ↓
AI 调用 available-coupons
  ↓
AI 调用 query-my-coupons
  ↓
AI 整理成「可领」和「已有」清单
  ↓
用户：帮我领了
  ↓
AI 调用 auto-bind-coupons
```

### 流程 2：按预算凑单
```
用户：30 块预算，帮我凑一单
  ↓
AI 调用 query-nearby-stores 确定门店
  ↓
AI 调用 query-meals 获取菜单
  ↓
AI 调用 query-store-coupons 获取门店券
  ↓
AI 组合 2-3 套方案
  ↓
AI 调用 calculate-price 计算每套券后价
  ↓
AI 给出最省钱方案
```

### 流程 3：按热量/营养推荐
```
用户：减脂期，500 大卡以内怎么吃？
  ↓
AI 调用 list-nutrition-foods 获取营养数据
  ↓
AI 调用 query-meals 获取当前菜单
  ↓
AI 筛选符合热量目标的组合
  ↓
AI 调用 calculate-price 计算价格
  ↓
AI 给出推荐搭配
```

---

## 业务价值

1. **省钱**：自动帮用户发现可领未领的优惠券，避免错过羊毛。
2. **省脑**：给定预算或目标，AI 直接给出 2-3 套可行方案，不用自己翻 App 凑。
3. **健康参考**：把营养数据加入点餐决策，满足健身/减脂人群的需求。
4. **真实可信**：所有价格、优惠、营养数据均来自麦当劳官方 MCP，不是爬虫或猜测。

---

## 开发环境

- WorkBuddy 5.7.7
- 模型：Deepseek-V4.1-Flash / Kimi-K3（用于工具调用）
- 开发过程对话记录见 `workbuddy.md`
