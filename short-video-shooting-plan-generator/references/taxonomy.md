# 类目分类与归一化规则

## 1）标准类目

仅使用以下一级类目值：

- `beauty-skincare`
- `fashion-accessories`
- `jewelry-watches`
- `personal-care-cleaning`
- `home-daily`
- `home-appliance`
- `consumer-electronics`
- `sports-outdoor`
- `automotive-accessories`
- `office-school`
- `toys-games`
- `food-beverage`
- `health-wellness`
- `mom-baby`
- `pet`
- `holiday-party`
- `tools-hardware`
- `luggage-travel`
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
2. `fashion-accessories`
3. `jewelry-watches`
4. `mom-baby`
5. `personal-care-cleaning`
6. `home-appliance`
7. `consumer-electronics`
8. `sports-outdoor`
9. `home-daily`
10. `office-school`
11. `food-beverage`
12. `health-wellness`
13. `pet`
14. `toys-games`
15. `automotive-accessories`
16. `tools-hardware`
17. `luggage-travel`
18. `holiday-party`

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
- essence
- toner
- cream
- moisturizer
- sunscreen
- makeup
- lip care
- facial care
- cleanser
- foundation
- blush
- lipstick
- 护肤
- 美妆
- 化妆品
- 彩妆
- 精华
- 面霜
- 防晒

### fashion-accessories

- fashion
- apparel
- clothing
- clothes
- outfit
- womenswear
- menswear
- dress
- top
- shirt
- pants
- jeans
- skirt
- activewear
- hoodie
- sweater
- jacket
- coat
- shapewear
- underwear
- bra
- shoe
- shoes
- sneakers
- sandals
- boots
- bag
- handbag
- tote
- wallet
- sunglasses
- belt
- 服饰
- 女装
- 男装
- 穿搭
- 鞋靴
- 箱包
- 配饰

### jewelry-watches

- jewelry
- jewellery
- ring
- rings
- necklace
- necklaces
- earrings
- bracelet
- bracelets
- pendant
- anklet
- brooch
- watch
- watches
- quartz watch
- fashion watch
- accessory jewelry
- 珠宝
- 首饰
- 饰品
- 戒指
- 项链
- 耳环
- 手链
- 手表

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
- body wash
- hair care
- razor
- cleansing
- cleaning
- laundry
- disinfect
- tissue
- paper towel
- wipes
- 个护
- 清洁
- 洗护
- 口腔护理
- 家清
- 纸品

### home-daily

- home
- household
- household item
- houseware
- kitchenware
- storage
- organizer
- bedding
- home tool
- daily use
- decor
- cookware
- tableware
- pillow
- blanket
- towel
- hanger
- container
- 家居
- 日用
- 收纳
- 厨房用品
- 餐厨
- 床品

### home-appliance

- appliance
- appliances
- small appliance
- home appliance
- kitchen appliance
- air fryer
- blender
- mixer
- juicer
- coffee machine
- espresso machine
- kettle
- vacuum
- robot vacuum
- fan
- heater
- humidifier
- purifier
- steam cleaner
- garment steamer
- iron
- electric cooker
- 电器
- 家电
- 小家电
- 厨电
- 空气炸锅
- 料理机
- 吸尘器
- 净化器
- 电热水壶

### consumer-electronics

- tech
- gadget
- electronics
- electronic
- digital
- 3c
- phone accessory
- mobile accessory
- charger
- charging
- charging cable
- power bank
- usb device
- usb c
- earbud
- earbuds
- headphone
- headphones
- speaker
- microphone
- ring light
- tripod
- camera accessory
- keyboard
- mouse
- smartwatch
- smart watch
- tablet accessory
- gaming accessory
- 数码
- 电子产品
- 手机配件
- 充电宝
- 耳机
- 键盘
- 鼠标
- 麦克风

### sports-outdoor

- sport
- sports
- outdoor
- outdoor gear
- fitness
- workout
- gym
- yoga
- pilates
- running
- jogging
- cycling
- hiking
- camping
- trekking
- fishing
- yoga mat
- resistance band
- dumbbell
- sports bottle
- 运动
- 户外
- 健身
- 瑜伽
- 跑步
- 骑行
- 露营
- 徒步
- 钓鱼

