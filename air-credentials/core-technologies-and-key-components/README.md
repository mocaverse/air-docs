# Core Technologies and Key Components

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Key Compenents of the SDK</p></figcaption></figure>

## Encryption and Privacy Protection Mechanisms

#### **Zero-Knowledge Proof (ZKP)**

The system leverages Zero-Knowledge Proof technology to enable **"selective disclosure"**, allowing the Holder to prove that they meet specific conditions (e.g., age, identity status) without revealing their complete identity data. The verification process is securely executed on-chain, effectively preventing credential data from being copied or tampered with.

#### **Threshold Keys and Multi-Party Computation (MPC)**

To encrypt the Holder's original identity data, the system adopts **threshold key technology**, where the Holder retains a share of the key. Through **Multi-Party Computation (MPC)**, data is encrypted while ensuring its integrity and confidentiality.

## **Efficient On-Chain Data Structure and Storage**

#### **Claim Tree (Merkle Tree) Structure**

To reduce on-chain storage costs and enhance data verification efficiency, AIR Credential organizes credential data using a **Merkle Tree** structure. This design minimizes data redundancy while enabling fast verification of the consistency and authenticity of credentials through tree-based proofs.

**Decentralized Storage**

All encrypted credential data (including VCs, keys, and original data) is stored in **dStorage**. This ensures data privacy and enables seamless access across devices and applications without relying on centralized servers.
