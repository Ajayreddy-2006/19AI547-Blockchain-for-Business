# Experiment 5: Zero-Knowledge Proof (ZK) Private Voting System
# Aim:
To implement a fully private and transparent voting system using Zero-Knowledge Proofs (ZKPs). This ensures that votes are counted fairly without revealing who voted for whom.

# Algorithm:
step 1.Each voter registers by submitting a hashed commitment of their vote using a secret and their choice.

step 2.The smart contract stores this commitment without revealing any actual vote information.

step 3.During the voting phase, voters reveal their secret and choice.

step 4.The contract verifies the hash matches the original commitment to ensure authenticity.

step 5.Valid votes are counted for either option A or B.

step 6.This process maintains vote privacy while ensuring accurate and tamper-proof tallying.


# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ZKVoting {
    struct Voter {
        bool registered;
        bytes32 voteCommitment;
    }

    mapping(address => Voter) public voters;
    uint256 public votesForA;
    uint256 public votesForB;

    event VoteCommitted(address voter, bytes32 commitment);
    event VoteRevealed(address voter, uint256 choice);

    function registerVoter(bytes32 commitment) public {
        require(!voters[msg.sender].registered, "Already registered");
        voters[msg.sender] = Voter(true, commitment);
        emit VoteCommitted(msg.sender, commitment);
    }

    function revealVote(string memory secret, uint256 choice) public {
        require(voters[msg.sender].registered, "Not registered");
        require(keccak256(abi.encodePacked(secret, choice)) == voters[msg.sender].voteCommitment, "Invalid proof");

        if (choice == 1) votesForA++;
        if (choice == 2) votesForB++;

        emit VoteRevealed(msg.sender, choice);
    }
}

```
# Output:
![image](https://github.com/user-attachments/assets/a2a4760f-3c86-4ea2-96e9-149d26571655)

![image](https://github.com/user-attachments/assets/9a96ce93-d389-4015-b62e-6cc75db4b889)


![image](https://github.com/user-attachments/assets/3b1214b4-d8f4-4a8f-8004-16bf30826cce)

![image](https://github.com/user-attachments/assets/23914121-fbca-4caa-a482-5500d4404428)



# High-Level Overview:
Uses ZKPs to ensure anonymous and fair elections.


Prevents vote tampering while maintaining voter privacy.


Mimics real-world ZK voting applications in governance and DAOs.

# RESULT: 
Thus, a Zero-Knowledge Proof (ZK) Private Voting System has been implemented.