### automotive-accessories

- automotive
- auto
- vehicle
- car accessory
- car accessories
- car organizer
- car charger
- car mount
- dashboard mount
- dash cam
- seat cushion
- steering wheel cover
- car sunshade
- floor mat
- seat gap filler
- car vacuum
- 车品
- 汽车用品
- 汽车配件
- 车载
- 车内
- 车充
- 车载支架
- 行车记录仪

### office-school

- office
- office supply
- office supplies
- school supply
- school supplies
- stationery
- notebook
- planner
- pen
- pencil case
- highlighter
- binder
- desk organizer
- study desk
- study lamp
- whiteboard
- 办公
- 学习
- 文具
- 学生用品
- 办公用品
- 桌面办公
- 笔记本

### toys-games

- toy
- toys
- game
- games
- board game
- puzzle
- building blocks
- blocks
- rc toy
- doll
- figurine
- educational toy
- sensory toy
- collectible toy
- 玩具
- 游戏
- 益智玩具
- 拼图
- 积木
- 桌游
- 遥控玩具

### food-beverage

- food
- snack
- snacks
- beverage
- drink
- tea
- coffee
- edible
- juice
- soda
- sparkling water
- sauce
- seasoning
- ready to eat
- 食品
- 饮料
- 零食
- 冲饮

### health-wellness

- supplement
- supplements
- vitamin
- vitamins
- probiotic
- probiotics
- fish oil
- omega 3
- collagen
- protein powder
- electrolyte
- wellness
- gummy vitamin
- nutrition support
- immunity support
- digestion support
- sleep support
- joint support
- 保健品
- 膳食补充剂
- 营养补充
- 维生素
- 益生菌
- 鱼油
- 胶原蛋白
- 蛋白粉
- 电解质

### mom-baby

- mom baby
- maternity
- baby
- infant
- newborn
- toddler
- kids
- baby feeding
- diaper
- baby bottle
- bottle
- pacifier
- stroller
- high chair
- breast pump
- baby carrier
- bib
- swaddle
- nursing
- maternal
- 母婴
- 婴儿
- 宝宝
- 新生儿
- 婴童
- 孕产
- 喂养
- 辅食

### pet

- pet
- pets
- dog
- cat
- puppy
- kitten
- pet food
- feeder
- litter
- litter box
- pet toy
- leash
- collar
- harness
- grooming
- scratching post
- pet bed
- 宠物
- 猫
- 狗
- 宠粮
- 猫砂
- 逗猫
- 狗绳

### holiday-party

- holiday
- seasonal
- christmas
- halloween
- valentine
- valentines
- easter
- thanksgiving
- party
- party supplies
- party decor
- balloons
- banner
- wrapping
- festive decor
- 节庆
- 节日
- 派对
- 派对用品
- 节日装饰
- 气球
- 圣诞
- 万圣节
- 情人节

### tools-hardware

- tool
- tools
- hardware
- diy
- repair tool
- home improvement
- drill
- electric drill
- screwdriver
- wrench
- pliers
- measuring tape
- utility knife
- socket set
- 工具
- 五金
- 维修工具
- 家装
- DIY
- 电钻
- 螺丝刀
- 扳手

### luggage-travel

- luggage
- suitcase
- carry on
- carry-on
- travel
- travel bag
- travel organizer
- packing cube
- packing cubes
- passport holder
- luggage tag
- toiletry bag
- neck pillow
- weekender
- 行李
- 行李箱
- 旅行
- 出行
- 旅行收纳
- 护照夹
- 登机箱
- 洗漱包

## 5）冲突消解

- 如果 `category_raw` 同时出现美妆和清洁相关词：
  - 若出现护肤/彩妆词，优先选 `beauty-skincare`；
  - 否则选 `personal-care-cleaning`。
- 如果同时出现服饰词和珠宝/手表词：
  - 若主体是戒指、项链、耳饰、手链、腕表，选 `jewelry-watches`；
  - 若主体是服装、鞋靴、箱包，选 `fashion-accessories`。
