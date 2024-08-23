# DeFi Kingdom Clone on Avalanche

## Overview

This repository provides a guide and example code for creating a DeFi Kingdom clone on the Avalanche network. Follow the steps below to set up your EVM subnet, define your native currency, connect to Metamask, and deploy the necessary smart contracts to build the foundational elements of your game.

## Prerequisites

Before you begin, ensure you have the following tools installed:

- Unix-based Computer (MacOS or Linux)
- [Avalanche CLI](https://docs.avax.network/build/avalanchego-setup)
- [Node.js](https://nodejs.org/)
- [Metamask](https://metamask.io/)
- [Solidity](https://soliditylang.org/)
- [Remix IDE](https://remix.ethereum.org/)

## Steps to Create Your DeFi Kingdom Clone

### 1. Set Up Your EVM Subnet

Use the [Avalanche CLI](https://docs.avax.network/build/avalanchego-setup) and follow the [Avalanche documentation](https://docs.avax.network/) to create a custom EVM subnet on the Avalanche network.

### 2. Define Your Native Currency

You can set up your own native currency, which will serve as the in-game currency for your DeFi Kingdom clone. Below is an example of an ERC20 token contract:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "DeFi Kingdom Token";
    string public symbol = "DFKT";
    uint8 public decimals = 18;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

### 3. Connect to Metamask

Connect your EVM Subnet to Metamask by following these steps:

1. Open Metamask and click on the network dropdown.
2. Select "Custom RPC".
3. Enter your EVM Subnet details as outlined in the guide.
4. Save and switch to your new network.

### 4. Deploy Basic Building Blocks

Use Solidity and Remix to deploy the basic building blocks of your game, such as smart contracts for battling, exploring, and trading. These contracts will define the game rules, such as liquidity pools, tokens, and more.

#### Example: Vault Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }

    function deposit(uint _amount) external {
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }
        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }
}
```

### 5. Connect Remix to Metamask

1. Open [Remix IDE](https://remix.ethereum.org/).
2. Go to the "Deploy & Run Transactions" tab.
3. Select "Injected Web3" from the environment dropdown.
4. Ensure Metamask is connected to your EVM subnet and Remix is using the same network.

### 6. Deploy the Smart Contracts

1. Compile your smart contracts in Remix.
2. Deploy them using the "Deploy" button in the "Deploy & Run Transactions" tab.

### 7. Test Your Application

1. Use Remix to interact with your deployed smart contracts.
2. Deploy tokens, pools, and more.
3. Verify functionality through transactions and interactions.

## Conclusion

With these steps, you can create a DeFi Kingdom clone on Avalanche that allows players to collect, build, and earn rewards for their participation in the game's activities.

## Tools Used

- Unix-based Computer (MacOS or Linux)
- Solidity
- Remix IDE
- Metamask
- Web Browser

Feel free to explore the code and customize it to suit your game's requirements. Enjoy building your DeFi Kingdom clone!
