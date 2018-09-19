# BiYong商户RSA公钥生成指南

## 什么是公钥和私钥？

公钥和私钥就是俗称的不对称加密方式。公钥（Public Key）与私钥（Private Key）是通过一种算法得到的一个密钥对（即一个公钥和一个私钥），公钥是密钥对中公开的部分，私钥则是非公开的部分。公钥通常用于加密会话密钥、验证数字签名，或加密可以用相应的私钥解密的数据。

通过这种算法得到的密钥对能保证在世界范围内是唯一的。使用这个密钥对的时候，如果用其中一个密钥加密一段数据，则必须用另一个密钥才能解密。比如用公钥加密的数据就必须用私钥才能解密，如果用私钥进行加密也必须用公钥才能解密，否则将无法成功解密。

## 数字证书的原理

数字证书采用公钥体制，即利用一对互相匹配的密钥对进行加密、解密。每个用户自己设定一把特定的仅为本人所知的私有密钥（私钥），用它进行解密和签名；同时设定一把公共密钥（公钥）并由本人公开，为一组用户所共享，用于加密和验证签名。

由于密钥仅为本人所有，这样就产生了别人无法生成的文件，也就形成了数字签名。

数字证书是一个经证书授权中心（CA）数字签名的包含公开密钥拥有者信息以及公开密钥的文件。最简单的证书包含一个公开密钥、名称以及证书授权中心的数字签名。数字证书还有一个重要的特征就是只在特定的时间段内有效。



## 创建私钥
BiYong对您的私有密钥的加密算法和长度有如下要求：

1. 加密算法使用RSA算法
2. 加密长度至少2,048位

### 使用OpenSSL工具生成私钥

OpenSSL是一个强大且应用广泛的安全基础库工具，您可以从 [http://www.openssl.org/source/](http://www.openssl.org/source/) 下载最新的OpenSSL工具安装包。

```
openssl genrsa -out rsa_private.key 2048
```

### 通过私钥导出RSA公钥

```
openssl rsa -in rsa_private.key -pubout -out rsa_public.key
```

###  处理成BiYong需要的格式

将以上文件rsa_public.key，使用文本编辑器打开。

假设rsa_public.key内容如下。

```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA4NSO7HUnep/TwHRcSL1L
AIiZ9thacOa/uIeTpMiTKfMyimB91s9x96ubV0w+kO6rIAv8o1uKzDGlGSbHZXKF
1vXweO3kveZc+c6ZhMjFoUtiKz4lsdi5r3D5JfXI/jZabWqGMoZ3AFxySQimiiFr
GgywRYJgR5GDmgDgVF87Bb3UL1jVedA+HdtUSq3yI9QOkqvV6tiKk04DAoQ9pbQW
/mm8/jjK5AVNIBLGHseJg7ojoOahokl9tW37k1IGvX/JAiR8yplUrwEwt26fcM2t
Imlfw2fsp8UJWKf5QzqyFDdbwvNxM25YOqPJ8RocSRpkMhAwIxmY1IK1l7/CACS+
CQIDAQAB
-----END PUBLIC KEY-----
```

删除掉以下几行内容
-----BEGIN PUBLIC KEY-----
-----END PUBLIC KEY-----

将剩余内容拼接为一行内容。最终结果如下。

```
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA4NSO7HUnep/TwHRcSL1LAIiZ9thacOa/uIeTpMiTKfMyimB91s9x96ubV0w+kO6rIAv8o1uKzDGlGSbHZXKF1vXweO3kveZc+c6ZhMjFoUtiKz4lsdi5r3D5JfXI/jZabWqGMoZ3AFxySQimiiFrGgywRYJgR5GDmgDgVF87Bb3UL1jVedA+HdtUSq3yI9QOkqvV6tiKk04DAoQ9pbQW/mm8/jjK5AVNIBLGHseJg7ojoOahokl9tW37k1IGvX/JAiR8yplUrwEwt26fcM2tImlfw2fsp8UJWKf5QzqyFDdbwvNxM25YOqPJ8RocSRpkMhAwIxmY1IK1l7/CACS+CQIDAQAB
```

* 无论您通过哪种方式生成密钥，请您完善地保管好您的私钥文件，私钥文件一旦丢失或者损坏，您申请的对应的公钥、及数字证书都将无法使用。