# Experiment 4: DeFi Lending and Borrowing Protocol
# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:
Step 1: Setup Lending and Borrowing Mechanism
Users deposit ETH into the contract as liquidity.


Depositors receive interest based on their deposits.


Borrowers can borrow ETH but must provide collateral (e.g., 150% of the borrowed amount).


Interest on borrowed funds is calculated dynamically based on utilization rate.


Step 2: Implement Overcollateralization
If a borrower’s collateral value drops below a certain liquidation threshold, their collateral is liquidated to repay the debt.


Step 3: Allow Liquidation
If collateral < liquidation threshold, liquidators can repay the borrower's debt and claim their collateral at a discount.



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
Users can deposit ETH and earn interest.


Users can borrow ETH by providing collateral.


If collateral < 150% of borrowed amount, liquidators can seize the collateral.



# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# RESULT :

DEPLOYING THE CONTRACT

![image](https://github.com/user-attachments/assets/f03e164f-5126-41a0-8dea-a13b6cc5da91)
![image](https://github.com/user-attachments/assets/8bf2fa83-3f05-4ec1-8ce7-894d201aa920)
![image](https://github.com/user-attachments/assets/c0901aa4-3091-46c5-b37b-e4f4c0ce6ce6)

DEPOSIT 

![image](https://github.com/user-attachments/assets/ee7e985b-00c0-44be-8806-c58bdd392769)
![image](https://github.com/user-attachments/assets/2f8d2e05-7f98-45a3-bfc6-c1977af0ee3f)

BORROWING

![image](https://github.com/user-attachments/assets/418371c5-90f9-4b90-bb26-614e062a3e6e)
![image](https://github.com/user-attachments/assets/03ad7dcd-26ad-4a30-973c-666f6428566b)

BORROWED

![image](https://github.com/user-attachments/assets/78667e71-4b03-4ad7-8b32-842a21a35704)
![image](https://github.com/user-attachments/assets/6b59ab79-1f38-4b88-bdb2-75f3601f4e93)

COLLATERAL

![image](https://github.com/user-attachments/assets/38c0808d-b6f2-4972-8ff9-398d56866ba8)
![image](https://github.com/user-attachments/assets/5a81ce87-7534-426d-9be9-98e6a5f2d458)

DEPOSITS

![image](https://github.com/user-attachments/assets/fc180c41-2bbd-4ba9-9fa6-35f9cd51765c)

INTEREST RATE

![image](https://github.com/user-attachments/assets/27a12627-83b7-487d-b468-158c7c71d80a)

LIQUIDATION THRESHOLD

![image](https://github.com/user-attachments/assets/79f71dd1-d10c-478a-a88b-2c83485ff115)

CALL TO OWNER

![image](https://github.com/user-attachments/assets/6cbf09a2-807b-4815-bf7b-3f23eb23aa71)

![image](https://github.com/user-attachments/assets/059ed4a0-ea64-4e22-aabc-2ed2d9243e9f)

![image](https://github.com/user-attachments/assets/a5ca556a-892b-4a64-beb2-cbd68f6b5ca0)

# THUS THE DECENTRALIZED LENDING PROTOCOL HAS BEEN CREATED AND OUTPUTS HAVE BEEN VERIFIED
