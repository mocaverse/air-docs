# Issuer Module

The **Issuer module in the SDK** is a development toolkit designed to streamline the integration of credential issuance. Providing standardized interfaces enables issuers to implement credential issuance processes without building complex systems from scratch.

### Features of the Issuer module

**Standardized Credential Generation:**

* Generates Standardized Verifiable Credentials based on predefined schemas and user data, ensuring interoperability and compliance with established standards.

**Standardized Credential Issuance:**

* Facilitates seamless credential issuance processes via the **Issue Node**, enabling efficient on-chain issuance workflows.

**Encrypted Data Storage:**

* Encrypts user data and Verifiable Credentials before securely storing them in decentralized storage, ensuring robust data security and privacy.

### Interaction Workflow&#x20;

**Step 1: Data Collection**

The **Issuer module** supports flexible and secure data collection methods to accommodate various business scenarios. These methods ensure accurate and secure input of user data during the issuance process:

* **When the Issuer Already Possesses User Data:**
  * If the issuer has pre-collected user data, the SDK integrates seamlessly with the issuer’s existing workflows. User data is passed into the SDK as input parameters, which are then used to sequentially generate and issue the credentials.
* **When the Issuer Does Not Possess User Data:**
  * **Custom Data Collection:** Issuers can develop their data collection mechanisms or integrate existing data collection processes to gather user information. The collected data can then be passed as input to the SDK for credential issuance.
  * **Secure Data Retrieval with zkTLS:** Using zkTLS technology, user data can be securely retrieved from designated data sources. Encrypted data returned by zkTLS is directly provided to the SDK as input for credential issuance.

These methods ensure that issuers, whether or not they have pre-existing access to user data, can efficiently and securely complete the data collection process, enabling the issuance of verifiable credentials.\
\
**Step 2: Verifiable Credential Generation**

* The SDK sends a backend service request to retrieve the schema.
* Based on the schema structure, the SDK uses the collected user data as input to generate a Verifiable Credential.

**Step 3: Verifiable Credential Issuance**

* The SDK initiates a request to the **Issue Node** to execute the on-chain credential issuance process.
* The SDK encrypts the user’s credentials and associated raw data.
* The encrypted data is stored in a decentralized storage, while relevant reference information is recorded on the Chain.

#### **Step 4: Completion of Issuance**

* Once the issuance process is complete, the user receives a Verifiable Credential that is both encrypted and tamper-proof. This credential serves as a secure and reliable means of identity verification, empowering users to authenticate themselves in the digital ecosystem. It ensures users can access various digital services seamlessly and securely, while maintaining control over their data.
