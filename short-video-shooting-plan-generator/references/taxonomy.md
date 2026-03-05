# 类目分类与归一化规则

## 1）标准类目

仅使用以下标准类目值：

- `beauty-skincare`
- `personal-care-cleaning`
- `home-daily`
- `food-beverage`
- `tech-small-appliance`
- `generic`

## 2）归一化流程

1. 将 `category_raw` 转为小写。
2. 去除标点和多余空格。
3. 先按“完整词/短语边界”匹配同义词。
4. 如果匹配到多个候选，按优先级选择最高置信度类目。
5. 如果都不匹配，返回 `generic`。

不要做粗糙的子串匹配（避免误匹配）。  
示例：不要把 `teaser` 误识别为 `tea`。

如果用户消息里有明确类目字段（例如 `Category: ...`），仅对该字段做归一化，不要对整段提示词做全量猜测。

## 3）优先级规则（高到低）

1. `beauty-skincare`
2. `personal-care-cleaning`
3. `home-daily`
4. `food-beverage`
5. `tech-small-appliance`

仅在“同等置信度命中多个类目”时使用此优先级。

## 4）同义词映射

### beauty-skincare

- skincare
- skin care
- beauty
- beauty product
- cosmetic
- cosmetics
- serum
- cream
- moisturizer
- sunscreen
- makeup
- lip care
- facial care
- 护肤
- 美妆
- 化妆品

### personal-care-cleaning

- personal care
- body care
- hygiene
- oral care
- toothpaste
- deodorant
- shower gel
- shampoo
- conditioner
- soap
- cleansing
- cleaning
- laundry
- disinfect
- 个护
- 清洁
- 洗护
- 口腔护理

### home-daily

- home
- household
- household item
- kitchenware
- storage
- organizer
- bedding
- home tool
- daily use
- 家居
- 日用
- 收纳
- 厨房用品

### food-beverage

- food
- snack
- snacks
- beverage
- drink
- tea
- coffee
- nutrition
- supplement
- edible
- 食品
- 饮料
- 零食
- 冲饮

### tech-small-appliance

- tech
- gadget
- electronics
- small appliance
- appliance
- usb device
- kitchen appliance
- home appliance
- earbud
- smart device
- digital
- 3c
- 小家电
- 数码
- 电子产品

## 5）冲突消解

- 如果 `category_raw` 同时出现美妆和清洁相关词：
  - 若出现护肤/彩妆词，优先选 `beauty-skincare`；
  - 否则选 `personal-care-cleaning`。
- 如果同时出现食品词和小家电词：
  - 若出现烹饪设备词（如 air fryer、blender、kettle），选 `tech-small-appliance`；
  - 若出现可食用单品词，选 `food-beverage`。
- 若置信度依旧不清晰，选 `generic`，并在结果中附带兜底说明。

## 6）输出字段

归一化后需要产出：

- `category_normalized`
- `strategy_file`

`strategy_file` 映射：

- `beauty-skincare` -> `references/strategy-beauty-skincare.md`
- `personal-care-cleaning` -> `references/strategy-personal-care-cleaning.md`
- `home-daily` -> `references/strategy-home-daily.md`
- `food-beverage` -> `references/strategy-food-beverage.md`
- `tech-small-appliance` -> `references/strategy-tech-small-appliance.md`
- `generic` -> `references/strategy-generic.md`
