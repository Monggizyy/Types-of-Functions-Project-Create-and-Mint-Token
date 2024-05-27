# Types-of-Functions-Project-Create-and-Mint-Token

# Smart Contract
This Assesment code implements ERC20 token functionality with mint, burn, and transfer capabilities

# Description
This ERC20 token contract offers a unique blend of minting, burning, and transfer functionalities, allowing the contract owner to create new tokens (with an optional maximum supply to control inflation), potentially burn an equivalent amount from their own balance during the minting process, and facilitate standard transfers between token holders, all while leveraging the security and ownership management features provided by OpenZeppelin's ERC20 and Ownable libraries.

# Getting Started 
So to run this go to https://remix.ethereum.org/ 
Once you're in the website click the start coding and put the provided code here. Then go to Solidity Compiler and run it. And now go to Deploy and Run Transaction to try the provided code.

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.8.0/contracts/token/ERC20/ERC20.sol";
    import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.8.0/contracts/access/Ownable.sol";
    
    
    contract TheERC20 is ERC20, Ownable {
    
    uint256 public maxTotalSupply; 

    
    constructor(
        string memory name,
        string memory symbol,
        uint256 initialSupply,
        uint256 _maxTotalSupply
    ) ERC20(name, symbol) {
        _mint(msg.sender, initialSupply); 
        if (_maxTotalSupply > 0) {
            maxTotalSupply = _maxTotalSupply;
        }
    }

   
    function mintWithPotentialBurn(address recipient, uint256 amount, bool burnFromSender) external onlyOwner {
        require(totalSupply() + amount <= maxTotalSupply, "Minting would exceed maximum supply");

        if (burnFromSender && balanceOf(msg.sender) >= amount) {
            _burn(msg.sender, amount); 
        }
        _mint(recipient, amount); 
    }
    
    function burn(address account, uint256 amount) external onlyOwner {
        require(balanceOf(account) >= amount, "Insufficient balance for burning");
        _burn(account, amount);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
    }

# Author
Dela Cruz, Dex Zyrius B. (3.1 BSIT)
