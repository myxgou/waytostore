# **对外开放接口说明文档**

## 微兔软件（香港）有限公司


- [**对外开放接口说明**](#0)
  - [接口说明](#-01)
  - [token签名说明](#-02)
- [**授权接口**](#1)
  - [获取域名](#-11)
  - [根据用户名和密码获取Token](#-12)
  
- [**门店查询接口**](#2)
  - [门店列表查询](#-21)
- [**仓位查询接口**](#3)
  - [获取空仓列表](#-31)
- [**接口异常**](#100)
  - [异常说明](#-101)

<!-- TOC END -->

<font color="#ff0000" size=5 face="黑体">测试域名:w2smapit.wayto.store</font>


<a name="-01"><h2>接口说明</h2></a>

### 1、请求体构成

> ### 1.URL:https://{Domain}/api/{Language}/{Path} 

>>- _Domain:主域默认是w2smapit.wayto.store，不同的公司由平台派发_
>>- _Language:当前的语言（目前仅支持中文zh-cn、英文us-en、繁体zh-hk）_  
>>- _Path: API路径_
  
>2.参数组成(如果是*Get*请将以下参数追加到URL后，如果是*POST*请将以下参数追加在body里)
>>- _fromFlag来源微信端:10,IOS端:20,安卓端:30,其他市场: 40_
>>- _token接口签名_


### 2、响应体构成

<font color="#ff0000" size=2 face="黑体">请求统一返回JSON数据</font>

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

<a name="-11"><h2>获取域名</h2></a> 

### _[GET]Path:app/getDomainById_  

 字段 |  类型 | 必填 |   说明
------ |--------|----| ----------------------------------------------------
  licenseID   |  String | Y | 授权码
 

###  返回结果
 
```
{
    "info": {
        "status": 1,
        "message": ""
    },
    "data": {
        "domain": "https://w2smapit.wayto.store/",
        "daysRemaining": 505
    }
}

```
 字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  domain   |  String | 返回当前企业域名
  daysRemaining   | Int  | 该企业有效期剩余天数
  
 
<a name="-12"><h2>根据用户名和密码获取Token</h2></a> 
### _[POST]Path:account/getToken  

  字段 |  类型 | 必填 |   说明
------ |--------|----| ----------------------------------------------------
 userName   |  String | Y | 账号
 pwd   |  String | Y | 密码
 isMD5   |  Int | Y | 是否MD5加密，1需要加密，0不用加密
 
### 返回结果
```
{
    "info": {
        "status": 1,
        "message": ""
    },
    "data": {
        "token": "1EF35F939491E9FDC54A1168FCFA0675DA8D545751BD61F5BC16E464139C9087052343EA073943DC9A59A92A80A78B04F40489FC4C11A9E8"
    }
}
```
 字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  token | String | 当前登录token
 

<a name="-21"><h2>门店列表查询</h2></a>  

### _[GET]Path:store/getlist_  

### 请求参数   

 字段 |  类型 | 必填 |   说明
------ |--------|----| ----------------------------------------------------
 page |  Int | Y | 从1开始
  rows | Int| Y | 每页显示条数
  storeName| String| N | 检索门店通过名字，空代表全部
  fromflg| Int | Y | 请求来源
  token |  String | Y | 登陆凭证


### 返回结果


```
{  
  "info": {  
    "status": 1,
    "message": ""
  },
  "data": [
    {
      "storeID": "2019041914281048143830fe44b7aca",
      "storeName": "Wine Banc Delta House",
      "storeNo": "No.0001",
      "district": "Central Area",
      "adress": "2 Alexandra Road Delta House 03-01E S(159919)",
      "longitude": "",
      "latitude": "",  
      "unitsQuantity": 10,
      "vacantQuantity": 0,  
      "company": "youku",  
      "companyLogo": "",  
      "companyID": ""
    }
  ]
}
```

  字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  storeID   |  String | 门店id
  storeName   | String  | 门店名称
  storeNo  |  String   | 门店编号
  district | String  | 地区
  adress | String  | 地址
longitude | String | 经度
latitude | String | 纬度
unitsQuantity | Int | 总仓数量
vacantQuantity | Int | 空仓数量
company | String | 所属公司名
companyLogo | String| 公司logo
companyID | String | 所属公司Id

<a name="-31"><h2>获取空仓列表</h2></a>  


###  _[GET]Path:_units/getVacantList_

### 请求参数  


  字段 |  类型  | 必填  |  说明
------ |--------| ------| ----------------------------------------------------
  page   |  Int |  Y  |  请求页数,1开始
  rows   | Int  | Y |   每页显示条数
  sort  |  String | N | 排序字段，默认updatetime,仓号：unitName价格:price面积：area体积：volume  
  order | String | N  | desc |排序规则: asc降序, desc降序
storeID | String  | N | 门店编号,选择的门店编号，多个逗号分隔传输
unitName | String  | N | 仓号，支持模糊查询
rangeFrom | String  | N | 最小面积
rangeTo | String  | N | 最大面积
priceFrom | String  | N |最低价格
priceTo | String  | N | 最高价格
fromflg| Int | Y | 请求来源
token   |  String | Y | 登陆凭证

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
      "unitID": "201903021911078170873c25c323214",
      "unitName": "XL0069",
      "storeName": "大唐西里",
      "storeID": "20181022110914965380570166b7ae3",
      "distant": 100,
      "longitude": "",
      "latitude": "",
      "hot": 0,
      "size": "10.00×5.50",
      "area": 55,
      "price": 1501.00,
      "specialInfo": "",
      "unitPic": "",
      "updateTime": "04/21/2019 16:12:35"
    }
  ]
}
```
  字段 |  类型  |   说明
------ |--------| ----------------------------------------------------
  unitID   |  String | 仓位编号
  unitName   | String  | 仓位号
  storeName | String | 门店名称
  storeID | String | 门店编号
  distant | Float | 距离
  hot | Int| 1特价
  longitude | String | 经度
  latitude | String | 纬度
  size | String  | 尺寸
  area| Float | 面积
  price| Float | 价格
  specialInfo | String | 特殊说明，如有柱子
  updateDateTime | String | 更新时间
  unitPic| String | 仓位图，多个通过","分割



<a name="-101"><h2>异常分类</h2></a>  
>1、TokenStatus:
>>- _1正常_  
>>- _2无效, 账号被他人登录，或Token被平台回收_  
>>- _3过期, 需要重新刷新token_  
    
>2、Status
>>- _1正常_
>>- _0异常,异常信息见Message_

