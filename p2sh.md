---
title: 比特币的 P2SH 机制
---

P2SH 是比特币的一种机制，略微有些复杂，需要先理解[比特币脚本](bitcoin-scripts)的原理之后才比较容易理解。

## 什么是 P2SH

P2SH 是和比特币地址紧密相关的。

先说说什么是 P2SH 地址。比特币地址就是一个长长的字符串，但是比特币地址是分不同类型的，常见的有两类：Pay-to-PubKeyHash (P2PKH) 和 Pay-to-ScriptHash (P2SH) 。

P2PKH 是最为常见的比特币地址类型，英文全称的意思是”向公钥哈希支付“，这种类型的地址是以1打头，例如，1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2 。

P2SH 是一种比较新的地址类型。英文全称的意思是”向脚本哈希支付“，这种类型的地址是以3打头的，例如，3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy 。

P2PKH 和 P2SH 两类地址的主要差别是，资金的转出条件不同。注意，转出指的是当发送方把比特币转入到地址上之后，接收方再把币转给其他人的这个行为。P2PKH 地址中的钱如果要转出，只需要提供公钥和私钥签名即可，形式比较固定。而 P2SH 中的资金要想转出，转出条件就可以很自由的进行设置。具体来讲，转出条件就是要写到一个赎回脚本(redeem script)中，P2SH 中的 S 代表赎回脚本。

以上就是 P2SH 的基本含义了。

## P2SH 的运行逻辑

P2SH 跟传统方式的最大区别是把设置转出条件的人从发送者变成接收者。这句话非常的不好理解，下面我们来详细介绍一下其中的逻辑。

还是先从 P2PKH 说起，这种传统的方式下，转出条件设置者是比特币的发送者。具体过程是这样：发送者在拿到接收者地址之后，要构建一个交易，转出条件是在交易中的上锁脚本中规定的。详细过程在[比特币脚本](bitcoin-scripts)中有介绍。接收者如果要转出地址中的币，就需要满足上锁脚本中的转出条件。

再来聊 P2SH ，这种新的方式下，转出条件是由接收者来设置。具体过程是这样：

接收者先来构建一个赎回脚本，里面的内容就是转出条件。注意，转出条件在 P2PKH 条件下是由发送者来写到上锁脚本中的。
接收者运算赎回脚本的哈希，这个哈希值就是一个 P2SH 类型的比特币地址，并把这个哈希值发送给发送者。
发送者构建交易，但是这次的交易输出不再有上锁脚本，而是变成了赎回脚本的哈希。具体转出条件是什么，发送者是不知道的，也没有必要知道。
接收者收到币之后，如果想花费这些币就必须去满足赎回脚本中的转出条件。例如，如果设置了适当的转出条件，就可以来实现[多重签名](multi-sig)。

以上就是一个 P2SH 的地址从生成到接收币再到转出币的整个过程。

## P2SH 的意义

P2SH 的思路是在 [BIP16](https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki) 中提出的，目的就是用来实现更为复杂的交易，增加了比特币的可编程货币的特性。

P2SH 最知名的一个应用就是来实现多重签名。就像前面讨论过的，如果接收者在赎回脚本中添加适当的转出条件，就可以把脚本哈希变成一个多重签名地址。比特币一旦转入这个地址就需要多个数字签名才能转出。多重签名机制使得支付通道和闪电网络成为可能，而这些都离不开 P2SH 这种机制。

所以说 P2SH 是有非常强的现实意义的。

## 总结
关于 P2SH 我们就聊到这里了。重点内容是 P2SH 是区别于传统的 P2PKH 的一种比特币机制。P2SH 跟传统方式的最大区别是把设置转出条件的人从发送者变成接收者。通过在赎回脚本中添加各种转出条件可以来实现形式多样的交易，例如多重签名交易。


参考：

- https://www.youtube.com/watch?v=6gqMPEh78Oc 
- https://www.mycryptopedia.com/p2sh-pay-to-script-hash-explained/ 
- https://github.com/bitcoin/bips/blob/master/bip-0016.mediawiki
