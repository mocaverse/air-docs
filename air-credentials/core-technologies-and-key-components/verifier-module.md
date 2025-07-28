# Verifier Module

The **Verifier module in the SDK** is designed to simplify the integration of credential verification functionality. It enables Verifiers to perform condition checks easily, generate Zero-Knowledge Proofs (ZKP), and display verification results.

## Features of the Verifier SDK

**Verifiable Credential Decryption:**

* The system decrypts the user’s Verifiable Credential and verifies whether it meets the predefined conditions of the verification program (e.g., age, nationality, etc.).

**Privacy-Preserving Verification:**

* Using Zero-Knowledge Proof (ZKP) technology, the Verifier SDK enables high-security identity verification without exposing the user’s original data, thereby safeguarding user privacy.

**On-Chain Verification Records:**

* The verification process is executed on-chain, with all results recorded and stored via smart contracts. This ensures immutability and full traceability of the data.

## Interaction Workflow&#x20;

### **Step 1: Credential Request**

* The user initiates the credential verification process on the Verifier’s application page.
* The user logs into their account to start the verification workflow.
* The Verifier SDK sends a backend request to retrieve the details of the verification program based on the Program ID. The SDK uses the user’s wallet address and the credential schema configured in the program to fetch the user’s encrypted credential from **the Decentralized Storage**.
* The user consents to decrypt the credential and authorizes the threshold key to the Verifier in compliance with regulatory requirements:
  * **Step 1**: The SDK uses the user’s private key to sign and retrieve the plaintext threshold key.
  * **Step 2**: The SDK then signs the threshold key with the Verifier’s public key to ensure that only the designated Verifier can decrypt and access the threshold key.
  * **Step 3**: The SDK encrypts the threshold key and stores it in the decentralized storage. This encrypted key is later used by the Verifier to complete the decryption process.

### **Step 2: Credential Decryption and ZKP Generation**

Credential Decryption：

* The Verifier module uses the user’s private key to decrypt the encrypted verifiable credential and obtain the plaintext credential information.
* The SDK validates the credential’s authenticity and checks its validity period.

ZKP Generation：

* Based on the **Queries** defined in the verification program and the user’s **Verifiable Credential**, the SDK generates a Zero-Knowledge Proof (ZKP) specific to this verification process.

### **Step 3: On-Chain Verification**

* **On-chain Verification:** If the verification program is configured for on-chain verification, the SDK submits a verification request to the smart contract. The contract validates the ZKP and returns the verification result.
* **Asset Chain Verification**: If the verification program is configured for an asset chain, the SDK initiates a verification request to the base Chain. After the base Chain smart contract processes the ZKP and returns the verification result, the SDK proceeds with subsequent steps based on this result. Additionally, the base Chain contract synchronizes the verification result to the asset chain using a cross-chain solution.

### **Step 4: Completion of Verification**

After the verification process is finalized, the user’s identity is authenticated through a secure and tamper-proof workflow. The user receives a verification result, which can be used to seamlessly access and utilize Web3 services, enabling users to unlock a new digital identity experience.

