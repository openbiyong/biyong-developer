BiYong IOS 商户介入流程
==============

[BiYong 官网](https://www.biyong.sg)&nbsp;


接入前需要了解的关键词:

* [scheme](https://github.com/openbiyong/biyong-developer/blob/master/BiYong-Merchant-IOS-AccessProcess.md#scheme%E5%8D%8F%E8%AE%AE) — 唤起授权的一种协议方式。
* [encodeURIComponent](https://www.biyong.sg) — 一种编码方式。
* [token](https://github.com/openbiyong/biyong-developer/blob/master/BiYong-Merchant-IOS-AccessProcess.md#token) — 商户平台的登录态。
* [biytoken](https://github.com/openbiyong/biyong-developer/blob/master/BiYong-Merchant-IOS-AccessProcess.md#biytoken) — 获取BiYong授权后的登录态。
* [merchantid](https://github.com/openbiyong/biyong-developer/blob/master/BiYong-Merchant-IOS-AccessProcess.md#merchantid) — 商户在BiYong授权平台申请的商户ID。

整体授权流程图
==============
看 `图`

<img src="https://i.postimg.cc/LsZxj8gf/5ae1731ee4b04f3db58434a0.png"><br/>


关键词解释
==============

### token

```
商户自己的登录态token
Tips:当生成此token 请务必保存一份到BiYong 服务端
```

### biytoken

```
授权BiYong 登录态token
```

### merchantid

```
商户在BiYong 平台的商户id
Tips:授权成功后，会在回调的url上增加biytoken参数，拿到此参数，请换取用户的open_id 
```

### scheme协议

1：url 需要授权成功后吊起的商户平台的URL <br>
2：merchantid 自然是商户在BiYong平台申请的商户id

```
biyong://authorApp?url=xxx&merchantid=xxx

发起授权请求需要执行此协议

```

H5-DEMO
==============

http://authortest.btcchat.io/authorH5.html<br/>

<img src="https://i.postimg.cc/MpSw0k1J/Wechat_IMG152.png"><br/>



授权测试环境Demo
==============
我们含有测试环境的授权流程demo，便于开发过程中使用<br/>

点击下载测试环境授权Demo工程（[授权App下载](https://www.pgyer.com/zngr)).<br/>

[查看详细流程](https://github.com/openbiyong/biyong-developer/blob/master/BiYong-Merchant-IOS-AccessProcess.md#%E8%A7%A3%E5%88%A8%E6%B5%81%E7%A8%8B%E5%9B%BE%E6%96%87%E8%AF%A6%E8%A7%A3)

需求
==============
App `iOS 10.0+` and `Xcode 8.0+`.

<br/><br/>

解说流程（图文详解）
==============
- 启动App <br/>
<img src="http://thyrsi.com/t6/377/1538122755x1822611383.png"><br/>
- 扫码获取测试用户token <br/>
<img src="http://thyrsi.com/t6/377/1538122800x1822611383.png"><br/>
<img src="http://thyrsi.com/t6/377/1538122838x1822611383.png"><br/>
- 不填写信息，会直接走BiYong测试商户授权流程 demoJS<br/>
<img src="http://thyrsi.com/t6/377/1538122865x1822611383.png"><br/>
- 获取授权信息 <br/>
<img src="http://thyrsi.com/t6/377/1538122889x1822611383.png"><br/>
- 发起授权，会打开新的Url，并且带biytoken，让商户获取平台openid <br/>
<img src="http://thyrsi.com/t6/377/1538122918x1822611383.png"><br/>
<br/>
<br/>
<br/>
- gif <br/>
<img src="http://thyrsi.com/t6/377/1538124606x-1566688526.gif"><br/>


线上测试流程
==============



