# Level 0: Hello Ethernaut

[**OpenZepperlin's Ethernaut**](https://ethernaut.openzeppelin.com/) is a Web3/Solidity-based wargame that provides Capture the Flag (CTF) styled challenges. Each level is a smart contract that needs to be 'hacked', in which we need to read the smart contract code provided, either compromise the contract according to their level objectives or  claim the ownership, drain all the ETH from the contract.

This will be the first article in a series of blog posts where I go through each level talking about how to compromise their contracts and complete the challenges.

## Pre-requisites

Before we play this game, we need some preknowledge, such as [**web3.js**](https://github.com/web3/web3.js), which is the Ethereum JavaScript API.

Ethereum Wallet is a software application that helps us manage Ethereum accounts. It can be used to save private keys, create Ethereum transaction packets, and broadcast on the network as us. [**Metamask**](https://metamask.io/) is a wallet based on browser extension, which runs in Web browsers, such as Chrome. We can use the wallet to link multiple Ethereum nodes and test the blockchain.

We also need some test ethers to interact with the contract. Here's my method to get some ETH on Goerli Testnet.

- You can request 0.2 Goerli ETH every 24h with on [**GOERLI FAUCET**](https://goerlifaucet.com/), simply sign into with a [free Alchemy account](https://alchemy.com/?a=goerli_faucet), enter your wallet address, and hit “Send Me ETH”.
- [**Goerli PoW Faucet**](https://goerli-faucet.pk910.de/) requires some mining work to be done in exchange for free testnet funds. Just enter your ETH Address and start mining. When you've collected enough ETH, stop mining and claim your rewards.

## Game Description

This is the level 0 of Ethernaut game. It explains the basics of getting started, the things you need to know, and the wallet you'll need for the challenges. We can see the following game description:

> This level walks you through the very basics of how to play the game.
>
> #### 1. Set up MetaMask
>
> If you don't have it already, install the [MetaMask browser extension](https://metamask.io/) (in Chrome, Firefox, Brave or Opera on your desktop machine). Set up the extension's wallet and use the network selector to point to the preferred network in the top left of the extension's interface. Alternatively you can use the UI button to switch between networks. If you select an unsupported network, the game will notify you and bring you to the default Goerli testnet.
>
> #### 2. Open the browser's console
>
> Open your browser's console: `Tools > Developer Tools`.
>
> You should see a few messages from the game. One of them should state your player's address. This will be important during the game! You can always see your player address by entering the following command:
>
> ```
> player
> ```
>
> Keep an eye out for warnings and errors, since they could provide important information during gameplay.
>
> #### 3. Use the console helpers
>
> You can also see your current ether balance by typing:
>
> ```
> getBalance(player)
> ```
>
> ###### NOTE: Expand the promise to see the actual value, even if it reads "pending". If you're using Chrome v62, you can use `await getBalance(player)` for a cleaner console experience.
>
> Great! To see what other utility functions you have in the console type:
>
> ```
> help()
> ```
>
> These will be super handy during gameplay.
>
> #### 4. The ethernaut contract
>
> Enter the following command in the console:
>
> ```
> ethernaut
> ```
>
> This is the game's main smart contract. You don't need to interact with it directly through the console (as this app will do that for you) but you can if you want to. Playing around with this object now is a great way to learn how to interact with the other smart contracts of the game.
>
> Go ahead and expand the ethernaut object to see what's inside.
>
> #### 5. Interact with the ABI
>
> `ethernaut` is a `TruffleContract` object that wraps the `Ethernaut.sol` contract that has been deployed to the blockchain.
>
> Among other things, the contract's ABI exposes all of `Ethernaut.sol`'s public methods, such as `owner`. Type the following command for example:
>
> `ethernaut.owner()` or `await ethernaut.owner()` if you're using Chrome v62.
>
> You can see who the owner of the ethernaut contract is.
>
> #### 6. Get test ether
>
> To play the game, you will need test ether. The easiest way to get some testnet ether is via a valid faucet for your chosen network.
>
> Once you see some coins in your balance, move on to the next step.
>
> #### 7. Getting a level instance
>
> When playing a level, you don't interact directly with the ethernaut contract. Instead, you ask it to generate a **level instance** for you. To do so, click the blue button at the bottom of the page. Go do it now and come back!
>
> You should be prompted by MetaMask to authorize the transaction. Do so, and you should see some messages in the console. Note that this is deploying a new contract in the blockchain and might take a few seconds, so please be patient when requesting new level instances!
>
> #### 8. Inspecting the contract
>
> Just as you did with the ethernaut contract, you can inspect this contract's ABI through the console using the `contract` variable.
>
> #### 9. Interact with the contract to complete the level
>
> Look into the level's info method `contract.info()` or `await contract.info()` if you're using Chrome v62. You should have all you need to complete the level within the contract. When you know you have completed the level, submit the contract using the submit button at the bottom of the page. This sends your instance back to the ethernaut, which will determine if you have completed it.
>
> ##### Tip: don't forget that you can always look in the contract's ABI!

## Solution

I assume you've read instructions of the level 0 and acquired test ether.

Ethernaut works in such a way that when you click on `Get new instance` and confirm the transaction on MetaMask, the Solidity contract methods are injected right into our browser console, so we can start calling methods. We can see the following informations in browser console:

```solidity
Hello Ethernaut
Type help() for a listing of custom web3 addons
=> Level address
0xBA97454449c10a0F04297022646E7750b8954EE8
=> Player address
0x7328c28323b00236C0556cbA56E26ce0f3a0abcd
=> Ethernaut address
0xD2e5e0102E55a5234379DD796b8c641cd5996Efd
```

With `help()` to see what other utility functions we have in the console type:

| (Index)                    | Value                                                  |
| -------------------------- | ------------------------------------------------------ |
| contract                   | 'current level contract instance (if created)'         |
| ethernaut                  | 'main game contract'                                   |
| fromWei(wei)               | 'convert wei units to ether'                           |
| getBalance(address)        | 'gets balance of address in ether'                     |
| getBlockNumber()           | 'gets current network block number'                    |
| getNetworkId()             | 'get ethereum network id'                              |
| instance                   | 'current level instance contract address (if created)' |
| level                      | 'current level contract address'                       |
| player                     | 'current player address'                               |
| sendTransaction({options}) | 'send transaction util'                                |
| toWei(ether)               | 'convert ether units to wei'                           |
| version                    | 'current game version'                                 |

You can interact with the contract's functions using `await contract.function_name()` from your browser's console.

Please enjoying this game when you call methods in console:

```solidity
> player
'0x7328c28323b00236C0556cbA56E26ce0f3a0abcd'
> await getBalance(player)
'1.124592918456526974'
> await ethernaut.owner()
'0x09902A56d04a9446601a0d451E07459dC5aF0820'
> await getBalance('0x09902A56d04a9446601a0d451E07459dC5aF0820')
'162.693095474383281907'
// Oh! What a fucking rich man.
> await contract.info()
'You will find what you need in info1().'
> await contract.info1()
'Try info2(), but with "hello" as a parameter.'
> await contract.info2("hello")
'The property infoNum holds the number of the next info method to call.'
// Note that infoNum() returns a BigNumber object so we convert it to string to see actual number.
> await contract.infoNum().then(v => v.toString())
'42'
> await contract.info42()
'theMethodName is the name of the next method.'
> await contract.theMethodName()
'The method name is method7123949.'
> await contract.method7123949()
'If you know the password, submit it to authenticate().'
// Maybe there is a password() method for contract to return the password?
> await contract.password()
'ethernaut0'
> await contract.authenticate('ethernaut0')
⛏️ Sent transaction ⛏ https://goerli.etherscan.io/tx/0xfd2618f4553b171f7598b141f2657325f9edcc0aa13d57e92b89a05e7766e381
⛏️ Mined transaction ⛏ https://goerli.etherscan.io/tx/0xfd2618f4553b171f7598b141f2657325f9edcc0aa13d57e92b89a05e7766e381
{tx: '0xfd2618f4553b171f7598b141f2657325f9edcc0aa13d57e92b89a05e7766e381', receipt: {…}, logs: Array(0)}
// Submitting level instance
☚ (<‿<)☚ Submitting level instance... <  < <<PLEASE WAIT>> >  >
⛏️ Sent transaction ⛏ https://goerli.etherscan.io/tx/0xc597b2b711f361bac754dddd409e883786982f31b1530da2030f59f0410f5f6b
⛏️ Mined transaction ⛏ https://goerli.etherscan.io/tx/0xc597b2b711f361bac754dddd409e883786982f31b1530da2030f59f0410f5f6b
ᕦ(ò_óˇ)ᕤ Well done, You have completed this level!!!
ᕦ(ò_óˇ)ᕤ Well done, You have completed this level!!!
ᕦ(ò_óˇ)ᕤ Well done, You have completed this level!!!
```

Level completed! [**Go to the next level**](Fallback.md).