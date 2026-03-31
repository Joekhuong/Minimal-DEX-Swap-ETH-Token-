# Minimal-DEX-Swap-ETH-Token-
Minimal DEX (Swap ETH ↔ Token)
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

contract MiniDEX {
    IERC20 public token;
    uint256 public rate = 1000; // 1 ETH = 1000 tokens

    constructor(address _token) {
        token = IERC20(_token);
    }

    function buy() public payable {
        uint256 amount = msg.value * rate;
        token.transfer(msg.sender, amount);
    }

    function sell(uint256 amount) public {
        uint256 ethAmount = amount / rate;
        token.transferFrom(msg.sender, address(this), amount);
        payable(msg.sender).transfer(ethAmount);
    }

    receive() external payable {}
}
