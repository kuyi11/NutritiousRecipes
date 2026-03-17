# 04 Data Model

## 核心实体

### 1. ingredient
系统内部统一的食材主表。

建议字段：
- id
- name
- aliases
- category
- backend_unit
- frontend_unit
- serving_size_backend_amount
- backend_to_frontend_ratio
- kcal_per_100g
- protein_per_100g
- fat_per_100g
- carbs_per_100g
- fiber_per_100g
- vitamin_tags
- mineral_tags
- search_keywords
- is_high_frequency

说明：
- `backend_unit` 用于后端统一存储和计算
- `frontend_unit` 用于前端展示
- `serving_size_backend_amount` 用于把后端克重 / 毫升 / 个数换算成“份”
- `backend_to_frontend_ratio` 用于把后端单位换算成前端展示单位
- `frontend_amount = backend_amount / backend_to_frontend_ratio`
- `serving_count = backend_amount / serving_size_backend_amount`

---

### 2. meal_item
推荐层的核心实体。

建议字段：
- id
- name
- type
- summary
- has_recipe
- meal_type_preference
- heaviness
- nutrition_role_tags
- protein_source_tags
- ingredient_refs
- nutrition_per_person
- image_url

---

### 3. recipe
只给需要详细制作过程的 meal item 使用。

建议字段：
- id
- meal_item_id
- estimated_time_min
- difficulty
- step_count
- steps
- step_images
- notes

说明：
- 首版可记录 `estimated_time_min` 和 `difficulty` 作为辅助信息
- 首版不把时间、难度、特殊厨具或复杂工序设为硬性推荐门槛

---

### 4. food_profile
食物库展示层，和 ingredient 关联。

建议字段：
- ingredient_id
- image_url
- summary
- highlight_nutrients
- simple_tips
- caution_notes
- related_nutrient_ids

---

### 5. nutrient_topic
营养知识库主表。

建议字段：
- id
- name
- category
- aliases
- short_summary
- body_role
- common_sources
- deficiency_notes
- excess_notes
- warning_tips
- subtype_notes
- related_bad_examples
- search_keywords
- image_url

---

### 6. day_plan
某天三餐的推荐结果。

建议字段：
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
- 同一天只保留一个最终 day_plan 记录
- 若当天发生多次刷新，当天记录会被最新结果覆盖，但最终只保留最后一次结果快照
- `refresh_count` 按北京时间自然日统计
- 每个自然日最多刷新 3 次
- 同一天内即使修改 `people_count` 或 `disliked_ingredient_ids`，也共用当天刷新次数
- 达到当天刷新上限后，不再生成新结果，而是返回当天最后保存结果

---

### 7. day_plan_summary
某天的营养汇总。

建议字段：
- day_plan_id
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
- 不按食材种类数累计

## 首版字段字典

说明：
- 枚举类字段在首版应尽量固定，不建议录入时自由扩展
- 多值标签字段统一使用 `|` 连接
- 若后续确需新增枚举值，应先更新 `docs/15_DECISIONS.md` 与本文件

### ingredient.category
- vegetable
- fruit
- meat
- seafood
- egg
- dairy
- bean_product
- staple
- mushroom
- nut_seed
- beverage
- seasoning
- other

### meal_item.meal_type_preference
- breakfast
- lunch
- dinner

说明：
- 多餐段适配时可使用 `|` 连接，例如 `lunch|dinner`

### meal_item.heaviness
- light
- medium
- heavy

### meal_item.nutrition_role_tags
- carb_source
- protein_source
- vegetable_source
- fruit_source
- fat_source
- soup_like

### meal_item.protein_source_tags
- chicken
- pork
- beef
- fish
- seafood
- egg
- tofu
- bean
- dairy
- duck
- lamb

### recipe.difficulty
- easy
- medium
- hard

### nutrient_topic.category
- macro
- vitamin
- mineral
- other

### ingredient.vitamin_tags / day_plan_summary.vitamin_coverage_tags
- vitamin_a
- vitamin_b
- vitamin_c
- vitamin_d
- vitamin_e
- vitamin_k
- folate

### ingredient.mineral_tags
- calcium
- iron
- zinc
- potassium
- magnesium
- sodium
- selenium
