#BiYong 前后端交互流程

对接准备

1. 在[BiYong开放平台](https://open.biyong.sg)注册成为商户
2. 等待审核通过
3. 创建一个APP
4. 部署前端页面以及后端应用，可参考[服务Demo(Html5+Java)](https://github.com/openbiyong/merchant-server-demo-java)

## 1. BiYong用户授权

#### [授权流程时序图](https://www.processon.com/view/link/5ae1731ee4b0411f64cfa46d)

1. 在手机端使用以下js代码进行拉起 BiYong App 进行授权

       function biyAuth() {
         let url = encodeURIComponent("https://your.domain.com"); // 跳转url必须与你的App内填写的域名相同
         let appId = "036cf7b2fe0f5f1239ba26aa9b37665a";          // 你的App的appId
         window.location.href = "biyong://authorApp?url=" + url + "&appid=" + appId;
       }

2. 用户在 App 内确认授权

3. BiYong App 会跳转至协议填写的 url，并附带参数 `biytoken`

       https://your.domain.com?biytoken=aBcdeFghIJkLmNOpqRstUvWxYz1234567890

4. 商户页面将 `biytoken` 传输到商户后台应用

5. 后台应用访问[用户授权接口](https://github.com/openbiyong/biyong-developer/blob/master/BiYong商户后台接口文档.md#1-用户授权)获取用户信息

   > 后台可以保存用户 `openId`，作为 BiYong 用户标识，用于[查询用户信息](https://github.com/openbiyong/biyong-developer/blob/master/BiYong商户后台接口文档.md#3-用户基本信息)等
   > <br>通过访问[上传用户token接口](https://github.com/openbiyong/biyong-developer/blob/master/BiYong商户后台接口文档.md#2-上传用户token)用户可以在 BiYong App 内保持用户的登录态，从BiYong发现页内打开商户页面url会附带此参数(token=xxx)
   > <br>后台通信成功后，前端页面可提示用户授权成功

## 1. BiYong支付

#### [支付流程时序图](https://www.processon.com/view/link/5c493ec5e4b0641c83e7ec07)

1. 后台应用访问[创建支付订单接口](https://github.com/openbiyong/biyong-developer/blob/master/BiYong商户后台接口文档.md#2-创建-biyong-支付订单)

   调用成功后，接口会返回参数 `payToken`

2. 在手机端使用以下js代码进行拉起 BiYong App 进行支付

       function biyAuth() {
         let url = encodeURIComponent("https://your.domain.com"); // 用户支付完成后跳转的url
         let payToken = "创建订单接口返回的payToken"
         window.location.href = "biyong://biypay?payToken=" + payToken + "&callbackUrl=" + url;
       }
             
   > 在 callbackUrl 添加订单号参数，用户支付完成后可以通过订单号追踪支付状态
   
       https://your.domain.com/pay_finish?outOrderCode=ORDER111222333

3. 用户在 App 内完成支付

   > 如果通过[设置回调接口](https://github.com/openbiyong/biyong-developer/blob/master/BiYong商户后台接口文档.md#7-设置回调信息)配置了回调信息，用户支付完成后会尝试通知支付成功
   > <br>可能会有多个不同IP发起此通知，请验证签名确保通知是真实的

4. 支付完成后 BiYong App 会跳转至协议填写的 `callbackUrl`

   > 如果未收到支付成功的通知，请使用后台应用访问[订单查询接口](https://github.com/openbiyong/biyong-developer/blob/master/BiYong商户后台接口文档.md#3-查询单笔-biyong-支付订单)根据订单状态确认是否支付成功

   