# Category Taxonomy and Normalization

## 1) Standard Categories

Use the following canonical category values only:

- `beauty-skincare`
- `personal-care-cleaning`
- `home-daily`
- `food-beverage`
- `tech-small-appliance`
- `generic`

## 2) Normalization Pipeline

1. Lowercase `category_raw`.
2. Remove punctuation and extra spaces.
3. Match aliases by whole word / phrase boundary first.
4. If multiple candidates match, pick the first high-confidence hit by priority.
5. If no candidate matches, return `generic`.

Do not do naive substring matching inside another word.  
Example: do not match `tea` from `teaser`.

If the user message contains an explicit category field (for example `Category: ...`), normalize only that extracted value rather than the whole prompt.

## 3) Priority Rule (High to Low)

1. `beauty-skincare`
2. `personal-care-cleaning`
3. `home-daily`
4. `food-beverage`
5. `tech-small-appliance`

Use this order only when multiple categories are triggered at the same confidence.

## 4) Synonym Mapping

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

## 5) Conflict Resolution

- If `category_raw` contains both beauty and cleaning words:
  - If skincare or makeup tokens appear, choose `beauty-skincare`.
  - Else choose `personal-care-cleaning`.
- If food terms and appliance terms both appear:
  - If terms include cooking equipment (air fryer, blender, kettle), choose `tech-small-appliance`.
  - If terms include edible item names, choose `food-beverage`.
- If confidence is still unclear, choose `generic` and add fallback note.

## 6) Output Fields

After normalization, produce:

- `category_normalized`
- `strategy_file`

`strategy_file` map:

- `beauty-skincare` -> `references/strategy-beauty-skincare.md`
- `personal-care-cleaning` -> `references/strategy-personal-care-cleaning.md`
- `home-daily` -> `references/strategy-home-daily.md`
- `food-beverage` -> `references/strategy-food-beverage.md`
- `tech-small-appliance` -> `references/strategy-tech-small-appliance.md`
- `generic` -> `references/strategy-generic.md`