- 如果同时出现运动词和服饰词：
  - 若重点是器械、户外装备、运动场景，选 `sports-outdoor`；
  - 若重点是穿搭与版型，选 `fashion-accessories`。
- 如果同时出现家电词和数码词：
  - 若是围绕“完成家务/烹饪/空气/清洁任务”的电器，选 `home-appliance`；
  - 若是围绕“手机、桌面、音频、充电、拍摄、连接”的设备或配件，选 `consumer-electronics`。
- 如果同时出现汽车词和数码词：
  - 若产品主要安装或使用在车内，选 `automotive-accessories`；
  - 否则选 `consumer-electronics`。
- 如果同时出现办公词和玩具词：
  - 若重点是学习、书写、桌面效率，选 `office-school`；
  - 若重点是娱乐、拼搭、互动游戏，选 `toys-games`。
- 如果同时出现食品词和保健词：
  - 若重点是口味、零食、饮用体验，选 `food-beverage`；
  - 若重点是营养补充、日常保养、功能支持，选 `health-wellness`。
- 如果同时出现母婴词和玩具词：
  - 若主要服务婴儿/幼儿喂养、护理、出行、睡眠，选 `mom-baby`；
  - 若主要是玩耍、拼搭、互动，选 `toys-games`。
- 如果同时出现母婴词和个护词：
  - 若产品主要服务婴儿/儿童/孕产人群，选 `mom-baby`；
  - 否则选 `personal-care-cleaning`。
- 如果同时出现家居词和家电词：
  - 若需要通电、充电或电机驱动，优先选 `home-appliance`；
  - 否则选 `home-daily`。
- 如果同时出现家居词和工具词：
  - 若重点是修理、安装、施工，选 `tools-hardware`；
  - 否则选 `home-daily`。
- 如果同时出现箱包词和旅行词：
  - 若重点是旅行收纳、登机、出差、打包，选 `luggage-travel`；
  - 若重点是日常搭配和造型，选 `fashion-accessories`。
- 如果同时出现节庆词和其他产品词：
  - 若重点是节日氛围、派对布置、礼赠场景，选 `holiday-party`；
  - 若重点仍然是商品本身的常态使用价值，优先选对应商品类目，不因“礼盒”“送礼”字样强行改类。
- 如果同时出现食品词和家电词：
  - 若出现烹饪设备词（如 air fryer、blender、kettle），选 `home-appliance`；
  - 若出现可食用单品词，选 `food-beverage`。
- 若置信度依旧不清晰，选 `generic`，并在结果中附带兜底说明。

## 6）输出字段

归一化后需要产出：

- `category_normalized`
- `strategy_file`

`strategy_file` 映射：

- `beauty-skincare` -> `references/strategy-beauty-skincare.md`
- `fashion-accessories` -> `references/strategy-fashion-accessories.md`
- `jewelry-watches` -> `references/strategy-jewelry-watches.md`
- `personal-care-cleaning` -> `references/strategy-personal-care-cleaning.md`
- `home-daily` -> `references/strategy-home-daily.md`
- `home-appliance` -> `references/strategy-home-appliance.md`
- `consumer-electronics` -> `references/strategy-consumer-electronics.md`
- `sports-outdoor` -> `references/strategy-sports-outdoor.md`
- `automotive-accessories` -> `references/strategy-automotive-accessories.md`
- `office-school` -> `references/strategy-office-school.md`
- `toys-games` -> `references/strategy-toys-games.md`
- `food-beverage` -> `references/strategy-food-beverage.md`
- `health-wellness` -> `references/strategy-health-wellness.md`
- `mom-baby` -> `references/strategy-mom-baby.md`
- `pet` -> `references/strategy-pet.md`
- `holiday-party` -> `references/strategy-holiday-party.md`
- `tools-hardware` -> `references/strategy-tools-hardware.md`
- `luggage-travel` -> `references/strategy-luggage-travel.md`
- `generic` -> `references/strategy-generic.md`
