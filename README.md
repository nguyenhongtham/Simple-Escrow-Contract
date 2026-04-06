# Simple-Escrow-Contract
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Escrow {
    address public buyer;
    address public seller;
    uint256 public amount;
    bool public buyerApproved;
    bool public sellerApproved;

    constructor(address _seller) payable {
        buyer = msg.sender;
        seller = _seller;
        amount = msg.value;
    }

    function approve() public {
        if (msg.sender == buyer) buyerApproved = true;
        if (msg.sender == seller) sellerApproved = true;
    }

    function release() public {
        require(buyerApproved && sellerApproved, "Both must approve");
        payable(seller).transfer(amount);
    }

    function refund() public {
        require(msg.sender == buyer, "Only buyer");
        payable(buyer).transfer(amount);
    }
}
