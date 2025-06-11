# Wagmi Connector

**Wagmi** is a collection of React Hooks containing everything you need to start working with Ethereum. `@mocanetwork/airkit-connector` is a connector for the popular [wagmi](https://github.com/tmm/wagmi) library to help you integrate and interact with the `AirService`.

This package can be used to initialize a [wagmi client](https://wagmi.sh/) that will seamlessly manage the interaction (wallet connection state and configuration, such as auto-connection, connectors, and ethers providers) of your dApp.

### Initialization

```typescript
import { http, createConfig, CreateConnectorFn } from "wagmi";
import { mainnet, sepolia } from "wagmi/chains";
import { airConnector, AirConnector, AirConnectorProperties } from "@mocanetwork/airkit-connector";

const wagmiConfig = createConfig({
  chains: [mainnet, sepolia],
  transports: {
    [mainnet.id]: http(),
    [sepolia.id]: http(),
  },
  connectors: [
    airConnector({
      partnerId: {{YOUR_PARTNER_ID}},
    }) as CreateConnectorFn,
  ],
});
```

Inside your (root) React component you can add the wagmi provider to the component tree.

```typescript
function App() {
  return (
    <WagmiProvider config={wagmiConfig}>
      ...
    </WagmiProvider>
  );
}
```

### Usage

Now you can access the connector in any component and interact with the wagmi connector. The `AirService` itself can also be accessed by casting the `Connector` to a union type with `AirConnectorProperties` or directly to `AirConnector`.

```typescript
const { connect, connectors, error } = useConnect();
const { addresses, connector } = useAccount();

const isAirWalletConnector = (connector as Connector & AirConnectorProperties)?.isMocaNetwork;
const airConnector = useMemo<AirConnector | null>(() => {
  if (isAirWalletConnector) {
    return connector;
  }
  return null;
}, [connector, isAirWalletConnector]);

const service = airConnector.airService;
```

{% hint style="warning" %}
Currently we're not supporting following features:

* Add Chain
* Switch Chain
* Sign Typed Message
{% endhint %}
