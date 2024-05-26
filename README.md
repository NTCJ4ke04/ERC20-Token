# ERC20-Token

// SPDX-License-Identifier: MIT
// Compatible with OpenZeppelin Contracts ^5.0.0
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract JakeToken is ERC20 {
    address public owner;

    constructor() ERC20("JakeToken", "JTK") {
        owner = msg.sender;
        _mint(msg.sender, 500000); 
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "You are not the contract owner");
        _;
    }

    function mint(address to, uint256 value) public onlyOwner {
        _mint(to, value);
    }

    function transfer(address to, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), to, amount);
        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) public override returns (bool) {
        _transfer(from, to, amount);
        _approve(from, _msgSender(), allowance(from, _msgSender()) - amount);
        return true;
    }
}
