# HamsterEcoHelper 插件指南
HamsterEcoHelper (仓鼠症插件) 是为促进服务器经济流动的辅助性插件。
此插件的设计核心是用于掌控整个服务器的资金流，并根据服务器内经济状况做动态微调、创造供需、活跃市场。

!> 最重要的是，该插件能够极大的缓解甚至可以基本解决众多 Minecraft 服务器在发展到后期时候遇到的通病——经济膨胀。

!> 注意：如果购买/拍到物品时背包已满，物品将会保存至末影箱中，末影箱已满将会保存至临时储存，需要使用 `/heh retrieve` 获取物品。

## 插件介绍
- 全球市场
全球市场用于快速上架、销售物品。新物品上架自带全服广播、可配置上架费用和每 24 小时物品保管费用、自定义税率、不同玩家组在全球市场的物品槽位限制等。

- 牌子商店
玩家设置的木牌商店，包含销售、收购、抽奖功能。每位玩家的同一种木牌内容自动同步，再也无须手动管理维护一大堆商店木牌。例如一名玩家在四个城市创建了销售木牌，这名玩家只需要上架一次物品，其他玩家均可在这四个木牌中购得。插件可配置木牌商店的交易税率、每位玩家的牌子和物品槽位限制等。

- 玩家拍卖/征购
玩家可发起拍卖/征购来快速卖出、购入物品。插件可自定义拍卖物品的税率。

- 系统拍卖/征购

- 税务
插件中的各种交易均可设置特定的税率，玩家支付的税将会通过系统征购或其他管理命令返回到玩家手中。

- 广告
在服务器中发布广告。即便玩家不在线，也会自动轮播广告，按展示量收取费用。酒香也怕巷子深，使用广告吸引其他玩家来抢购物品吧。

- 死亡惩罚
带有最低额度、最高额度和按百分比扣除账户金钱的死亡惩罚功能。

- 系统余额及 API/管理命令
系统余额是插件的核心功能。所有玩家支付的税务、广告费、死亡惩罚、系统拍卖所得资金甚至 NyaaUtils 插件中产生的消费等，都会存储到系统余额。这些资金会通过系统征购物品返还到玩家手中，或者通过自带的支付/扣除命令使资金在系统与玩家之间流通。

!> 不仅这些功能，仓鼠经济插件通过各种配置选项实现防滥用、防刷（例如向固定系统收购倾销，短时间获取大量金钱）特性。服务器中的金钱印发后便不会消失而是在玩家之间流通，方便掌控服务器内经济发展状况。

## 指令列表

以下指令分为**指令全名**和**短命令**。前者在使用的时候需要在前面加上 `/heh`，后者则不需要。

| 指令全名 | 短命令 | 参数 | 含义 |
| :-: | :-: | :-: | :-: |
| `requisition` | `/hreq` | `<物品ID \|hand> <单价> <数量>` | 发起征购 |
| `requisition sell` | `/hsell` | `[数量]` | 响应征购 |
| `auction auc` | `/hauc` | `<起步价> <步进价> [保留价]` | 发起拍卖，拍卖手中的（所有）物品 |
| `auction bid` | `/hbid` | `[价格]` | 拍卖竞价 |
| `shop sell` | - | `<单价>` | 将手中的（所有）物品上架至木牌商店销售 |
| `shop buy` | - | `<单价>` | 将手中的（所有）物品上架至木牌商店收购 |
| `shop storage set` | - | 无 | 指定一个箱子作为收购物品的存储 |
| `shop storage info` | - | 无 | 查找自己的存储箱 |
| `shop lotto set` | - | 无 | 指定一个箱子作为装有抽奖物品的箱子 |
| `shhop lotto info` | - | 无 | 查找自己装有抽奖物品的箱子 |
| `search` | - | 见下 | 搜索木牌商店的物品 | 
| `searchpage` | - | `<页数>` | 翻页 |
| `transaction sellto` | `/hsellto` | `<玩家名> <单价>` | 手持所售物品创建点对点交易 |
| `transaction pay` | `/hpay` | `<账单 ID>` | 支付账单 |
| `transaction cancel` | - | `<账单 ID>` | 取消账单 |
| `ads add` | - | `<广告内容> <展示次数>` | 发布广告 |

## 如何使用？

### 征购

#### 玩家发起征购
命令 `/heh requisition req <物品 ID | hand> <单价> <数量>` 可发起征购。
如果你手里已经有希望征购的物品，可以用 `hand` 代替物品ID。

可用短命令 `/hreq <物品 ID | hand> <单价> <数量>` 替代。

?> **示例**<br>
以单价 1 coin 征购 100 个白色羊毛，可以用 `/hreq wool 1 100`。

!> 你拥有的 coins 余额必须至少为 **征购款项总额**；你的背包空间须足以存储这些物品。发起征购后会将这些 coins 暂存在系统，物品会直接存入你的背包。
征购结束后，如果还有剩余的 coins 会退回你的账户。

#### 玩家响应征购
出现 `[征购]` 提示时，可以使用 `/heh requisition sell [数量]` 将手中指定数量的符合要求的物品卖出，响应征购。
若“数量”不填（即`/heh requisition sell`），则视为全部卖出，直至征购量已满为止。

也可以短命令 `/hsell [数量]` 代替。

### 拍卖

#### 玩家发起拍卖
手中拿着要竞拍的物品（可堆叠），执行 `/heh auction auc <起步价> <步进价> [保留价]`。

用短命令 `/hauc <起步价> <步进价> [保留价]`，可快速发起拍卖。

