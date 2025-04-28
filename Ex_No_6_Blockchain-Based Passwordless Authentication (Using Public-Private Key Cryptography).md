# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
Step 1: User Registration
A user registers with their Ethereum public key (instead of a password).


Step 2: Login Process
When logging in, the user signs a random challenge message using their private key.


The smart contract verifies the signature using the user’s public key.



# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuth {
    mapping(address => bool) public registeredUsers;

    event UserRegistered(address user);
    event UserAuthenticated(address user);

    function registerUser() public {
        require(!registeredUsers[msg.sender], "Already registered");
        registeredUsers[msg.sender] = true;
        emit UserRegistered(msg.sender);
    }

    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(registeredUsers[msg.sender], "User not registered");
        address signer = ecrecover(hash, v, r, s);
        return signer == msg.sender;
    }
}
```

# Expected Output:
Users can register without a password.
![image](https://github.com/user-attachments/assets/85c00289-3857-49a6-ad53-c673935790f2)


Users sign a challenge with their private key for authentication.
![image](https://github.com/user-attachments/assets/5c601ee1-b7a9-408c-a0aa-08b4c2dcac4a)


The smart contract verifies signatures to confirm identity.
![image](https://github.com/user-attachments/assets/35047963-b851-493e-85fb-84f09a529d3c)

![image](https://github.com/user-attachments/assets/5137e293-f72a-4793-9531-a6edc71f026e)




# High-Level Overview:
Eliminates password hacks & phishing attacks.


Uses Ethereum's built-in cryptographic functions.


Inspired by Web3 login solutions like MetaMask authentication.

# RESULT: 

Hence we implemented code for Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
