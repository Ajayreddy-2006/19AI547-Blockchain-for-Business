# Experiment 4: DeFi Lending and Borrowing Protocol


# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:

### Step 1: 
Deploy contract and set the owner, interest rate, and liquidation threshold.

### Step 2: 
User deposits funds by calling deposit() and sending Ether to the contract.

### Step 3: 
Check collateral and ensure it meets the required threshold for borrowing.

### Step 4: 
User borrows funds by calling borrow(amount), transferring borrowed funds to the user.

### Step 5: 
Record deposits in the deposits mapping and borrowed amounts in the borrowed mapping.

### Step 6: 
Accumulate interest (future step, not implemented in current contract).

### Step 7: 
Check liquidation if the user’s collateral is below 150% of the borrowed amount.

### Step 8: 
Liquidate borrower by calling liquidate() and transferring seized collateral to the caller.



Program:
```

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {
    address public owner;
    uint256 public interestRate = 5; // 5% interest per cycle
    uint256 public liquidationThreshold = 150; // 150% collateralization
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Deposited(address indexed user, uint256 amount);
    event Borrowed(address indexed user, uint256 amount, uint256 collateral);
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 collateralSeized);

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Deposit must be greater than zero");
        deposits[msg.sender] += msg.value;
        emit Deposited(msg.sender, msg.value);
    }

    function borrow(uint256 amount) public payable {
        require(msg.value >= (amount * liquidationThreshold) / 100, "Nota enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
    }
    function reduceCollateral(address user, uint256 amount) public {
    require(msg.sender == owner, "Only owner can reduce");
    require(collateral[user] >= amount, "Not enough collateral to reduce");
    collateral[user] -= amount;
    }

    function liquidate(address borrower) public {
        require(collateral[borrower] < (borrowed[borrower] * liquidationThreshold) / 100, "Not eligible for liquidation");
        uint256 debt = borrowed[borrower];
        uint256 seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;
        payable(msg.sender).transfer(seizedCollateral);
        emit Liquidated(borrower, debt, seizedCollateral);
    }
}


```
# Expected Output:

![image](https://github.com/user-attachments/assets/af2be60f-07e9-4cfd-b421-b245626fbd02)
![image](https://github.com/user-attachments/assets/086ed55e-301b-4d94-8a51-6dbdbef1fffc)
![image](https://github.com/user-attachments/assets/e5d04fc8-b7b9-40a0-b2aa-afc5e296cc9d)
![image](https://github.com/user-attachments/assets/bd6b8089-2d5d-4735-b109-f222a93398e4)



# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# RESULT : 
Thus, a DeFi Lending and Borrowing Protocol has been successfully built and implenmented on Remix - Ethereum IDE

