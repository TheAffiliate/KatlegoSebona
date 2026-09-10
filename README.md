📜 Smart Contracts Overview

The system consists of three Solidity smart contracts.

1. WeThinkCodeStudents.sol

Purpose:
Manages and stores academic marks for students enrolled at WE Think Code.

Key Features
Uses unique Ethereum addresses as student identifiers.
Allows multiple students to have the same name without causing conflicts.
Provides admin-controlled mark assignment through setMarks().
Provides a public getMarks() function for retrieving a student's marks.
Tracks whether a student belongs to:
WE Think Code
Africa's Blockchain Club
Both organizations
Example
setMarks(studentAddress, 85);

The student's Ethereum address is used as the unique identifier rather than their name.

2. ABCStudentRanks.sol

Purpose:
Manages student progression ranks within Africa's Blockchain Club (ABC).

Key Features
Uses a Solidity enum to define student ranks.
Supports three progression levels:
Junior
Intermediate
Senior
Allows only the administrator to assign ranks using setRank().
Provides a public getRank() function to retrieve a student's current rank.
Rank Structure
enum Rank {
    Junior,
    Intermediate,
    Senior
}
3. SchoolManager.sol

Purpose:
Acts as the master coordinator for the WE Think Code and ABC student management contracts.

The contract demonstrates the Composition Pattern by interacting with separately deployed contracts rather than using multiple inheritance.

Key Features
Stores references to the deployed student-management contracts.
Calls functions on WeThinkCodeStudents.
Calls functions on ABCStudentRanks.
Allows marks and ranks to be managed through a central contract.
Demonstrates how multiple independent smart contracts can work together.
Why Composition?

Composition is used instead of multiple inheritance to avoid potential Solidity declaration and function collision issues.

Instead of:

contract SchoolManager is WeThinkCodeStudents, ABCStudentRanks {
    // ...
}

The SchoolManager contract instead works with deployed contract instances:

WeThinkCodeStudents public weThinkCodeStudents;
ABCStudentRanks public abcStudentRanks;

This provides a cleaner separation of responsibilities between contracts.

🔐 Access Control

The contracts use an onlyAdmin modifier to restrict sensitive operations.

modifier onlyAdmin() {
    require(
        msg.sender == admin,
        "Only admin can perform this action"
    );
    _;
}
Admin Privileges

The account that deploys the contract is automatically assigned as the administrator.

constructor() {
    admin = msg.sender;
}

The administrator is responsible for:

Assigning student marks.
Updating student ranks.
Managing authorized student information.
Security

Non-admin accounts cannot modify academic marks or student ranks.

This prevents unauthorized users from tampering with student performance data stored on the blockchain.

🏗️ Architecture

The system follows a modular smart contract architecture:

                    ┌─────────────────────────┐
                    │     SchoolManager        │
                    │                         │
                    │  Master Coordination     │
                    └────────────┬────────────┘
                                 │
                  ┌──────────────┴──────────────┐
                  │                             │
                  ▼                             ▼
       ┌─────────────────────┐       ┌─────────────────────┐
       │ WeThinkCodeStudents │       │   ABCStudentRanks  │
       │                     │       │                     │
       │ Student Marks       │       │ Student Ranks       │
       │ Enrollment Status   │       │ Junior              │
       │                     │       │ Intermediate        │
       │                     │       │ Senior              │
       └─────────────────────┘       └─────────────────────┘

Each contract has a specific responsibility while SchoolManager coordinates interactions between them.

🚀 Getting Started with Remix IDE
Step 1: Open Remix

Open the Remix IDE:

https://remix.ethereum.org/
Step 2: Create the Project Structure

Create or navigate to the following directory:

Assignment 1/

Inside it, create the Contracts/ directory:

Assignment 1/
└── Contracts/

Add the following Solidity files:

Contracts/
├── WeThinkCodeStudents.sol
├── ABCStudentRanks.sol
└── SchoolManager.sol
Step 3: Compile the Contracts

Open the Solidity Compiler in Remix.

Select a Solidity compiler version compatible with:

pragma solidity ^0.8.0;

Then compile:

WeThinkCodeStudents.sol
ABCStudentRanks.sol
SchoolManager.sol

