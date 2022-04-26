
# TRC20-ThinkPhP5.0

源码地址：[源码下载地址](https://www.kancloud.cn/bishengzhu/trc20)  


*****
- 当前SDK目前支持波场的 TRX 和 TRC20 中常用生成地址，转账，余额查询，离线签名等功能。
- 一套写法兼容 TRON 网络中 TRX 货币和 TRC 系列所有通证
- 接口方法可可灵活增减

看云官方文档作者是我，通过QQ找到本人购买源码可以八折优惠，源码会定期优化升级

>[danger] 当前项目演示的合约地址为  `TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t` 精度为 6,如下所示
```
/*基础配置*/
$this->config = [
    'contract_address' => 'TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t',// USDT TRC20
    'decimals' => 6, /*精度*/
];
```

>[warning] 可以加我QQ 1113249273，对于目前的应用当中的查询余额，转账，查账等几个功能可以做技术支持。之外的功能不在技术支持范围内，请悉知。
QQ 群：925283872 ，问题回答正确才能入群，PHP基本知识。


>[info] 代码以及返回参数摘要

 生成地址 generateAddress() 回参
~~~
{
    "privateKey": "0xe2ad74294c273467027f80*********6cc2e9a9cd4214ef3418b818d48e66",
    "address": "TLowZwvVHCQSKH8Pjwgo67TPe2dea7grWa",
    "hexAddress": "4176e8c1d6e77d1ce87a0b242366c26f550556b689"
}
~~~

### 交易转账 transfer($from,$to,$acount)  回参 

>[info] 交易转账回参不能判断是否交易成功，还需要根据回参的 `txID` 参数去调用另一个API来实现，继续往下看
~~~
{
   "code": 200,
   "data": {
      "signature": [],
      "txID": "eb150f82dde7eec67bfa84a433397769d6beccac781e01b62fd5dc76515153dc",
      "raw_data": {
         "contract": [
            {
               "parameter": {
                  "value": {
                     "data": "a9059cbb00032200000000000000004144967f55976c06c4fb55b2e310843c25105ba78d00000000000000000000000000000000000000000000000000000000000f4243",
                     "owner_address": "4144967f222206c4fb55b2e310843c25105ba78d",
                     "contract_address": "41a614f803b22280986a42c78ec9c7f77e6ded13c"
                  },
                  "type_url": "type.googleapis.com/protocol.TriggerSmartContract"
               },
               "type": "TriggerSmartContract"
            }
         ],
         "ref_block_bytes": "cb77",
         "ref_block_hash": "904b72d42bb7d1b8",
         "expiration": 1649065326000,
         "fee_limit": 100000000,
         "timestamp": 1649065268704
      },
      "contractRet": "PACKING"
   }
}
~~~

*****
### 根据交易哈希查询信息 transactionReceipt($txID)  
>[danger] 回参的contractRet参数为SUCCESS为成功，别的均为不成功。虽然上面的交易api也有这个回参，但是并不会告诉你成功与否，你还得单独调用一次这个接口才能知道结果。
~~~
{
   "code": 1,
   "data": {
      "signature": [],
      "txID": "fad174a7b0fcbd10c2e1c9a***974505f7af6609b8e080956dc2111223e6d",
      "raw_data": {
         "contract": [
            {
               "parameter": {
                  "value": {
                     "data": "a9059cbb00000000000000000004144967f55976c06c4fb55b2e310843c25105ba78d0000000000000000000000000000000000000000000000000000000077359400",
                     "owner_address": "4144967f55976c06c4fb52e3143c25105ba78d",
                     "contract_address": "41a614f803b6fd780986c79c7f77e6ded13c"
                  },
                  "type_url": "type.googleapis.com/protocol.TriggerSmartContract"
               },
               "type": "TriggerSmartContract"
            }
         ],
         "ref_block_bytes": "dfef",
         "ref_block_hash": "a0e7bd2508835fe",
         "expiration": 16488814348000,
         "fee_limit": 100000000,
         "timestamp": 1648884290694
      },
      "contractRet": "SUCCESS"
   }
}
~~~

#### 开发环境要求
- php版本 >=7.3
- php扩展务必装 `gmp` 扩展，否则转账不成功
- ThinkPHP5基本运行要求，这个不需要额外多说
#### 案例使用方法
>[info] 在当前的项目中开发，可以看下面这个配置调整，如果需要将当前代码作为接口开放，可以看下一章节

 `application/index/controller/Trc20.php` 里面的 `$this->privateKey` 私钥改成您自己的私钥

剩下的就是 `application/index/controller/Trc20.php`里面封装的几个常用方法稍微看下就明白了。实在不明白，就部署好代码，使用接口测试。



>[info]  API 源码对应目录地址 `application/api/controller/Index.php` ，接口文档如下所示
*****


#  **转账接口**


##### 请求URL
- ` /api/index/transfer `
  
##### 请求方式
- POST/GET 

##### 参数

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|toaddress |是  |string |接收方地址   |
|amount |是  |float | 转账金额（最低为1，精度为6）|

##### 返回示例 

```
{
   "code": 1,
   "data": {
      "signature": [],
      "txID": "eb150f82dde7eec67bfa84a433397769d6beccac781e01b62fd5dc76515153dc",
      "raw_data": {
         "contract": [
            {
               "parameter": {
                  "value": {
                     "data": "a9059cbb00032200000000000000004144967f55976c06c4fb55b2e310843c25105ba78d00000000000000000000000000000000000000000000000000000000000f4243",
                     "owner_address": "4144967f222206c4fb55b2e310843c25105ba78d",
                     "contract_address": "41a614f803b22280986a42c78ec9c7f77e6ded13c"
                  },
                  "type_url": "type.googleapis.com/protocol.TriggerSmartContract"
               },
               "type": "TriggerSmartContract"
            }
         ],
         "ref_block_bytes": "cb77",
         "ref_block_hash": "904b72d42bb7d1b8",
         "expiration": 1649065326000,
         "fee_limit": 100000000,
         "timestamp": 1649065268704
      },
      "contractRet": "PACKING"
   }
}
```

##### 返回参数说明 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|txID |string   |交易哈希值（用于查询转账交易）  |

##### 备注 

- txID 用来查询订单信息从而确定是否交易成功
- 可以查询余额后再转账，否则余额不够转死都转不成功。查询余额的方法返回的是一个负点数金额。


```
/**
 * 获取余额  完整代码可以下载项目源码查看
 * $address 地址对象,通过私钥可以用获取
 * 这里查询的是转账用户的余额
 */
private function getBalance($address)
{
    ...
    /*私钥地址*/
    $balance = $TRC20->balance($address);
    return $balance;
}
```

```
/**
 * 根据私钥获取地址 完整代码可以下载项目源码查看
 * $privateKey 私钥
 */
private function getAddress()
{
    ...
    /*私钥地址*/
    $privateKeyToAddress = $TRC20->privateKeyToAddress($this->privateKey);
    return $privateKeyToAddress;
}
```

*****





#  **查单接口**



##### 请求URL
- ` /api/index/transData `
  
##### 请求方式
- POST/GET 

##### 参数

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|txID |是  |string |交易哈希值   |

##### 返回示例 

```
{
   "code": 1,
   "data": {
      "signature": [],
      "txID": "fad174a7b0fcbd10c2e1c9a***974505f7af6609b8e080956dc2111223e6d",
      "raw_data": {
         "contract": [
            {
               "parameter": {
                  "value": {
                     "data": "a9059cbb00000000000000000004144967f55976c06c4fb55b2e310843c25105ba78d0000000000000000000000000000000000000000000000000000000077359400",
                     "owner_address": "4144967f55976c06c4fb52e3143c25105ba78d",
                     "contract_address": "41a614f803b6fd780986c79c7f77e6ded13c"
                  },
                  "type_url": "type.googleapis.com/protocol.TriggerSmartContract"
               },
               "type": "TriggerSmartContract"
            }
         ],
         "ref_block_bytes": "dfef",
         "ref_block_hash": "a0e7bd2508835fe",
         "expiration": 16488814348000,
         "fee_limit": 100000000,
         "timestamp": 1648884290694
      },
      "contractRet": "SUCCESS"
   }
}
```

##### 返回参数说明 

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|contractRet |string   |交易结果：SUCCESS 代表成功，否则为失败）  |









