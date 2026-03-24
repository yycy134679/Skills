# 类目分类与归一化规则

## 1）标准类目

仅使用以下一级类目值：

| 标准值 | 中文类目 |
|---|---|
| `office-stationery` | 办公文具 |
| `pet-supplies` | 宠物用品 |
| `kitchenware` | 厨房用品 |
| `computers` | 电脑 |
| `home-textiles` | 家纺布艺 |
| `home-decor-festive` | 家居装饰 & 节庆装饰 |
| `home-appliances` | 家用电器 |
| `home-improvement-materials` | 家装建材 |
| `household-daily` | 居家日用 |
| `beauty-personal-care` | 美妆个护 |
| `mother-baby` | 母婴用品 |
| `automotive-motorcycle` | 汽车与摩托车 |
| `phones-digital` | 手机与数码 |
| `toys-hobbies` | 玩具与爱好 |
| `tools-hardware` | 五金工具 |
| `sports-outdoor` | 运动和户外 |
| `generic` | 通用兜底 |

## 2）归一化流程

1. 将 `category_raw` 转为小写。
2. 去除标点、`&` 两侧多余空格和重复空白。
3. 优先读取显式类目字段，不要对整段用户提示词做粗糙全量猜测。
4. 先按“完整词/短语边界”匹配同义词，再看歧义规则。
5. 如果命中多个候选，仅在同等置信度时使用优先级规则。
6. 若无法稳定命中，返回 `generic`。

不要做粗糙子串匹配。
示例：不要把 `case` 误识别成 `showcase`，也不要把 `pad` 误识别成任意平板配件。

## 3）优先级规则（高到低）

1. `beauty-personal-care`
2. `mother-baby`
3. `pet-supplies`
4. `home-appliances`
5. `phones-digital`
6. `computers`
7. `sports-outdoor`
8. `automotive-motorcycle`
9. `office-stationery`
10. `toys-hobbies`
11. `kitchenware`
12. `home-textiles`
13. `home-decor-festive`
14. `home-improvement-materials`
15. `tools-hardware`
16. `household-daily`

仅在“同等置信度命中多个类目”时使用该优先级。

## 4）同义词映射

### office-stationery

- office supplies
- office supply
- stationery
- school supplies
- notebook
- planner
- binder
- folder
- pen
- pencil
- highlighter
- desk organizer
- 文具
- 办公文具
- 办公用品
- 学习用品
- 笔记本
- 笔袋
- 便签
- 文件夹

### pet-supplies

- pet
- pet supplies
- cat supplies
- dog supplies
- pet accessory
- pet feeder
- pet toy
- litter box
- scratcher
- leash
- harness
- pet bed
- 宠物
- 宠物用品
- 猫用品
- 狗用品
- 宠物玩具
- 猫砂盆
- 饮水机
- 牵引绳

### kitchenware

- kitchenware
- cookware
- bakeware
- tableware
- cutlery
- utensil
- utensils
- food container
- lunch box
- dish rack
- chopping board
- frying pan
- saucepan
- 厨房用品
- 厨具
- 餐厨
- 餐具
- 炊具
- 锅具
- 烘焙工具
- 保鲜盒
- 砧板

### computers

- computer
- computers
- pc
- desktop
- laptop
- notebook computer
- monitor
- keyboard
- mouse
- printer
- docking station
- webcam
- hard drive
- computer accessory
- 电脑
- 笔记本电脑
- 台式机
- 显示器
- 键盘
- 鼠标
- 打印机
- 扩展坞

### home-textiles

- home textile
- bedding
- bed sheet
- fitted sheet
- duvet cover
- quilt cover
- pillowcase
- blanket
- throw
- curtain
- rug
- cushion cover
- 家纺
- 家纺布艺
- 床品
- 四件套
- 被套
- 枕套
- 毛毯
- 窗帘
- 地毯
- 靠垫套

### home-decor-festive

- home decor
- decoration
- decorative item
- ornament
- wall decor
- vase
- photo frame
- candle holder
- party decor
- balloon garland
- holiday decor
- seasonal decor
- birthday decor
- 家居装饰
- 装饰摆件
- 墙饰
- 节庆装饰
- 派对布置
- 节日装饰
- 气球布置
- 香薰摆件

### home-appliances

