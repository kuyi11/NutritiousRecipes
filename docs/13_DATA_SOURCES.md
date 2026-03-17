# 13 Data Sources

## 目标
说明项目中数据从哪里来、哪些可以 AI 辅助、哪些必须人工校对。

本项目数据主要分为三类：
1. 食材营养数据
2. 菜谱内容数据
3. 营养知识内容数据

## 一、食材营养数据来源

### 优先级 1：中国权威来源
优先使用中国相关食物营养资料，原因：
- 更接近日常食材
- 更符合目标用户
- 更适合中国饮食场景

### 优先级 2：国际权威来源
作为补充使用：
- USDA FoodData Central 等

### 使用原则
- 食材营养值统一为每 100g 或每 100ml
- 不混用 serving 制
- 优先采用结构化数据源
- 录入到 ingredient 主表时统一单位

## 二、菜谱数据来源

### 当前策略
用户手工录入为主：
- meal item 名称与类型
- 1 人份基础食材
- 是否有 recipe
- 如有 recipe，则录入步骤与步骤图

AI 仅作为可选辅助：
- 食材草稿整理
- 步骤草稿润色
- 步骤图 prompt 草稿

人工负责：
- 校对步骤是否合理
- 校对食材搭配是否符合家常逻辑
- 校对是否适合项目定位
- 校对步骤图是否与步骤一致

### 菜谱数据来源说明
首版不依赖大规模外部菜谱平台全量导入。

采用“手工录入为主，AI 可选辅助”的方式建立 meal items 和 recipes。

补充说明：
- 不是每个 meal item 都必须有 recipe
- `breakfast_combo` 默认不要求正式 recipe
- 只要 `has_recipe = true`，就必须提供步骤图
- 首版不把时间、难度、食材数、特殊厨具、复杂工序设为硬性规则

### 建议的最小录入信息
建议每条内容至少补齐：
- meal item 名称
- meal item 类型
- 1 人份基础食材与数量
- 是否有 recipe
- 若有 recipe：步骤、步骤图

可选但推荐补齐：
- estimated_time_min
- difficulty
- notes
- 特殊厨具说明（如确有需要）

## 三、营养知识内容来源

### 推荐来源类型
- 中国权威营养资料
- 国际可信营养资料
- 教育类、科普类权威内容

### 使用方式
1. 收集权威资料
2. 提取核心事实
3. AI 帮助整理为统一格式
4. 人工最终校对

### 原则
- AI 不直接作为最终事实来源
- AI 只用于结构化、总结和初稿整理
- 所有关键营养描述需要人工确认

## 四、Food Library 内容来源
Food Library 主要依赖：
- ingredient 主表营养数据
- 结合人工撰写的简短说明
- AI 协助生成总结文案和结构化描述

例如：
- highlight_nutrients
- simple_tips
- caution_notes

这些内容可以 AI 辅助生成，但要人工检查是否符合项目定位。

## 五、哪些内容可以 AI 辅助

### 可以 AI 辅助的内容
- recipe 步骤草稿
- recipe 步骤图 prompt
- food summary 初稿
- nutrient topic 初稿
- 食物亮点文案
- 搜索关键词补充草稿

### 必须人工确认的内容
- 食材营养值
- 每 100g 基础数据
- 风险提示是否过界
- 营养素作用是否表述准确
- 是否出现医疗化措辞
- recipe 是否真的可执行

## 六、数据统一原则

### 1. 单位统一
- 固体：g
- 液体：ml
- 可数单位：piece（同时可配置显示转换）

前后端职责：
- 后端按 `backend_unit` 统一存储和计算
- 后端根据 `frontend_unit`、`backend_to_frontend_ratio` 输出展示值
- 前端直接展示后端返回的换算结果，不自行做业务换算

换算公式：
- `frontend_amount = backend_amount / backend_to_frontend_ratio`
- `serving_count = backend_amount / serving_size_backend_amount`

### 2. 营养值统一
- 统一按每 100g / 100ml 存储
- meal item 再换算为 1 人份基础量

补充：
- `day_plan_summary` 中的份数字段由后端按克重自动换算得到
- 具体换算依赖 `serving_size_backend_amount`

### 3. 名称统一
- 同一个食材应统一主名
- aliases 只做搜索和展示兼容

### 4. 标签字段统一
- 多值标签字段统一使用 `|` 连接
- 枚举类字段应优先复用 `docs/04_DATA_MODEL.md` 中的首版字段字典
- 不建议录入时自由发明新的枚举值

例如：
- 主名：番茄
- 别名：西红柿

## 七、录入模板

首版建议使用 `data/imports/` 下的模板文件逐步录入：
- `ingredient_template.csv`
- `meal_item_template.csv`
- `meal_item_ingredient_refs_template.csv`
- `recipe_template.csv`
- `recipe_steps_template.csv`

录入顺序建议：
1. 先录 `ingredient`
2. 再录 `meal_item`
3. 再录 `meal_item_ingredient_refs`
4. 如需要，再录 `recipe`
5. 最后录 `recipe_steps`

## 八、后期可能增加的数据来源
未来可考虑增加：
- 更系统的中国食物成分数据导入
- 第三方公开营养数据库
- 更多高频食材与 meal item 数据
- 更完整的 nutrient topic 内容

但首版不依赖大规模自动导入。
