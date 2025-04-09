---
description: 'To get started following the steps below:'
---

# Quickstart

#### Installation

```
npm install path/to/mocanetwork-airkit-0.5.0.tgz
```

#### Initialize & Login

```typescript
import { AirService, EMBED_BUILD_ENV } from "@mocanetwork/airkit";

const service = new AirService({
 partnerId: YOUR_PARTNER_ID,
});
await service.init();
await service.login(options: { authToken });
```

The `AirService` creates an iframe that loads the login flow and sets up communication streams between the iframe and your DApp's javascript context.