- appliance
- appliances
- home appliance
- kitchen appliance
- small appliance
- air fryer
- blender
- mixer
- juicer
- coffee machine
- kettle
- vacuum
- robot vacuum
- fan
- heater
- humidifier
- purifier
- 电器
- 家电
- 家用电器
- 小家电
- 厨电
- 空气炸锅
- 咖啡机
- 吸尘器

### home-improvement-materials

- building material
- renovation material
- home improvement
- wall panel
- wallpaper
- tile
- flooring
- faucet
- shower head
- sink accessory
- pipe fitting
- sealant
- adhesive
- cabinet handle
- door handle
- curtain track
- 建材
- 家装建材
- 墙板
- 墙纸
- 瓷砖
- 地板
- 水龙头
- 花洒
- 管件
- 密封胶
- 拉手

### household-daily

- household
- household goods
- daily use
- home organizer
- storage organizer
- hanger
- tissue
- paper towel
- trash bag
- laundry
- cleaning supplies
- bathroom accessory
- mop
- 居家日用
- 日用品
- 家居日用
- 家清
- 清洁用品
- 收纳用品
- 衣架
- 纸巾
- 垃圾袋

### beauty-personal-care

- beauty
- skincare
- makeup
- cosmetic
- cosmetics
- serum
- cleanser
- moisturizer
- sunscreen
- lipstick
- body care
- hair care
- shampoo
- conditioner
- oral care
- deodorant
- perfume
- 美妆
- 个护
- 美妆个护
- 护肤
- 彩妆
- 洗护
- 口腔护理
- 身体护理
- 洗发护发

### mother-baby

- mother and baby
- maternity
- baby
- infant
- toddler
- nursing
- feeding
- stroller
- bottle
- breast pump
- diaper
- baby care
- 母婴
- 母婴用品
- 婴儿用品
- 宝宝用品
- 喂养
- 哺乳
- 奶瓶
- 纸尿裤
- 推车
- 辅食

### automotive-motorcycle

- automotive
- auto accessory
- car accessory
- car care
- vehicle accessory
- motorcycle
- motorbike
- motorcycle accessory
- helmet
- dash cam
- car mount
- 汽车用品
- 汽车配件
- 车载
- 汽车与摩托车
- 摩托车
- 摩托车配件
- 头盔
- 行车记录仪
- 车载支架

### phones-digital

- phone accessory
- mobile accessory
- phone case
- charger
- cable
- power bank
- earbuds
- speaker
- tablet accessory
- camera accessory
- tripod
- gimbal
- microphone
- smartwatch
- 手机与数码
- 手机配件
- 数码
- 充电器
- 充电宝
- 耳机
- 音箱
- 摄影配件
- 云台
- 麦克风

### toys-hobbies

- toy
- toys
- hobby
- hobby kit
- collectible
- model kit
- puzzle
- board game
- building blocks
- rc car
- action figure
- blind box
- diy craft kit
- 玩具
- 爱好
- 玩具与爱好
- 潮玩
- 模型
- 积木
- 拼图
- 桌游
- 盲盒
- 手作套装

### tools-hardware

- tools
- tool set
- toolbox
- power tool
- screwdriver
- wrench
- drill
- plier
- hammer
- measuring tape
- screw kit
- 五金工具
- 工具
- 工具箱
- 电钻
- 螺丝刀
- 扳手
- 钳子
- 锤子
- 卷尺
- 螺丝套装

### sports-outdoor

- sports
- outdoor
- fitness
- gym gear
- yoga
- running
- training gear
- camping
- hiking
- trekking
- fishing
- cycling
- 运动和户外
- 运动户外
- 健身
- 瑜伽
- 跑步
- 露营
- 徒步
- 户外装备
- 训练器材

## 5）宠物用品二级差异层

仅当 `category_normalized=pet-supplies` 时，才继续判断二级差异层。

判断顺序：

1. 若用户显式给出二级类目字段或截图中的标准二级类目，优先使用。
2. 若 `category_raw` 已经足够具体，可直接用来命中宠物二级。
3. 若只有商品名和自然语言描述，则只在高置信时推断。
4. 若命中多个二级或置信度不足，返回空值，不强行猜测。

返回字段：

- `subcategory_normalized`
- `subcategory_label`
- `subcategory_file`

### livestock-poultry / 家畜及家禽用品

