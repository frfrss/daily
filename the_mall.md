## 数据集
**user:**  

item|note|type
:-|:-:|:-:|
\_uuid||
nickname|昵称|text
sex|性别|select
birthday|生日|datetime
hobby|兴趣|class_uuid(foreignkey)
shoppingCart_uuid|购物车|foreignkey（shoopingCart）
purchasedGoods_uuid|已购商品|foreignkey(goods)
comment_uuid|评论|foreignkey


**class**

item|note|type
:-|:-:|:-:|
\_uuid||
class_uuid|上一级分类|foreignkey(class)
name|类别名称|text

**goods**

item|note|type
-|:-:|:-:
\_uuid||
class_uuid|所属类别|foreignkey（class）
name|商品名称|text
price|价格|number
sales_num|销售数量|number

**shoppingCart**

item|note|type 
-|:-:|:-:
\_uuid||
goods_uuid|商品|foreignkey(goods)
user_uuid|所属用户|foreignkey（user）

**commnet**

item|note|type
-|:-:|:-:|
\_uuid||
content|内容|text
user_uuid|发表用户|foreignkey（user）
date|发表日期|datetime
comment_uuid|下一级评论|foreignkey（comment）
goods_uuid|所属产品|foreignkey