如果成功卖出，买家获得该物品并支付相应的coins；而卖家可以获得完整的出价 coins。
如因未能达到保留价，或者无人竞拍而流拍，物品会退回你的背包。

?> **示例**<br>
要拍卖钻石 2 颗，则在手中拿着 2 颗；希望以总价 100 coins 起步，每次出价至少比上次高 10 coins，输入 `/hauc 100 10` 即可。
如果还希望成交价最低 150 coins，则使用 `/hauc 100 10 150`。

#### 玩家竞价
出现`[拍卖]`提示时可以使用 `/heh auction bid [价格]` 来出价。
若“价格”不填，或填写`min`（即`/heh auction bid`），可跟进当前允许的最低出价。

短命令 `/hbid [价格]`可快速出价。

### ~~世界商店~~
插件冲突，已经停用。

### 木牌商店
HamsterEcoHelper 的木牌商店结合了线下商店的优势（较低的税率、无放置保管上架费用等），多种功能（销售、收购、抽奖）和网络技术（同类型木牌内容同步），开设和维护商店都变得更加简单。每一位玩家的同类型木牌将拥有一致的内容。不需要费劲维护大一堆的木牌，只需要编辑一个商店，其他同类型的商店都会同步更新。

#### 规则与限制
- 玩家可以创建 50 个木牌；
- 玩家在自己的销售区可以使用 54 个物品槽（slot）。
- Lotto 木牌的物品不占用槽的空间，但是每个 Lotto 木牌会占用一个木牌的限制。
- Lotto 木牌必须在说明部分或附近牌子注明抽奖物品和中奖率。不合格的木牌或虚标中奖率的木牌将被强制退回交易。
- 合法的 Lotto 木牌交易结果自负，交易前请三思。
- 交易消费税 2%。

#### 开始销售
首先创建一个牌子。格式：

- 第一行 `[shop]`
- 第二行 `SELL`
- 第三行 任意文本
- 第四行 任意文本

然后准星对着牌子，手里拿着要上架的物品，敲 `/heh shop sell [单价]`。手中物品将全部上架。

?> **示例**<br>
手中拿着 64 个石头，每个石头 9 coins 的价格销售，则 `/heh shop sell 9`。

#### 开始收购
先创建一个牌子。

- 第一行 `[shop]`
- 第二行 `BUY`
- 第三行 任意文本
- 第四行 任意文本

然后要选择一个箱子作为收购物品的存储。命令 `/heh shop storage set` 然后右键点一个箱子。
接下来手中拿着要收购的物品，准星对着收购木牌，敲 `/heh shop buy [单价]`。例如 9 coins 每个收购石头，则手中拿着石头然后敲 `/heh shop buy 9`

如果忘记自己的存储箱在哪里，命令 `/heh shop storage info`。

!> 注意，选择箱子只需选择一次，后续设置其他收购就可以跳过这一步，以后所有的收购都会存放到该收购箱子里，不可以设置每个收购单独一个储存箱子。

#### 购买商品与响应收购
右键点击**“销售”（SELL）**牌子即可打开商店界面。单击购买一个，按住 <kbd>Shift</kbd> 单击则购买一整个槽。

右键点击**“收购”（BUY）**牌子可查询该木牌所收购的物品明细。
手持相应物品，对准该木牌**左键**点击，即可出售给收购方。按住 <kbd>Shift</kbd> **左键**则出售一整个槽。

每次交易均加征 2% 消费税。

#### 抽奖
创建牌子先。
- 第一行 `[shop]`
- 第二行 `LOTTO`
- 第三行 任意文本
- 第四行 `每次使用价格`

然后需要选择一个装有抽奖物品的箱子 `/heh shop lotto set` 然后右键点箱子。

如果忘记了箱子的位置，用 `/heh shop lotto info`。

玩家右键点击 `Lotto` 木牌即会支付一次交易的费用随机获得箱子中的一个槽的物品。

!> 抽奖设置的箱子和收购设置的箱子不一样哦！

### 搜索
现已支持在世界范围内搜索木牌商店的物品。
#### 用法
`/heh search 关键词 [选项:值]`
#### 可用选项
- `i` 或 `item`：物品名称或 ID，仅搜索指定物品
- `p` 或 `player`：玩家 ID，仅搜索指定玩家
- `r` 或 `range`：搜索范围，仅搜索指定距离内有木牌的商店
- `a` 或 `advanced`：高级搜索选项：
    - `ench`：搜索包括附魔
    - `enchonly`：仅搜索附魔
    - `lore`：搜索包括描述
    - `loreonly`：仅包括描述
- 选项间用 `|` 并列，如 `ench|lore`

若搜索结果多于 1 页，可使用 `/heh searchpage page` 命令翻页。

### 点对点交易
点对点交易是全新的交易模式。交易只有买卖双方参与，内容不会泄露给第三者，双方利益与隐私被全面保护。双方在任意时刻、地点皆可“面交”，大大提升便利性。
#### 出售物品并发送账单
手持所售物品，使用命令`/heh transaction sellto <玩家 ID> <单价>`，则将物品**整个槽全部**出售。
或者，也可用快捷命令`/hsellto <玩家 ID> <单价>`。

使用后，物品立即暂存于系统，并向目标玩家发送账单。

#### 支付或取消账单
待目标玩家在线时，如有新账单会立即在聊天栏提示，并附带唯一的“账单 ID”。
收到账单后3分钟内，按提示使用命令`/heh transaction pay <账单 ID>`即可**支付并收货**。短命令`/hpay <账单 ID>`亦可。
每次支付加征 1% 消费税。

使用命令`/heh transaction cancel <账单 ID>`即可取消账单。 需要交易双方都同时确认订单才会完成交易。
