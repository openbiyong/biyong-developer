# BiYong商户平台接口文档

返回值统一格式：
    
    {
      "status":"0",                 // 0 代表接口请求成功
      "message":"",                 // 错误信息，请求成功无此字段
      "timestamp":"1525616709780",  // 时间戳
      "data": {}                    // JSON格式业务数据，每个接口返回数据格式请参考以下文档
    }

部分错误码：

|`status`|`描述`|
|---|---|
|100000|通用业务错误，具体信息参考 message|
|100100|调用异常|
|100101|商户无权访问此接口|
|100102|用户未授权商户访问此数据|
|100400|请求时间戳与服务器误差超过限制|
|100401|验签失败|

## 1. BiYong用户相关接口

### 1. 用户授权

> /biyong-user/auth

请求参数:

    {
      "authToken":"EuQkJ3mx4kinvHLFR3DPQ15SjVyh9" // 必填 授权临时token
    }

返回data:

    {
      "authList": [   // 用户授予的权限列表
        "PUBLIC_INFO",// 查看公开信息(昵称、头像等)权限
        "PHONE",      // 查看手机号权限
        "BALANCE"     // 查看资金流水权限
      ],
      "openId":"9bd0fda7d0dad3411fed9c488bd4b8a7"
    }

### 2. 上传用户token

> /biyong-user/update

请求参数:

    {
      "openId":"9bd0fda7d0dad3411fed9c488bd4b8a7", // 必填 用户openId
      // 以下参数不能全为空
      "token":"00000000000000000000000000000000"  // 商户生成token(必须为1~64位字母、数字、下划线的组合)，可用于保持用户在商户页面登录态
    }

返回data: null

### 3. 用户基本信息

> /biyong-user/info

请求参数:

    {
      "openId":"387f6d41e60449cb84c8587b1d6d4104" // 必填 用户openId
    }

返回data:

    {
      "auth":"true",              // true/false 是否有查看此用户信息权限
      "pubInfoAuth":"true",       // true/false 是否有查看用户公开信息权限
      "firstName":"John",         // 如果 pubInfoAuth 为 false，此字段不返回
      "lastName":"Due",           // 如果 pubInfoAuth 为 false，此字段不返回
      "selfieUrl":"http://..."    // 用户头像链接，如果 pubInfoAuth 为 false 或用户无头像，此字段不返回
      "isKycPass":"true",         // 如果 pubInfoAuth 为 false，此字段不返回，true/false 用户是否进行了实名认证
      "phoneAuth":"true",         // true/false 是否有查看用户手机号
      "phone":"86-13300000000",   // 如果 phoneAuth 为 false，此字段不返回
    }


### 4. 获取用户资金流水

> /biyong-user/balance/page

请求参数:

    {
      "openId":"387f6d41e60449cb84c8587b1d6d4104", // 必填 用户openId
      "pageNo":"0",                                // 必填 页码，从0计数
      "pageSize":"100",                            // 必填 每页条数，最大1000
      "startTime":"1525335516797",                 // 可选 数据起始时间
      "endTime":"1525335516797"                    // 可选 数据截止时间
    }

返回data:

    {
      "total":1200, // 总条数
      "records" : [
        {
          "tokenSymbol":"ACT",   // 代币BiYong内部代码
          "balance":10.32,       // 资产变更数量，负数代表资产扣减
          "time":1525335516797   // 资产变更时间
        },
        ...(more records)...
      ]
    }

## 2. 其它接口

### 1. BiYong支持的代币列表查询

> /common/token-info/page

请求参数:

    {
      "pageNo":"0",                                // 必填 页码，从0计数
      "pageSize":"100",                            // 必填 每页条数，最大1000
    }

返回data:

    {
      "total":123, // 总条数
      "records" : [
        {
          "symbol":"ACT",          // 代币BiYong内部代码
          "name":"ACT",            // 代币名称
          "projectName":"Achain",  // 代币项目名称
        },
        ...(more records)...
      ]
    }

