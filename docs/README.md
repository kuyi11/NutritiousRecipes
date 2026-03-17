# Docs Guide

## 目的
本目录用于管理项目的：
- 产品需求
- 核心规则
- 数据结构
- 推荐逻辑
- 内容规范
- 技术边界

当前阶段最重要的任务是：

> 先把需求讲清楚，再进入架构和实现

## 阅读顺序
所有新参与者 / AI 应按顺序阅读：
1. `00_PROJECT_OVERVIEW.md`
2. `01_PRODUCT_SCOPE.md`
3. `02_CORE_RULES.md`
4. `03_GLOSSARY.md`
5. `04_DATA_MODEL.md`
6. `05_MEAL_ITEM_SPEC.md`

## 文档分类

### 产品层
- `00_PROJECT_OVERVIEW.md`
- `01_PRODUCT_SCOPE.md`
- `02_CORE_RULES.md`

作用：定义项目是什么、首版做什么、核心规则是什么。

### 概念层
- `03_GLOSSARY.md`

作用：统一概念，避免术语混乱。

### 数据层
- `04_DATA_MODEL.md`
- `05_MEAL_ITEM_SPEC.md`

作用：定义系统的核心对象与推荐单位。

### 逻辑层
- `06_RECOMMENDATION_LOGIC.md`
- `07_SHOPPING_LIST.md`

作用：定义推荐结果如何形成，以及购物清单如何生成。

### 内容层
- `08_FOOD_LIBRARY.md`
- `09_NUTRITION_KNOWLEDGE.md`
- `14_CONTENT_RULES.md`

作用：定义展示内容、内容边界和内容风格。

### 系统边界层
- `10_OFFLINE_STRATEGY.md`
- `11_API_CONTRACT.md`
- `12_ARCHITECTURE.md`

作用：定义交付方式和技术边界，但当前不是首要讨论焦点。

### 数据来源层
- `13_DATA_SOURCES.md`

作用：约束营养数据、内容数据和 AI 辅助的使用方式。

### 决策与未来层
- `15_DECISIONS.md`
- `99_FUTURE_FEATURES.md`

作用：记录已经确认的决策，以及明确暂不进入首版的能力。

## 修改规则
- 修改产品范围：更新 `01_PRODUCT_SCOPE.md`
- 修改核心规则：更新 `02_CORE_RULES.md`
- 修改数据结构：更新 `04_DATA_MODEL.md`
- 修改推荐流程：更新 `06_RECOMMENDATION_LOGIC.md`
- 修改内容边界：更新 `14_CONTENT_RULES.md`
- 形成新决策：同步写入 `15_DECISIONS.md`

## 冲突处理规则
当多个文档冲突：
1. `15_DECISIONS.md`
2. `02_CORE_RULES.md`
3. 其他文档

## 当前文档状态
当前文档已具备主线，但仍处于需求收敛阶段。

当前重点：
- 补齐需求边界
- 收束含糊点
- 修正文档冲突
- 为后续实现建立稳定事实来源

## 注意
不要：
- 在多个文档重复定义同一概念
- 在未更新规则前修改实现方向
- 绕过 docs 直接推进复杂实现

docs 是项目当前阶段的唯一事实来源。
