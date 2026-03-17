# Data Directory

## 目的
本目录用于管理项目的数据资产与数据录入材料。

当前阶段这里主要承担两类职责：
1. 为后续数据录入提供模板
2. 为后续实现预留正式数据存放位置

## 子目录职责

### `imports/`
存放手工录入模板和导入准备文件。

当前主要用于：
- `ingredient` 模板
- `meal_item` 模板
- `recipe` 模板
- 关联表模板

### `seeds/`
存放后续可直接进入系统的正式种子数据。

未来适合放：
- 首版高频食材
- 首版 meal item
- 首版 recipe
- 首版 food library
- 首版 nutrient knowledge

### `mappings/`
存放各种映射关系和中间转换表。

未来适合放：
- 外部数据源字段映射
- 别名映射
- 分类映射
- 标签映射

## 当前状态
当前项目仍处于需求与录入规则收敛阶段。

因此：
- `imports/` 已开始承载录入模板
- `seeds/` 和 `mappings/` 目前仍是占位目录

## 使用原则
- 录入规则优先参考 `docs/04_DATA_MODEL.md`
- 数据来源和单位换算规则优先参考 `docs/13_DATA_SOURCES.md`
- 已确认的数据边界以 `docs/15_DECISIONS.md` 为准
