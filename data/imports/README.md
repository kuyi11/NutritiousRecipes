# Data Import Templates

## 目的
本目录用于放首版手工录入模板。

当前推荐先录三层：
1. `ingredient`
2. `meal_item`
3. `recipe`

其中 `meal_item` 和 `ingredient` 的关系通过单独映射表维护。

## 文件说明

### 1. ingredient_template.csv
食材主数据模板。

一行代表一个 `ingredient`。

### 2. meal_item_template.csv
餐项主表模板。

一行代表一个 `meal_item`。

### 3. meal_item_ingredient_refs_template.csv
餐项和食材的关联模板。

一行代表某个 `meal_item` 中的一条食材引用。

### 4. recipe_template.csv
菜谱主表模板。

一行代表一个 `recipe`。

### 5. recipe_steps_template.csv
菜谱步骤模板。

一行代表一个步骤。

## 录入顺序
1. 先录 `ingredient`
2. 再录 `meal_item`
3. 再录 `meal_item_ingredient_refs`
4. 对需要做法的内容录 `recipe`
5. 最后录 `recipe_steps`

## 录入约定
- 布尔值使用 `TRUE` / `FALSE`
- 多值字段使用 `|` 分隔
- 可选字段可以留空
- `id` 建议保持稳定，不要随意改名

## 单位规则
- `backend_unit`：后端统一计算单位，如 `g`、`ml`、`piece`
- `frontend_unit`：前端展示单位，如 `g`、`ml`、`个`
- `backend_to_frontend_ratio`：后端转前端的换算比例
- `serving_size_backend_amount`：1 份对应多少后端单位

公式：
- `frontend_amount = backend_amount / backend_to_frontend_ratio`
- `serving_count = backend_amount / serving_size_backend_amount`

示例：
- 鸡蛋：`backend_unit=piece`，`frontend_unit=个`，`backend_to_frontend_ratio=1`
- 西兰花：`backend_unit=g`，`frontend_unit=g`，`backend_to_frontend_ratio=1`
- 牛奶：`backend_unit=ml`，`frontend_unit=ml`，`backend_to_frontend_ratio=1`

## 当前建议
- `breakfast_combo` 可以没有 `recipe`
- 只要 `has_recipe = TRUE`，就必须补 `recipe_template.csv` 和 `recipe_steps_template.csv`
- 当前建议一个步骤对应一张图；如暂时没有图，也至少先把步骤文本录完整
