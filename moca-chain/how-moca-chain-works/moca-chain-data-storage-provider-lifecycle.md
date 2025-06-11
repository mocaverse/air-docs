# Moca Chain Data Storage Provider Lifecycle

## **What is Moca Chain Data Storage Provider?**

Moca Chain Data Storage Provider(MDSP) is an infrastructure provider for storage services. They work in synergy with Moca Chain validators to provide a complete storage service. Validators store metadata and financial ledgers with consensus, while MDSPs store the actual data (payload data) of objects using the Moca Chain as the ledger and a single source of truth. MDSPs provide a range of convenient services for users and DApps to manage data on Moca Chain.

## **How do the Moca Chain Data Storage Providers work?**

MDSPs need to register themselves first by depositing on the Moca Chain as their `Service Stake`. The Moca Chain validators will then conduct a governance procedure to vote to elect the MDSPs. When joining and leaving the network, MDSPs must follow specific actions to ensure data redundancy for users, or they will face fines for their `Service Stake`.&#x20;

MDSPs provide publicly accessible APIs that allow users to upload, download, and manage data. These APIs are designed to be similar to Amazon S3 APIs, making it easier for existing developers to write code for them. MDSPs are responsible for responding to user requests to write (upload) and read (download) data, as well as managing user permissions and authentications. Each MDSP maintains its local full node, allowing for a strong connection with the Moca Chain network. This enables the MDSP to directly monitor state changes, properly index data, send transaction requests on time, and manage local data accurately. To encourage MDSPs to showcase their capabilities and provide a professional storage system with high-quality SLA, it is recommended that they advertise their information and prove to the community.





