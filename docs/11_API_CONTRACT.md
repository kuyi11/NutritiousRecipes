# 11 API Contract

## 目标
定义前后端在首版阶段的主要接口边界。

说明：
- 这里是“需求级接口契约”
- 不要求现在写死全部技术细节
- 用于统一前后端讨论

## 一、设计原则
1. 业务逻辑在后端
2. 前端不自己计算推荐结果
3. 前端只做展示、交互和缓存
4. 返回结构尽量稳定、清晰
5. 允许后期扩展字段，但不轻易改基础结构

## 二、核心接口列表

### 1. 获取今日 day plan
`GET /api/day-plan/today`

#### 请求参数（当前建议）
- people_count
- disliked_ingredient_ids（可选）

#### 预留但非首版必做参数
- goal

说明：
- 当前首版先以“默认均衡模式”为主
- 首版只支持生成“今天”的 day plan
- `goal` 可先保留扩展空间，但不要求在首版必须生效
- `disliked_ingredient_ids` 应来自当前 ingredient 列表的搜索选择结果
- 首版不支持自由文本排除输入
- `refresh_count` 按北京时间自然日计算
- 每天 00:00~23:59 视为同一天
- 每个自然日最多刷新 3 次
- 同一天内即使修改 `people_count` 或 `disliked_ingredient_ids`，也共用当天刷新额度
- 达到当天刷新上限后，后端不再生成新结果，而是返回当天最后保存结果

#### 返回结构（示意）
```json
{
  "date": "2026-03-17",
  "people_count": 4,
  "disliked_ingredient_ids_snapshot": ["ing_09"],
  "generation_status": "success",
  "request_applied": true,
  "breakfast": [
    { "id": "mi_001", "name": "牛奶+鸡蛋", "type": "breakfast_combo", "image_url": "" }
  ],
  "lunch": [
    { "id": "mi_101", "name": "土豆焖鸡", "type": "dish", "image_url": "" },
    { "id": "mi_102", "name": "清炒西兰花", "type": "dish", "image_url": "" }
  ],
  "dinner": [
    { "id": "mi_201", "name": "玉米胡萝卜排骨汤", "type": "soup", "image_url": "" }
  ],
  "recommendation_reasons": [
    "今日三餐覆盖了主食、蛋白质和蔬菜来源",
    "已按 4 人用餐场景调整组合"
  ],
  "shopping_list": [
    { "ingredient_id": "ing_01", "name": "鸡蛋", "amount": 2, "unit": "piece", "display_text": "2个" }
  ],
  "summary": {
    "total_kcal": 1800,
    "total_protein": 85,
    "total_fat": 60,
    "total_carbs": 210,
    "vegetable_count": 3,
    "fruit_count": 1,
    "protein_source_tags": ["egg", "chicken"]
  },
  "refresh_count": 1,
  "max_refresh_count": 3
}
```

说明：
- 返回的 `refresh_count` 对应当前自然日已使用的刷新次数
- 如果同一天切换了 `people_count` 或 `disliked_ingredient_ids`，仍消耗同一天的刷新额度
- 同一天保存的 day plan 结果始终以最后一次成功生成并停留在页面上的结果为准
- `summary` 中的 `vegetable_count` 和 `fruit_count` 由后端按克重自动换算为份数
- 购物清单中的 `amount`、`unit`、`display_text` 由后端完成单位换算后返回，前端直接展示

生成状态语义：
- `generation_status = success`：本次请求条件已成功生效，并生成了新的今日方案
- `generation_status = refresh_blocked`：今日刷新次数已用完，返回当天最后保存结果，本次新条件未生效
- `generation_status = no_valid_plan`：当前条件下无法生成完整且合规的新方案
- `request_applied = true` 表示本次请求条件已生效
- `request_applied = false` 表示本次请求条件未生效，前端应明确提示用户

失败处理约定：
- 若 `generation_status = refresh_blocked`，后端返回当天最后保存结果，并附带提示文案
- 若 `generation_status = no_valid_plan` 且当天已有成功生成结果，后端返回当天原结果，并附带提示文案
- 若 `generation_status = no_valid_plan` 且当天尚无成功生成结果，后端可返回空餐数组与提示文案，且不保存新的 day plan

### 2. 获取最近 7 天历史 day plan
`GET /api/day-plan/history`

#### 请求参数（当前建议）
- days（可选，默认 7，首版最大 7）

#### 返回结构（示意）
```json
{
  "items": [
    {
      "date": "2026-03-17",
      "summary": {
        "total_kcal": 1800,
        "protein_source_tags": ["egg", "chicken"]
      },
      "breakfast_count": 1,
      "lunch_count": 2,
      "dinner_count": 2
    }
  ]
}
```

说明：
- 历史列表中每个日期只返回 1 条记录
- 若当天曾刷新多次，只保留当天最后一次结果
- 首版历史列表只返回已经生成并保存过的日期，不生成未来日期

### 3. 获取某一天的 day plan 详情
`GET /api/day-plan/by-date`

#### 请求参数（当前建议）
- date

说明：
- 该接口返回该日期最终保存的结果快照
- 返回内容应包含当时保存的 `people_count` 与 `disliked_ingredient_ids_snapshot`
- 首版不要求基于历史记录重新生成或编辑
- 该接口只用于读取历史已保存日期，不用于生成未来日期计划
