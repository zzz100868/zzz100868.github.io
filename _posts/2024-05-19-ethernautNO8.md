---
title: ethernaut NO.8
tags: TeXt
---

首先我们来看代码和要求

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Vault {
    bool public locked;
    bytes32 private password;

    constructor(bytes32 _password) {
        locked = true;
        password = _password;
    }

    function unlock(bytes32 _password) public {
        if (password == _password) {
            locked = false;
        }
    }
}
```

要求：打开vault

**可以通过访问web3库来找到密码**

await web3.eth.getStorageAt(contract.address, 1)

然后就可以得到lock值

这串代码是在使用 Web3.js 库来读取以太坊智能合约的存储状态。

具体来说:

web3.eth.getStorageAt(contract.address, 1)

web3.eth.getStorageAt() 是 Web3.js 提供的一个方法,用于从以太坊节点读取指定合约地址的存储状态。
contract.address 是合约实例的地址。
1 是要读取的存储槽的索引。在 Solidity 中,状态变量按照声明顺序存储在合约的存储空间中,第一个状态变量存储在槽 0,第二个状态变量存储在槽 1,依此类推。
await

await 是 JavaScript 中用于等待异步操作完成的关键字。
因为 web3.eth.getStorageAt() 是一个异步操作,所以需要用 await 来等待结果返回。
综合起来,这段代码的作用是:

获取指定合约地址的存储状态。
读取合约中第二个状态变量的值(因为状态变量是从槽 0 开始存储的)。
等待异步操作完成,并获取读取到的值。
这种读取合约存储状态的方式在某些情况下很有用,比如:

调试合约时,查看特定状态变量的值。
在链下分析合约行为时,获取链上存储的数据。
在合约升级或迁移时,读取旧版本合约的状态变量。
但需要注意,直接读取存储槽可能会有一些局限性,比如无法区分不同类型的状态变量。在实际开发中,最好使用合约提供的公开函数来读取状态,以确保安全性和可靠性。

（有待学习）