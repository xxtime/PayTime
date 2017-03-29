## PayTime移动支付SDK
PayTime移动支付SDK，

* 规范优雅命名  
* 符合PSR标准  
* 支持多种网关支付  

## 支持的网关
* Apple Pay
* Google Pay
* Alipay
* WeixinPay （待开发）
* Paypal
* Mol
* MyCard
* Paymentwall

## 支持沙箱测试的网关
以下网关在回调时支持沙箱测试  

* paymentwall（回调参数：is_test）  
* paypal（回调参数：test_ipn）  

___

## 所需环境
PHP >= 5.5  
composer  

___

## 安装
```shell
composer require "xxtime/paytime:dev-master"
```

___

## 使用方法
```php
<?php

use Xxtime\PayTime\PayTime;

// 不同网关方法略有区别
// $payTime = new PayTime('Mycard_card');
$payTime = new PayTime('Alipay');

$payTime->setOptions(
    array(
        'app_id'      => 123456,
        'private_key' => '/path/to/privateKey.pem',
        'public_key'  => '/path/to/publicKey.pem',
        'return_url'  => 'http://host/returnUrl',
        'notify_url'  => 'http://host/notifyUrl',
    );
);

$payTime->purchase([
    'transactionId' => 2016121417340937383,
    'amount'        => 0.05,
    'currency'      => 'CNY',
    'productId'     => 'com.xxtime.product.1',
    'productDesc'   => '测试产品',
    'custom'        => '自定义',   // 选填
    'userId'        => '123456'   // 选填
]);


try {
    $response = $payTime->send();

    // 个别渠道需要单独处理，例如:MyCard需要存储单号后跳转(其回调无单号)
    // start call service process, only MyCard can get here now
    // do something
    // end call

    if (isset($response['redirect'])) {
        $payTime->redirect();
    }
} catch (\Exception $e) {
    // TODO :: error log
    echo $e->getMessage();
}
```

## 回调方法

```php
<?php

use Xxtime\PayTime\PayTime;

// 订单验证
$payTime = new PayTime('Alipay');
$response = $payTime->notify();
if (!$response->isSuccessful()) {
    exit('失败');
}
echo '成功';
```


*response返回方法：*

参数名 | 返回类型 | 描述
--- | --- | ---
isSuccessful    | bool          | 储值结果
message         | varchar(64)   | 返回消息
transactionId   | varchar(32)   | 订单ID
transactionReference    | varchar(64)   | 充值网关订单ID
amount          | decimal(10,2) | 金额
currency        | varchar(32)   | 币种
card_no         | varchar(32)   | 卡号
raw             | varchar(1000) | 原始信息

___

## 关于项目
主页: [https://github.com/xxtime/paytime](https://github.com/xxtime/paytime)  
作者: [https://www.xxtime.com](https://www.xxtime.com)  