- livestock
- poultry
- farm animal
- chicken coop
- hen feeder
- livestock care
- 家畜
- 家禽
- 鸡鸭鹅
- 养殖用品
- 畜牧用品

### cat-dog-health / 猫狗保健

- pet health
- dog health
- cat health
- supplement
- probiotic
- vitamin
- dewormer
- calming aid
- dental care
- joint support
- 猫狗保健
- 宠物保健
- 营养膏
- 宠物维生素
- 益生菌
- 驱虫
- 洁牙
- 关节养护

### cat-dog-apparel / 猫狗宠物装

- pet apparel
- pet clothes
- dog clothes
- cat clothes
- pet costume
- pet sweater
- raincoat
- bandana
- 宠物装
- 宠物衣服
- 宠物服饰
- 狗狗衣服
- 猫咪衣服
- 宠物毛衣
- 宠物雨衣

### cat-dog-furniture / 猫狗家具

- pet furniture
- pet bed
- cat tree
- scratch post
- scratching tree
- pet house
- pet sofa
- pet stairs
- 猫狗家具
- 宠物家具
- 猫爬架
- 宠物窝
- 宠物床
- 宠物楼梯
- 宠物沙发

### cat-dog-grooming / 猫狗美容用具

- grooming
- pet grooming
- brush
- slicker brush
- nail clipper
- clipper
- grooming kit
- pet shampoo
- deshedding
- 猫狗美容
- 宠物美容
- 宠物梳子
- 指甲剪
- 剃毛器
- 洗护工具
- 开结梳

### cat-dog-litter / 猫狗如厕用品

- litter
- cat litter
- litter box
- pee pad
- training pad
- poop bag
- poop scoop
- deodorizer
- 猫狗如厕
- 宠物如厕
- 猫砂
- 猫砂盆
- 尿垫
- 拾便袋
- 除臭
- 铲屎

### cat-dog-accessories / 猫狗用品配件

- pet accessory
- collar
- leash
- harness
- bowl
- feeder
- water fountain
- tag
- carrier
- travel bag
- toy
- chewer
- 猫狗配件
- 宠物配件
- 项圈
- 牵引绳
- 胸背
- 食盆
- 喂食器
- 饮水机
- 宠物包
- 宠物玩具

### birds / 鸟类用品

- bird
- birds
- bird cage
- perch
- bird feeder
- parrot toy
- 鸟类用品
- 鸟用品
- 鸟笼
- 站杆
- 鹦鹉玩具

### reptiles-amphibians / 爬行动物及两栖动物用品

- reptile
- amphibian
- terrarium
- heat lamp
- reptile tank
- basking
- 爬宠
- 爬行动物
- 两栖动物
- 守宫
- 蜥蜴
- 龟
- 蛙
- 饲养箱
- 加热灯

### small-pets / 小宠用品

- small pet
- hamster
- rabbit
- guinea pig
- chinchilla
- ferret
- 小宠
- 仓鼠
- 兔子
- 豚鼠
- 龙猫
- 雪貂

### aquatics / 鱼类及水产用品

- fish
- aquarium
- aquatic
- fish tank
- filter
- air pump
- water pump
- pond
- 鱼类用品
- 水产用品
- 水族
- 鱼缸
- 过滤器
- 氧气泵
- 水泵

宠物二级的补充边界规则：

- 若主体是猫狗常规用品，且没有命中保健、宠物装、家具、美容、如厕中的任一高置信方向，默认归 `cat-dog-accessories`。
- 若主体是猫爬架、宠物窝、宠物床、抓板、宠物楼梯，优先归 `cat-dog-furniture`，不要落到 `cat-dog-accessories`。
- 若主体是猫砂、尿垫、拾便袋、猫砂盆、除臭清洁类如厕产品，优先归 `cat-dog-litter`。
- 若主体是营养补充、洁牙、驱虫、情绪舒缓、关节养护等，优先归 `cat-dog-health`。
- 若主体明确服务鸟类、鱼类水族、爬宠或小宠，不要再落到猫狗二级。

## 6）边界/歧义规则

- `beauty-personal-care` vs `household-daily`
  - 护肤、彩妆、洗护、身体护理、口腔护理归 `beauty-personal-care`。
  - 家居清洁、纸品、垃圾袋、洗衣与收纳清洁耗材归 `household-daily`。
