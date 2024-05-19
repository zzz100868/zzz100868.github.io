---
title:ethernaut NO.6
tags: TeXt
---

首先我们来看合约的代码和要求

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Delegate {
    address public owner;

    constructor(address _owner) {
        owner = _owner;
    }

    function pwn() public {
        owner = msg.sender;
    }
}

contract Delegation {
    address public owner;
    Delegate delegate;

    constructor(address _delegateAddress) {
        delegate = Delegate(_delegateAddress);
        owner = msg.sender;
    }

    fallback() external {
        (bool result,) = address(delegate).delegatecall(msg.data);
        if (result) {
            this;
        }
    }
}

```

要求：这一关的目标是申明你对你创建实例的所有权

**思路**：触发fallback进行委托调用Delegate合约中的pwn函数，然后就可以将owner设置为msg.sender