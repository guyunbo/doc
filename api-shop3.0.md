# Shop商家店铺API

基于微信小程序的商家店铺API

+ [3.0新增接口详情]
  + [门票类产品列表（修改）]
  + [门票类产品详情（修改）]
  + [门票预约日历]
  + [订单列表（修改）]
  + [门票产品订单详情]
  + [门票产品订单创建]



<a name="productList"></a>
### 门票类产品列表（修改）
#### GET  /product/list

###### 入参：
```javascript
      * page            Number  页码   // 默认page=1
      * conut           Number  数量   // 默认count=20
      * type            Number  2       //
        is_random       Bool    是否随机 //默认不填为false 不随机  
```

###### 出参：
```javascript
"result":
   {
      "list": [
       {
          "product_id"  : "产品id",
          "name"        : "产品名称",
          "cover"       : "封面图片url",
          "desc"        : "产品介绍",
          "price"       : "产品价格",
          "tag"         : [
                            '标签',
                            '标签',
          ],
          "bright_point"       : "一句话两点",
       },
       "total_count": 20, // 门票

     ]
   }
```

<a name="productDetail"></a>
### 券类产品详情（修改）
#### GET  /product/detail

###### 入参：
```javascript
   * id            String  产品ID   
   * type          String  产品类型   
```

###### 出参：
```javascript
"result":
    {
          "product_id"      : "产品ID"
          "name"            : "产品名称",
          "bright_point"    : "亮点介绍",
          "price"           : "产品价格",
          "original_price"  : "市场价",
          "tag"             : [
              "标签",
              "标签",
          ], //最多五个
          "desc"            : "产品详情", 
          "contain"         : "费用包含", 
          "usage"           : "产品须知", 
          "product_thumb"   : [
                                 "url", // 顶部轮播图
                                 "url",
           ],
          "product_vedio"  : {
                                 "vedio" : "vedio", // 顶部视频
                                 "cover" : "cover",
           },
           "product_type"  :  "";//产品类型
           "ticket"  : [
                             {
                                     'name':'门票名称',
                                     'ticket_price':'门票价格', 
                                     "original_price":"划线价格",
                                     'desc':'门票购买须知',
                                     "id"   : "门票id", 
                                     "status":"门票状态",
                             }, // 券列表,更多
                        ],
           "product_status"        : "产品状态", 
           "refund_rule"   : "退改规则", 
           "is_card"       : "是否可以使用礼品卡", 
           "is_refund"     : "是否可退款", 
           "is_invoice"    : "是否可开发票", 
           "is_coupons"    : "是否可用优惠券", 
   }
```

<a name="bookingCalendar"></a>
### 门票预约日历（新增）


#### GET  /product/calendar

###### 入参：
```javascript
      * id            Number  门票ID
      * type          Number  类型2
      * page          Number  页数
      * count         Number  获取数量
```

###### 出参：
```javascript
"result":
    {
          "current_time"    : "当前时间",
          "end_time"        : "截止时间",
          "validTime"       : [
            {
                   "date": "2018-06-01",
                   "stock": 12, // 库存个数
                   "status": 1, // 1: 可预订 2: 售尽,
            }  
          ]
       
   }
```


<a name="orderCreate"></a>
### 创建订单(修改)
#### POST  /order/create

######  入参：
```javascript
    * auth_access_token          String  授权token
    * id                         String  门票id
    * type                       Number  类型门票为2
    * contact_id                 String  联系人ID
    * count                      Number  购买数量
    * guest_info                 Array[{"name": "姓名", "id_card": "身份证/护照号", "birth": "出生日期"}]  // 入住信息
    * total_price                Number  总价
      coupon_id                  String  优惠券ID
      remark                     String  备注
          
```
###### 出参：
```javascript
"result":
   {
      "order_id": "订单ID"
   }
```

<a name="voucherOrderList"></a>
### 订单列表(修改)
#### GET  /order/list

###### 入参：
```javascript
        * auth_access_token         String  授权token
          status                    Number  订单状态  // 0 全部订单 2  待支付 3  支付成功 5  接单 8  交易完成 9  交易关闭  默认为0
          count                     Number  数量     // 默认count=20
          page                      Number  页码   // 默认page=1
```

###### 出参：
```javascript
"result":
   {
      "list": [
         {
           "order_id": "订单ID",
           "order_time": "2018-06-20", // 下单时间,精确到天
           "order_status": "订单状态", // 1: 待支付  2: 支付成功  3: 预约成功  4: 交易关闭 5: 交易完成
           "sub_status":"券状态",//根据规则，还未定,并且为了这个状态，需要改动多个预约相关接口
           "pay_total": "支付总价",
           "order_count": "订单件数",
           "cover": "产品封面",
           "id": "产品id",
           "product_name": "产品名称",
           "shop_id": "门店id",
           "shop_name": "门店名称",
           "name": "门票名称",
           "is_refundable": false , // 是否可退款
           "type"       : '1', //产品类型
           "sub_status" : '0', //子状态，用来判断订单状态是否加部分
           "count"      : "购买数量",
           "time"       : "出游时间",
           
           
        }
      ],
      "total_count": 28 // 订单个数
   }
```



<a name="orderDetail"></a>
### 订单详情
#### GET  /order/detail

######  入参：
```javascript
    * auth_access_token         String  授权token
    * order_id                  Number  订单id 
```
###### 出参：
```javascript
"result":
   {
      "order_id": "订单号",
      "order_time": "2018-06-20 10:30:30", // 下单时间,精确到秒
      "order_status": "订单状态id", // 1: 待支付  2: 支付成功  3: 预约成功  4: 交易关闭 5: 交易完成
      "ticket_name": "门票名称",
      "product_id": "产品ID",
      "shop_id": "门店id",
      "shop_name": "酒店或店铺名称",
      "ticket_id": "券ID",
      "product_cover": "产品封面",
      "product_name": "产品名称",
      "order_count": 1, // 订单件数
      "order_price": 61278, // 该订单预定期间的均价 
      "order_total":"商品总价",
      "pay_total":"支付总价",
      "coupon":"使用优惠券金额",  // 未使用优惠券显示0
      "ticket":[
          {
              "order_ticket_id":"111",//预定订单券id
              "status":"0",//订单门票状态
              "desc":"门票介绍",
              "remark":"券备注",
              "contact_list":[
                    {
                      "name": "入住人名字",
                      "iD_card": "身份证号",
                    },
                    ...
                    ],
             "check_in_time":"2019-01-01",
             "user_type":'1',//预约类型
             "price": 1500,//价格
          }
      ],
      "code":[
          "qrcode":'二维码url',
          "credential_code":"凭证码",
          "tracking"   :    "查询号",
      ]
      "contact":{
              "name": "联系人名字",
              "tel": "联系人手机号"
            },
      "order_remark":"订单备注",
      "refund": {
        "is_refundable": false, // 是否可退款
        "reason_map": [
          "id": 1,
          "reason": "退款原因选项"
        ],
        "status": "退款状态id", // 1: 退款中 2: 退款完成
      },
      "refund_rule"     : "退款规则", 
      "contain"         : "费用包含", 
      "usage"           : "使用要求", 
      "type"       : '5' //产品类型 
   }
```




