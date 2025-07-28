---
noIndex: true
---

# Release Notes

## **Version 1.4.0 (June 26, 2025)**

### ✨ **New Features**

* Moca Testnet chain added

### ➕ **Improvements**

* New SANDBOX environment for development and testing



## **Version 1.3.0 (June 5, 2025)**

### ➕ **Improvements**

* Captcha required during OTP for enhanced security
* Signing screen shows actual message
* Improved security by moving signing and transaction screens into separate window
* Improved Passkey provider suggestions
* Wagmi connect method exposes authToken parameter
* Access token contains `sourcePartnerId` in case of cross partner rehydration
* Many small stabilization improvements

### **⚠️ Notes**

* Signing and transaction screens are now shown in a separate browser window instead of iframe modals



## **Version 1.2.0 (May 7, 2025)**

### ✨ **New Features**

* User login token can be refreshed

### ➕ **Improvements**

* Improved MFA setup and verification flow
* More user-friendly transaction screen
* Bottom sheet instead of modal on mobile screens
* Animations for smoother transitions
* Support of small mobile screens
* Improved session key support
* Improved provider error handling



## **Version 1.1.0 (March 27, 2025)**

### ✨ **New Features**

* Keep user sessions across AIR Kit dApps
* Protect user accounts with MFA via Passkey
* Login via EoA wallet
* Soneium chain support (Testnet / Mainnet)

### ➕ **Improvements**

* Simplified transaction screens
* Improved modal UI
* Wallet can be preloaded in the background
* Provider can be retrieved and subscribed to before wallet initialization
* Paymaster policies can be defined per chain
* Various minor bug fixes and optimizations

### **⚠️ Notes**

* The AA will only be returned after the user has MFA set up
* MFA setup will automatically trigger on any wallet action



## **Version 1.0.0 (Feb 27, 2025)**

### ✨ **New Features**

* Smart accounts (AA) can be checked for deployment
* Smart accounts (AA) can be deployed without minting an Air Id
* Experimental session key support

### ➕ **Improvements**

* Full wallet services support
* Improved token refresh mechanism
* Login rehydration across partners

### **⚠️ Notes**

* The `getUserInfo()` and `getPartnerUserInfo()` methods have been merged into `getUserInfo()`



## **Version 0.6.0 (Feb 17, 2025)**

### ✨ **New Features**

* Beta version of wallet services

### ➕ **Improvements**

* Small UI updates
* More helpful error messages

### **⚠️ Notes**

* The wallet initialized event also returns the Smart Account (AA) address from now on



## **Version 0.5.0 (Feb 11, 2025)**

The first official release of AIR Kit, rebranded from Realm SDK, focuses on drastic performance and UX improvements and sets the foundation for upcoming features.

### ✨ **New Features**

* Customizable language support, including localized emails
* Toggleable email input field
* Support for custom authentication (Bring Your Own Auth)
* Optional partner user linking flow
* Introduction of a Global User ID to unify multiple Air IDs

### ➕ **Improvements**

* Simplified design tokens for UI customization
* Significantly faster initialization time and login experience
* Keep login session alive by automatic token refresh
* Backward compatibility down to ES2020
* Various minor bug fixes and optimizations

### **⚠️ Notes**

* The MPC Signer address is no longer being returned
* External wallet login (e.g. MetaMask) is not yet supported
* Wallet-related services, including EIP1193 provider support, are unavailable in this version
* Smart Account (AA) addresses created in this version are different from previous versions



