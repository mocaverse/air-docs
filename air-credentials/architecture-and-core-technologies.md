# Architecture & Core Technologies

## How does it work?

### **Block 1: Presentation Layer**

<figure><img src="../.gitbook/assets/Screenshot 2025-06-03 at 9.23.37 PM.png" alt=""><figcaption><p>Presentation Block </p></figcaption></figure>

* **AIR Account:** The standardized account management system within the AIR Credential ecosystem, providing users with identity authentication and data management functionality.
* **Issuer Dashboard**： Provides an interface for issuers to manage credential schemas, define credential fields, and view issuance records.
* **Verifier Dashboard**： Provides an interface for verifiers to configure verification conditions, manage verification schemes, and view verification results.
* **Issuance Widget SDK**： A development toolkit for quickly integrating credential issuance capabilities into applications, enabling seamless interaction between issuers and users during the issuance process.
* **Verification Widget SDK**： A development toolkit for quickly integrating credential verification capabilities into applications, enabling seamless interaction between verifiers and users during the verification process.

### **Block 2: Application Layer**

<figure><img src="../.gitbook/assets/Screenshot 2025-06-03 at 9.24.23 PM.png" alt=""><figcaption><p>Application Block </p></figcaption></figure>

* **Holder Credential Management**： Manages users' credentials, including credential presentation, status changes, and on-chain records.
* **Issuer Application**： Handles specific operations for issuing credentials.
* **Verifier Application**： Handles specific operations for verifying credentials.

### **Block 3: Storage and Database Layer**

<figure><img src="../.gitbook/assets/Screenshot 2025-06-03 at 9.44.57 PM.png" alt=""><figcaption><p>Storage and Database Block </p></figcaption></figure>

* **Decentralized Storage**： Decentralized storage for users' encrypted data, credentials, and threshold keys.
* **Local or Centralized Database**： Stores non-sensitive data such as schemas and queries.

### **Block 4: Blockchain Layer**

<figure><img src="../.gitbook/assets/Screenshot 2025-06-03 at 9.25.14 PM.png" alt=""><figcaption><p>Blockchain Block </p></figcaption></figure>

* **On-Chain Smart Contract**： Records credential statuses, on-chain verifications, and gas fee settlements.
