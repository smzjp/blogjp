---
original: true
title: solidity语法
description: solidity的使用
date: 2022-12-20
slug: 
image: 
tags:
    - defer
categories:
    - Golang
---



### 类型

#### 引用类型

1. **memory**内存  声明周期只存在于函数调用期间
2. **storage**存储 状态变量保存的位置，gas开销最大
3. **calldata**调用数据 用于函数参数不可变存储区域 



#### abi编码

1. abi：application binary interface 应用程序二进制接口
2. abi接口：定义如何与合约交互
3. abi编码
   1. 函数选择器：对函数签名计算keccak-256哈希，取前4个字节
   2. 参数编码：

调用一个合约函数 = 向合约地址发送一个交易（交易的内容就是ABI编码数据）



#### 修饰符

1. **view** 修饰函数（视图函数）不修改状态
2. **pure** 修饰函数（纯函数）不修改状态，不读取状态，跟合约状态无关
3. **payable** 表示函数可以被支付eth，支付的eth值用msg.value 获取
4. **modifier** 函数修改器，用来修饰其他函数



#### 继承

1. 使用关键字 **is** 
2. 部署合约的时候只会生成一个合约，基类合约的代码会被编译进子类合约
3. 除了private修饰的变量或者函数，其他子类都可以访问

#### 抽象合约

1. 不可以被单独部署，使用关键字 **abstract** 修饰
2. 函数没有实现，使用 **virtual** 修饰，表示函数可以被重载
3. 抽象合约可以作为基类，子类可以通过重载(使用关键字**override**)实现基类的函数，**override**表示重写了父类合约函数
4. 调用父合约函数使用 `super.xxx`

#### 库

1. 定义库使用关键字 **library**





polygon网络是matic网络改名来的，polygon不是专有区块链，它是以太坊区块链之上的第2层

Mumbai 是polygon的测试网



因此，Polygon可以与其他竞争网络相提并论，例如[Solana](https://www.kraken.com/zh-cn/prices/solana)、 [Polkadot](https://www.kraken.com/zh-cn/prices/polkadot)、[Cosmos](https://www.kraken.com/zh-cn/prices/cosmos)和[Avalanche](https://www.kraken.com/zh-cn/prices/avalanche)。





#### 传统应用

前端 《===============》 后端服务器



#### web3应用



#### ABI和字节码

1. 合约 ABI 是与 EVM 字节码交互的接口
2. ABI包括了函数的结构和实参
3. 因为evm里面存放的是字节码（源码编译来的），字节码也需要调用函数。所以有了ABI用来交互和字节码。



wagmi



#### 数据平台

Thegraph

Dune 





#### 合约自动化调用

1. chainlink automation
2. Gelato network
3. 