- `phones-digital` vs `computers`
  - 手机、平板、音频、充电、拍摄、便携数码配件归 `phones-digital`。
  - 笔记本、台式机、显示器、键鼠、打印与桌面计算场景归 `computers`。
- `home-appliances` vs `kitchenware`
  - 通电、充电、电机驱动或核心价值来自机器工作流程的，归 `home-appliances`。
  - 非电动锅具、餐具、刀具、备餐与收纳器皿归 `kitchenware`。
- `tools-hardware` vs `home-improvement-materials`
  - 重点是拧、钻、量、装、切等“执行任务的工具”时归 `tools-hardware`。
  - 重点是墙面、地面、水路、五金件、装饰面材、安装辅材时归 `home-improvement-materials`。
- `home-decor-festive` vs `home-textiles` vs `household-daily`
  - 装饰、氛围、摆件、节庆布置归 `home-decor-festive`。
  - 床品、窗帘、毯类、布艺软装归 `home-textiles`。
  - 收纳、清洁、纸品、通用日用品归 `household-daily`。
- `toys-hobbies` vs `mother-baby`
  - 核心价值是玩、拼、收集、互动、兴趣养成，归 `toys-hobbies`。
  - 核心价值是喂养、护理、出行、安抚、阶段适配，归 `mother-baby`。
- `automotive-motorcycle` vs `phones-digital`
  - 产品主要安装、固定、使用在车内或摩托车场景中，归 `automotive-motorcycle`。
  - 若只是普通手机数码配件且不绑定车/摩托场景，归 `phones-digital`。

## 7）未覆盖旧类目处理

以下旧域不再作为本轮标准一级类目维护：

- 服饰鞋包
- 珠宝手表
- 食品饮料
- 保健滋补
- 箱包出行

若用户显式给出这些类目，默认走 `generic`，除非商品描述同时明确命中当前 16 类之一。

## 8）策略文件映射

- `office-stationery` -> `references/strategy-office-stationery.md`
- `pet-supplies` -> `references/strategy-pet-supplies.md`
- `kitchenware` -> `references/strategy-kitchenware.md`
- `computers` -> `references/strategy-computers.md`
- `home-textiles` -> `references/strategy-home-textiles.md`
- `home-decor-festive` -> `references/strategy-home-decor-festive.md`
- `home-appliances` -> `references/strategy-home-appliances.md`
- `home-improvement-materials` -> `references/strategy-home-improvement-materials.md`
- `household-daily` -> `references/strategy-household-daily.md`
- `beauty-personal-care` -> `references/strategy-beauty-personal-care.md`
- `mother-baby` -> `references/strategy-mother-baby.md`
- `automotive-motorcycle` -> `references/strategy-automotive-motorcycle.md`
- `phones-digital` -> `references/strategy-phones-digital.md`
- `toys-hobbies` -> `references/strategy-toys-hobbies.md`
- `tools-hardware` -> `references/strategy-tools-hardware.md`
- `sports-outdoor` -> `references/strategy-sports-outdoor.md`
- `generic` -> `references/strategy-generic.md`

### pet-supplies 二级差异文件映射

- `livestock-poultry` (`家畜及家禽用品`) -> `references/pet-supplies/livestock-poultry.md`
- `cat-dog-health` (`猫狗保健`) -> `references/pet-supplies/cat-dog-health.md`
- `cat-dog-apparel` (`猫狗宠物装`) -> `references/pet-supplies/cat-dog-apparel.md`
- `cat-dog-furniture` (`猫狗家具`) -> `references/pet-supplies/cat-dog-furniture.md`
- `cat-dog-grooming` (`猫狗美容用具`) -> `references/pet-supplies/cat-dog-grooming.md`
- `cat-dog-litter` (`猫狗如厕用品`) -> `references/pet-supplies/cat-dog-litter.md`
- `cat-dog-accessories` (`猫狗用品配件`) -> `references/pet-supplies/cat-dog-accessories.md`
- `birds` (`鸟类用品`) -> `references/pet-supplies/birds.md`
- `reptiles-amphibians` (`爬行动物及两栖动物用品`) -> `references/pet-supplies/reptiles-amphibians.md`
- `small-pets` (`小宠用品`) -> `references/pet-supplies/small-pets.md`
- `aquatics` (`鱼类及水产用品`) -> `references/pet-supplies/aquatics.md`