Make sure there are no compilation errors before proceeding.

Step 4: Deploy WeThinkCodeStudents

Navigate to Deploy & Run Transactions.

Select:

WeThinkCodeStudents

Then click:

Deploy

Copy the deployed contract address.

Step 5: Deploy ABCStudentRanks

Select:

ABCStudentRanks

Click:

Deploy

Copy the deployed contract address.

Step 6: Deploy SchoolManager

Select:

SchoolManager

The constructor requires the addresses of the previously deployed contracts.

Enter:

WeThinkCodeStudents Contract Address
ABCStudentRanks Contract Address

Then click:

Deploy

The SchoolManager contract can now communicate with both contracts.

🧪 Testing the Contracts

After deployment, the contracts can be tested directly through the Remix interface.

Testing Student Marks

Using the administrator account, call:

setMarks(studentAddress, marks)

For example:

setMarks(
    0x1234567890123456789012345678901234567890,
    85
)

Then retrieve the student's marks:

getMarks(studentAddress)

Expected result:

85
Testing Student Ranks

Use:

setRank(studentAddress, rank)

The rank values correspond to the enum:

0 = Junior
1 = Intermediate
2 = Senior

For example:

setRank(studentAddress, 2)

This assigns:

Senior

The student's rank can then be retrieved using:

getRank(studentAddress)
👤 Student Identification

Students are identified using their Ethereum wallet addresses.

For example:

0x1234567890123456789012345678901234567890

This approach provides a unique identifier for every student.

It also prevents problems where two different students may have the same name.

Example

Two students can both be named:

John Smith

But they can still be uniquely identified using their wallet addresses:

John Smith → 0xABC...
John Smith → 0xDEF...

Therefore, their academic records remain separate.

🔗 Composition Pattern

The project demonstrates the Composition Pattern in Solidity.

Instead of combining multiple contracts through inheritance, SchoolManager maintains references to separately deployed contracts.

Benefits
Better separation of responsibilities.
Reduced inheritance complexity.
Easier contract maintenance.
Reduced risk of function-name collisions.
Individual contracts can be developed and tested independently.
Demonstrates interaction between deployed smart contracts.
🛡️ Security Considerations

The system implements basic access control through the onlyAdmin modifier.

Protected Operations

The following operations are restricted to the administrator:

Assigning student marks
Updating student ranks

Unauthorized users attempting to perform these operations will receive:

Only admin can perform this action

Student records can still be read through the public getter functions.

📚 Technologies Used
Technology	Purpose
Solidity	Smart contract development
Ethereum	Blockchain platform
Remix IDE	Smart contract compilation and deployment
Ethereum Addresses	Unique student identification
Enums	Student rank management
Composition Pattern	Multi-contract architecture
Git / GitHub	Version control and repository management
🎯 Learning Objectives

This assignment demonstrates the following Solidity and blockchain concepts:

Smart contract development.
Solidity state variables.
Solidity mappings.
Solidity enums.
Function visibility.
Constructors.
Modifiers.
Access control.
Ethereum wallet addresses.
Contract-to-contract interaction.
Smart contract deployment.
Composition over inheritance.
Basic blockchain data management.
Using Remix IDE for development and testing.
📌 Project Summary

The Student Management Smart Contract System provides a modular blockchain-based solution for managing student academic information across WE Think Code and Africa's Blockchain Club.

The system separates responsibilities into three contracts:

WeThinkCodeStudents.sol
        ↓
Student Marks & Enrollment

ABCStudentRanks.sol
        ↓
Student Progression Ranks

SchoolManager.sol
        ↓
Coordinates Both Contracts

The use of Ethereum addresses provides unique student identification, while administrator-only functions protect academic data from unauthorized modification.

The project demonstrates how Solidity composition can be used to build modular and maintainable decentralized applications.

👨‍💻 Author

Developer: Katlego Sebona

Organizations:

WE Think Code
Africa's Blockchain Club (ABC)
📄 License

This project is licensed under the MIT License.

See the LICENSE file for more information.

⭐ Acknowledgements

Special thanks to:

WE Think Code
Africa's Blockchain Club (ABC)
Ethereum Developer Community
Remix IDE

for providing the learning environment and resources used to develop this project.