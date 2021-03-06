1.什么是对称加密？
在对称加密算法中，加密和解密使用的是同一把钥匙，即：使用相同的密匙对同一密码进行加密和解密；

2.什么是非对称加密？

非对称加密有两个钥匙，及公钥（Public Key）和私钥（Private Key）。公钥和私钥是成对的存在，如果对原文使用公钥加密，则只能使用对应的私钥才能解密；因为加密和解密使用的不是同一把密钥，所以这种算法称之为非对称加密算法。

非对称加密算法的密匙是通过一系列算法获取到的一长串随机数，通常随机数的长度越长，加密信息越安全。通过私钥经过一系列算法是可以推导出公钥的，也就是说，公钥是基于私钥而存在的。但是无法通过公钥反向推倒出私钥，这个过程的单向的。

## 用处

对称加密与算法的加密和解密密钥相同，算法速度快，但在密钥交换环节容易出现泄密，主要用在大量文档主体的加密；
非对称加密与算法的加密和解密密钥不同，加解密的计算量大，但不存在密钥的交换问题，安全性好，主要用在数字签名，对称密钥的加密、数字封装等方面。
