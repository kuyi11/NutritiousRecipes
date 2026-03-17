# Assets Directory

## 目的
本目录用于放项目中的非代码资源。

这类资源包括：
- 页面图片
- 食材图片
- 菜谱步骤图
- AI 辅助生成使用的 prompt

## 子目录职责

### `images/`
存放面向用户展示的图片资源。

未来适合放：
- meal item 图片
- food library 图片
- nutrient topic 图片
- recipe 步骤图

### `prompts/`
存放图片生成或内容辅助生成时使用的 prompt 文本。

未来适合放：
- recipe 步骤图 prompt
- food 图片 prompt
- nutrient 插图 prompt

## 当前状态
本目录当前仍以占位为主。

原因是：
- 首版内容和录入规则还在收敛
- 图片资产应跟随正式内容一起整理

## 使用原则
- 图片与 prompt 的内容边界参考 `docs/14_CONTENT_RULES.md`
- 是否必须有步骤图，参考 `docs/02_CORE_RULES.md` 和 `docs/15_DECISIONS.md`
