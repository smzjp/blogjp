---
original: true
title: 关键字defer
description: defer延迟函数
date: 2022-12-20
slug: 
image: 
tags:
    - defer
categories:
    - Golang
---



### 安装

1. 在mac系统下 执行 `curl -L https://foundry.paradigm.xyz | bash`
2. 如果提示 安装 **libusb**，执行 `brew install libusb`
3. 重启一个终端或者更新下环境变量的设置。然后执行 foundryup

### 命令

有三种命令

1. **forge**  可用来测试、构建和部署您的智能合约。
2. 

### 项目

#### 新建项目

1. 在想要存放项目的目录下 执行 `forge init hello_foundry` ，会创建一个默认的模版项目
   1. 如果使用指定的模版项目可以使用 `forge init --template https://github.com/foundry-rs/forge-template hello_template`
   2. 有四个目录，`lib` 存放依赖库，`src` 存放自己写的合约源码的地方。`script` 是存放一些脚本。`test`编写测试用例。
   3. 模版默认安装了一个依赖项：forge-std，这是用于 Foundry 项目的首选测试库。可以在lib目录下看到。
2. `cd hello_foundry`开始工作

#### 已有项目

1. 先克隆项目 `git clone https://github.com/abigger87/femplate`
2. `cd femplate ` 切换到项目目录下。
3. 安装依赖项 `forge install` 会把所有的第三方依赖库下载下来。

#### 依赖

foundry没有自己的项目依赖工具，使用**git submodules** 管理依赖项。

##### 安装依赖

要导入第三方依赖库可以使用 `forge install OpenZeppelin/openzeppelin-contracts` ,具体使用的时候，在需要导入的.sol文件import。如果不清楚路径，可以使用 `forge remappings` 命令，会帮你映射路径，使路径变短方便引用。也可以自定义映射路径，需要在项目根目录新建一个 `remappings.txt` 文件, 然后把需要映射的路径写入，比如 `solmate-utils/=lib/solmate/src/utils/`,  如果要使用 solmate的utils目录下的文件，就可以 `import "solmate-utils/SignedWadMath.sol"；`

##### 更新依赖

1.  更新所有依赖 `forge update`
2. 更新某一个依赖 `forge update lib/solmate`

##### 删除依赖

`forge remove solmate` 或者 `forge remove lib/solmate`





### 编译项目

1. `forge build`

### 测试项目

foundry里面测试用例使用solidity语言写的。

#### 编写测试

1. 执行`forge test`
   1. 也可以执行 `forge test -vvv`。为失败的测试生成跟踪信息
   2. `forge test -vvvv` 为所有的测试用例生成跟踪信息。
   3. `forge test --gas-report` 为合约生成gas报告，可以预估合约将消耗多少gas
      1. 需要现在配置文件`foundry.toml` 配置哪些合约需要生成gas报告。`gas_reports = ["MyContract", "MyContractFactory"]` 或者 `gas_reports = ["*"]`所有的合约都生成。
