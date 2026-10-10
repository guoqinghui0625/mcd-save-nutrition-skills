# 使用 WorkBuddy 开发「麦麦省钱营养师」过程记录

## 开发环境

- WorkBuddy 版本：5.7.7
- 配置时间：2026-10-10
- 模型：Deepseek-V4.1-Flash / Kimi-K3
- 连接器：mcd-mcp（麦当劳中国官方 MCP）

## 关键对话节点

### 1. 环境搭建
- 在 WorkBuddy 中通过「专家·技能·连接器」→「自定义连接器」→「配置 MCP」接入 mcd-mcp。
- 配置文件路径：`C:\Users\LENOVO\.workbuddy\mcp.json`
- 连接成功后显示：`mcd-mcp 35/35 个工具已启用`。

### 2. MCP 首次调通
- 用户输入：`请你调用mcd-mcp 然后：帮我查一下我现在能领哪些麦麦省优惠券`
- AI 调用 `available-coupons` 和 `query-my-coupons` 成功。
- 返回结果示例：
  - 巧克力味厚松饼猪柳蛋套餐（早餐）— 可领取 × 多张
  - 免费脆薯饼 — 可领取
  - 人气麦旋风·买一送一 — 可领取
  - 9.9 元中杯冰美式 — 可领取
- 确认为用户账号实时数据，非网络搜索结果。

### 3. Skill 开发思路
- 方向：「麦麦省钱营养师」——省钱 + 营养二合一。
- 核心能力：查券领券、按预算凑单、按热量/营养目标推荐。
- 输出风格：口语化、单行 ≤16 字、先给结论再给理由、结尾带行动建议。

### 4. 项目文件生成
- 在 WorkBuddy 辅助下，于本地生成：
  - `SKILL.md`
  - `README.md`
  - `MCP_INTEGRATION.md`
  - `mcp-config.example.json`
  - `CONTEST_DECLARATION.md`（官方原样复制）
  - `workbuddy.md`

## 后续计划

- 将本项目文件上传至 GitHub 公开仓库。
- 在麦当劳官方活动仓库提交 Issue 报名。
- 继续用 WorkBuddy 实测「按预算凑单」和「按热量推荐」两个核心流程。
