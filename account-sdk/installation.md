# Installation

#### Installation

```
npm install @mocanetwork/airkit
```

#### Bundling

This module is distributed in 3 formats

* `esm` build `dist/airkit.esm.js` is es6 format
* `commonjs` build `dist/airkit.cjs.js` in es5 format
* `umd` build `dist/airkit.umd.min.js` In ES5 format without polyfilling corejs minified

By default, the appropriate format is used for your specified use case. A different format can be used (if you know what you're doing) by referencing the correct file.

#### Dynamic Import

If not already, the node buffer and process libraries need to be polyfilled.

{% tabs %}
{% tab title="Webpack" %}
Install [node-polyfill-webpack-plugin](https://www.npmjs.com/package/node-polyfill-webpack-plugin):

```sh
npm install --save-dev node-polyfill-webpack-plugin
```

Update your webpack config as follows

```javascript
const NodePolyfillPlugin = require("node-polyfill-webpack-plugin");

module.exports = {
  webpack: {
    plugins: [
      new NodePolyfillPlugin({
        additionalAliases: [
          "buffer",
          "crypto",
          "assert",
          "http",
          "https",
          "os",
          "url",
          "zlib",
          "stream",
          "_stream_duplex",
          "_stream_passthrough",
          "_stream_readable",
          "_stream_writable",
          "_stream_transform",
          "process",
        ],
      }),
    ],
  },
};
```
{% endtab %}

{% tab title="Vite" %}
Install [vite-plugin-node-polyfills](https://www.npmjs.com/package/vite-plugin-node-polyfills):

```sh
npm install --save-dev vite-plugin-node-polyfills
```

Update the <mark style="color:orange;">`vite.config.ts`</mark> file as follows

```javascript
import react from "@vitejs/plugin-react";
import { defineConfig } from "vite";
import { nodePolyfills } from "vite-plugin-node-polyfills";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    react(),
    nodePolyfills({
      include: [
        "buffer",
        "crypto",
        "assert",
        "http",
        "https",
        "os",
        "url",
        "zlib",
        "stream",
        "_stream_duplex",
        "_stream_passthrough",
        "_stream_readable",
        "_stream_writable",
        "_stream_transform",
      ],
    }),
  ],
});

```
{% endtab %}
{% endtabs %}

Let's get you started with integrating the AIR account \


> Refer to this [GitHub Repo](https://github.com/MocaNetwork/airkit-example/blob/main/src/App.tsx) to understand the functionalities of the AIR Account SDK and setup the project locally.



