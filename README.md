# Fee-Collector-with-Custom-Fees
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract FeeCollector {
    address public treasury;
    uint256 public platformFee = 200; // 2%

    mapping(address => bool) public isExempt;

    event FeeCollected(address from, uint256 amount);

    constructor(address _treasury) {
        treasury = _treasury;
    }

    function collectFee() public payable {
        uint256 fee = (msg.value * platformFee) / 10000;
        uint256 netAmount = msg.value - fee;

        payable(treasury).transfer(fee);
        payable(msg.sender).transfer(netAmount); // or keep for contract use

        emit FeeCollected(msg.sender, fee);
    }

    function setExempt(address account, bool exempt) public {
        require(msg.sender == treasury, "Only treasury");
        isExempt[account] = exempt;
    }
}
