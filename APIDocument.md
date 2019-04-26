# **对外开放接口说明文档**

## 微兔软件（香港）有限公司


- [**对外开放接口说明**](#0)
  - [接口说明](#-01)
  - [token签名说明](#-02)
- [**授权接口**](#1)
  - [获取Domain接口](#-11)
  - [登陆接口](#-12)
  - [根据Token获取用户信息接口](#-13)
  - [刷新Token接口](#-14)
  

- [**门店查询接口**](#2)
  - [门店列表查询](#-21)
- [**仓位查询接口**](#3)
  - [仓位列表查询](#-31)
  - [仓位详细查询](#-32)
- [**接口异常**](#100)
  - [异常说明](#-101)

<!-- TOC END -->

<a name="-01"><h2>接口说明</h2></a>


### 1、请求体构成


> ### 1.URL:https://{Domain}/api/{Language}/{Path} 

>>- _Domain:主域默认是w2smapit.wayto.store，不同的公司由平台派发_
>>- _Language:当前的语言（目前仅支持中文cn、英文en）_  
>>- _Path: API路径_
  
>2.参数组成(如果是*Get*请将以下参数追加到URL后，如果是*POST*请将以下参数追加在body里)
>>- _fromFlag来源微信端:10,IOS端:20,安卓端:30,其他市场: 40_
>>- _token接口签名_


### 2、响应体构成

<font color=#ff0000 size=2 face="黑体">请求统一返回JSON数据</font>

> 响应主体结构
> ``{
"info":[{"status":{Status},"message":{Message}}],
"data":{Result},
"tokenStatus": {TokenStatus},
"totalPage": {Pages}
}``
>>- _Status为1表示成功，0表示异常_  
>>- _Message提示信息_  
>>- _Result返回结果数据为JSON_
>>- _TokenStatus: 1正常，2失效，3过期_  

<a name="-02"><h2>token签名说明</h2></a>


### token签名是保证接口数据安全的机制，由登录或平台定制生成，失效后请刷新token，采用单点登录方式如被其他人登录此账号token将失效。

<a name="-11"><h2>获取Domain接口</h2></a> 

### _[GET]Path:app/getDomainByCode_  

 字段 |  类型 | 必填 |   说明
------ |--------|----| ----------------------------------------------------
  code   |  String | Y | 授权码
 

###  返回结果
 
```
{
    "info": {
        "status": 1,
        "message": ""
    },
    "data": {
        "domain": "https://w2smapit.wayto.store/",
        "daysLeft": 505,
        "describe": "",
        "companyName": "LockerInC",
        "companyLogo": null
    }
}

```
 字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  domain   |  String | 返回当前企业域名
  daysLeft   | Int  | 该企业有效期剩余天数
  describe  |  String   | 描述信息
  companyName | String  | 企业名称
  companyLogo | String  | 企业Logo
  
 
<a name="-12"><h2> 登陆接口</h2></a> 
### _[POST]Path:users/login_  

  字段 |  类型 | 必填 |   说明
------ |--------|----| ----------------------------------------------------
 account   |  String | Y | 账号
 password   |  String | Y | 密码
 deviceToken   |  String | N | 设备信息，用户app端推送
 isMD5   |  Int | Y | 是否MD5加密，1需要加密，0不用加密
 
### 返回结果
```
{
    "info": {
        "status": 1,
        "message": ""
    },
    "data": {
        "id": "2019032910064731227786fa0bdc966",
        "name": "Demolb",
        "headPortrait": null,
        "gender": 0,
        "remarks": "",
        "token": "1EF35F939491E9FDC54A1168FCFA0675DA8D545751BD61F5BC16E464139C9087052343EA073943DC9A59A92A80A78B04F40489FC4C11A9E8",
        "couponsAbleCount": 2
    }
}
```
 字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  id   |  String | 用户id
  token | String | 当前登录token
  name   | String  | 用户名
  headPortrait  |  String   | 用户头像
  gender | Int  | 性别：1男，2女，0未知
  couponsAbleCount| Int | 可使用优惠券张数
  remarks | String  | 备注
 
 <a name="-13"><h2>根据Token获取用户信息接口</h2></a> 
 
 ### _[POST]Path:user/getInfoByToken_  
 
  字段 |  类型 | 必填 |   说明
------ |--------|----| ----------------------------------------------------
  token   |  String | Y | token
  ### 返回结果
  
```
{
    "info": {
        "status": 1,
        "message": ""
    },
    "data": {
        "id": "2019032910064731227786fa0bdc966",
        "name": "Demolb",
        "headPortrait": null,
        "gender": 0,
        "remarks": "",
        "token": "1EF35F939491E9FDC54A1168FCFA0675DA8D545751BD61F5BC16E464139C9087052343EA073943DC9A59A92A80A78B04F40489FC4C11A9E8",
        "couponsAbleCount": 2
    }
}
```
 字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  id   |  String | 用户id
  token | String | 当前登录token
  name   | String  | 用户名
  headPortrait  |  String   | 用户头像
  gender | Int  | 性别：1男，2女，0未知
  couponsAbleCount| Int | 可使用优惠券张数
  remarks | String  | 备注
  
  
  <a name="-14"><h2>刷新Token接口</h2></a>  
  ### _[POST]Path:user/refreshTokenByToken_  
  
 字段 |  类型 | 必填 |   说明
------ |--------|----| ----------------------------------------------------
  code   |  String | Y | 原Token
  
  
 ### 返回结果
  
  ```
  {
    "info": {
        "status": 1,
        "message": ""
    },
    "tokenStatus": 1,
    "data": {
        "token": "1EF35F939491E9FDC54A1168FCFA0675DA8D545751BD61F5BC16E464139C9087052343EA073943DC157A98EF920E4193A4D3A2DF486E5E3B"
    }
}
  ```
  字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  code   |  String | 新token

<a name="-21"><h2>门店列表查询</h2></a>  


  

### _[GET]Path:store/list_  


### 返回结果


```
{  
  "info": {  
    "status": 1,
    "message": ""
  },
  "data": [
    {
      "id": "2019041914281048143830fe44b7aca",
      "name": "Wine Banc Delta House",
      "no": "No.0001",
      "district": "Central Area",
      "adress": "2 Alexandra Road Delta House 03-01E S(159919)",
      "warehouseCount": 5,
      "longitude": "",
      "latitude": "",  
      "distant": 0,  
      "amountRented": 1,  
      "amountUnleased": 4,  
      "amountOutOfStock": 0,  
      "amountValidReserved": 2
    },
    {
      "id": "2019041715013884546021686739685",
      "name": "WinebancDelta",
      "no": "No.0013",
      "district": "Alexandra Road",
      "adress": "NO 2 ALEXANDRA ROAD DELTA HOUSE",
      "warehouseCount": 100,
      "longitude": "",
      "latitude": "",
      "distant": 0,
      "amountRented": 3,
      "amountUnleased": 97,
      "amountOutOfStock": 0,
      "amountValidReserved": 0
    }
  ]
}
```

  字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  id   |  String | id
  name   | String  | 名称
  no  |  String   | 编号
  district | String  | 地区
  adress | String  | 地址
longitude | String | 经度
latitude | String | 纬度
distant | Double | 距离
warehouseCount| Int | 仓数
amountRented | Int | 已租仓总数
amountUnleased | Int | 未租仓数总数
amountOutOfStock| Int| 下架仓总数
amountValidReserved| Int| 预留仓总数

<a name="-31"><h2>仓位列表查询</h2></a>  


### _[GET]Path:_warehouse/list_


### 请求参数  


  字段 |  类型  | 必填 | 默认值 |  说明
------ |--------| ------|-----| ----------------------------------------------------
  page   |  Int |  Y  |  |请求页数,1开始
  rows   | Int  | Y |  |每页显示条数
  sort  |  String | N | updatetime | 更新时间:updatetime,编号:id,仓位号:no,仓型:type,面积:area,价格:price,体积:volume,天数:days
  order | String | N  | desc |排序规则: asc降序, desc降序
  type | String  | N | |仓型
id | String  | N | |仓id
no | String  | N | |仓号
minArea | String  | N | |最小面积
maxArea | String  | N | |最大面积
minPrice | String  | N | |最低价格
maxPrice | String  | N | |最高价格

### 返回结果

```
{
  "info": {
    "status": 1,
    "message": ""
  },
  "pages": 1,
  "data": [
    {
      "accrualBasis": 0,
      "id": "201903021911078170873c25c323214",
      "name": "大唐西里",
      "storeId": "20190302190653556334420a60fd6fc",
      "typeName": "L",
      "typeId": "20181022110914965380570166b7ae3",
      "no": "XL0069",
      "distant": -1,
      "discounts": 0,
      "longitude": "",
      "latitude": "",
      "status": 20,
      "size": "10.00×5.50",
      "area": 55,
      "price": 1501.00,
      "deposit": null,
      "rentableDate": "",
      "days": -1,
      "reservedInfo": "",
      "reservedID": "",
      "orderID": "PO201903040002",
      "reservedPayed": 0,
      "remarks": "",
      "updateTime": "04/21/2019 16:12:35",
      "address": null,
      "picAlls": null
    }
  ]
}
```
  字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  id   |  String | id
  name   | String  | 名称
  storeId| String | 门店Id
  typeName | String | 仓型
  typeId| String | 仓型Id
  discounts| Int| 1特价
  no  |  String   | 编号
  status | Int  | 10未租，20已租，30下架，40预留
  size | String  | 尺寸
  area| Float | 面积
  price| Float | 价格
  deposit| Float | 押金
  rentAbleDate| String | 未租状态:就是可租日期，已租状态:租期至
  days| Int | 未租状态:已空天数，已租状态:还剩下几天 
  reservedInfo| String| 预留信息（如未预留则空）
  reservedID| String | 预留Id
  reservedPayed| Int | 预留单:10未支付,20已支付 (如果未预留此字段无效)
  orderID| String | 订单Id(如果未租则空)
  remarks| String | 备注信息
  updateDateTime | String | 更新时间
  picAlls| String | 仓位图，多个通过","分割
	longitude | String | 经度
	latitude | String | 纬度
	distant | Double | 距离



<a name="-101"><h2>异常分类</h2></a>  
>1、TokenStatus:
>>- _1正常_  
>>- _2无效, 账号被他人登录，或Token被平台回收_  
>>- _3过期, 需要重新刷新token_  
    
>2、Status
>>- _1正常_
>>- _0异常,异常信息见Message_

