# 08 Food Library

## 目标
Food Library（食物库）以“单个食物”为视角，为用户提供：

1. 这个食物主要含什么营养
2. 每 100g 大致有哪些营养值
3. 可以跳转到相关营养素知识页

Food Library 的定位是：
- 服务于菜谱系统
- 提供食物层面的营养解释
- 不做全量食品百科

## 一、首版范围
首版只做：
- 30~50 个高频食物
- 以菜谱中高频出现的食材为主
- 每个食物提供基础信息和主要营养标签

首版不做：
- 冷门食物大规模扩充
- 品牌商品库
- 零食 / 加工食品大全
- 扫码识别

## 二、食物库的定位

### 当前定位
Food Library 是“食物层内容模块”，不是系统唯一主数据来源。

### 正确关系
- `ingredient`：系统内部主数据表
- `food_profile`：面向用户展示的食物详情扩展层

也就是说：
- recipe、购物清单、推荐逻辑都引用 `ingredient`
- 食物库页展示 `food_profile + ingredient` 的组合信息

## 三、食物详情页展示什么
建议每个食物详情页展示：

### 1. 基础信息
- 食物名称
- 图片
- 分类
- 简介

### 2. 每 100g 营养
- 热量
- 蛋白质
- 脂肪
- 碳水
- 膳食纤维（如适用）
- 关键微量营养标签

### 3. 主要营养亮点
例如：
- 高蛋白
- 富含维生素 C
- 含膳食纤维
- 含钙

### 4. 简单食用说明
例如：
- 常见做法
- 常见搭配
- 适合作为早餐 / 配菜 / 汤料等

### 5. 简单注意事项
例如：
- 糖分较高，适量食用
- 钠较高，不建议过量

### 6. 关联营养素
例如：
- 鸡蛋 -> 蛋白质、脂肪
- 凤梨 -> 维生素 C、膳食纤维

## 四、食物列表页展示建议
每个食物卡片展示：
- 图片
- 名称
- 2~3 个主要营养标签

例如：
- 凤梨：维生素 C / 纤维
- 鸡蛋：蛋白质 / 脂肪
- 西兰花：维生素 C / 纤维

## 五、搜索设计
首版支持：
- 按食物名称搜索
- 按别名搜索
- 按营养标签关键词搜索

### 示例关键词
- 鸡蛋
- 牛奶
- 高蛋白
- 维 C
- 纤维

## 六、建议字段

### ingredient（系统主数据）
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

### food_profile（展示扩展）
- ingredient_id
- image_url
- summary
- highlight_nutrients
- simple_tips
- caution_notes
- related_nutrient_ids

## 七、与其他模块的关系

### 与 recipe 的关系
recipe 中引用 ingredient。

### 与 shopping list 的关系
购物清单按 ingredient 汇总。

### 与 nutrient knowledge 的关系
Food Library 中的食物详情页应可链接到相关营养素页面。

## 八、首版建议收录方向
首版优先收录：
- 菜谱中高频食材
- 早餐组合中高频食材
- 典型蛋白来源
- 典型碳水来源
- 典型蔬菜来源
- 典型水果补充来源

例如：
- 鸡蛋
- 牛奶
- 鸡胸肉
- 牛肉
- 豆腐
- 土豆
- 玉米
- 西兰花
- 胡萝卜
- 番茄
- 凤梨
- 香蕉
