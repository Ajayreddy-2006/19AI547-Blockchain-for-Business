### Experiment 1: Decentralized Certificate Verification
## Aim:
  To develop a smart contract for issuing and verifying academic certificates on Ethereum, preventing forgery and ensuring authenticity.
## Algorithm:
1. Deploy a smart contract where universities can issue certificates.
2. Store a hash of certificate data on-chain.
3. Provide a verification function that checks certificate authenticity.
4. Users can verify the certificate by comparing the stored hash.
## Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract certificateVerification {
    address public university;
    mapping(bytes32=>bool) public certificates;

    event CertificateIssued(bytes32 certHash);
    constructor() {
        university = msg.sender;
    }

    function issueCertificate(string memory studentName, string memory degree, uint year)public {
        require(msg.sender == university, "Only university can issue certificates");

        bytes32 certHash = keccak256(abi.encodePacked(studentName, degree, year));
        certificates[certHash] = true;
        emit CertificateIssued(certHash);
    }

    function verifyCertificate(string memory studentName, string memory degree, uint256 year) public view returns (bool) {
        bytes32 certHash = keccak256(abi.encodePacked(studentName, degree, year));
        return certificates[certHash];
    }
}
```
# Expected Output:
```
● When the university issues a certificate, it gets stored as a hash.
● A student or employer can verify the certificate by entering the details.
● If valid, it returns true; otherwise, false.
High-Level Overview:
● Used to prevent fake certificates.
● Enables quick verification by employers or other institutions.
● Shows how blockchain can be used in education and credential verification.
```
# Result:
when certificate is verified

![WhatsApp Image 2025-04-16 at 09 19 09_d55ca997](https://github.com/user-attachments/assets/a44195c4-1083-4ee8-8d67-9291f6472c6b)

when certificate is not verified

![WhatsApp Image 2025-04-16 at 09 19 09_a34145b6](https://github.com/user-attachments/assets/75a55264-2276-40c3-b42c-aaefb205d38c)


