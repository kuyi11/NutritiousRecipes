# 10 Offline Strategy

## 目标
App 首版支持 L1 离线能力，保证在无网或弱网时仍有基本可用性。

离线能力的目标不是“完整离线生成新三餐”，而是：
1. 不白屏
2. 能看最近内容
3. 能看最近 7 天 day plan
4. 能查看高频知识和食物信息

---

## 一、首版离线范围（L1）

### 支持离线查看
- 最近 7 天 day plan
- 最近 7 天 day plan summary
- 已缓存的 recipe 详情
- 高频 food library 内容
- 高频 nutrient knowledge 内容

### 不支持离线生成
首版不做：
- L2 离线生成新三餐
- 离线锁定某道菜后重新补全
- 复杂离线搜索排序
- 本地完整推荐引擎替代后端

---

## 二、离线优先级

### 一级：必须缓存
1. 最近 7 天 day plan
2. 最近 7 天 day plan summary

### 二级：高频缓存
1. 高频 breakfast_combo
2. 高频 meal items
3. 高频 food library
4. 高频 nutrient topics

### 三级：按用户访问缓存
1. 用户最近打开过的 recipe
2. 用户最近打开过的 food
3. 用户最近打开过的 nutrient topic

---

## 三、离线状态下的行为

### 1. 首页
如果离线：
- 优先展示最近一次已缓存的今日 day plan
- 若无今日数据，则展示最近一天缓存的 day plan
- 提示当前为离线状态

### 1.1 历史浏览
如果离线：
- 用户仍可查看最近 7 天内已缓存的历史 day plan
- 若某天记录存在对应 summary，也应一并展示
- 历史浏览为只读模式

### 2. recipe 详情
如果已缓存：
- 正常展示
如果未缓存：
- 显示“该内容未缓存，需要联网查看”

### 3. food library / nutrient knowledge
如果已缓存：
- 正常查看
如果未缓存：
- 显示离线提示

### 4. 搜索
首版支持：
- 本地搜索缓存数据
- 不要求离线状态下搜全量内容

---

## 四、本地缓存建议内容

### day_plan
建议缓存字段：
- id
- date
- people_count
- breakfast_meal_item_ids
- lunch_meal_item_ids
- dinner_meal_item_ids
- disliked_ingredient_ids_snapshot
- recommendation_reasons
- shopping_list_snapshot
- refresh_count

说明：
- 同一天缓存只保留最后一次结果快照

### day_plan_summary
建议缓存字段：
- total_kcal
- total_protein
- total_fat
- total_carbs
- total_fiber
- vegetable_count
- fruit_count
- protein_source_tags
- vitamin_coverage_tags

说明：
- `vegetable_count` 和 `fruit_count` 由后端按克重自动换算为份数

### meal item 详情缓存
建议缓存：
- id
- name
- type
- ingredient_refs
- nutrition_per_person
- image_url
- has_recipe

### recipe 缓存
建议缓存：
- meal_item_id
- estimated_time_min
- difficulty
- step_count
- steps
- step_images
- notes

### food library 缓存
建议缓存：
- ingredient_id
- summary
- highlight_nutrients
- simple_tips
- caution_notes
- image_url

### nutrient knowledge 缓存
建议缓存：
- name
- short_summary
- body_role
- common_sources
- deficiency_notes
- excess_notes
- warning_tips
- subtype_notes
- image_url

---

## 五、缓存更新策略

### 在线时
- 打开首页时同步最新今日 day plan
- 打开详情页时更新对应缓存
- 可在后台轻量刷新高频 food / nutrient 数据

### 离线时
- 不尝试强制刷新
- 仅使用已缓存内容
- UI 明确提示“当前为离线内容”

说明：
- 历史 day plan 浏览优先复用最近 7 天缓存
- 该能力是首版离线体验的重要组成部分
- 若某天有过多次刷新，离线历史中只保留当天最后一次结果

---

## 六、缓存失效策略

### 建议失效规则
- day plan / summary：保留最近 7 天
- recipe：LRU 或保留最近访问的 N 条
- food library：优先保留高频条目
- nutrient knowledge：优先保留高频条目

### 图片缓存
- 首版使用系统缓存为主
- 不主动做复杂图片离线包

---

## 七、后期升级方向（L2）

未来如需支持 L2，可考虑：
1. 本地保存可推荐的 meal item 池
2. 本地实现简化版推荐逻辑
3. 本地基于历史缓存生成新 day plan

但这些不在首版范围内。
