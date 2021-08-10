Brief introduction to the Colgate-Kolibree Web SDK
==============================

The Colgate-Kolibree Web SDK is a collection of NPM packages at every abstraction level for supporting the development of web-based *cross-platform* brushing experiences with Colgate smart toothbrushes, among which there are low-level data constructs, utilities, abstract APIs, 3D graphics support, bluetooth, websocket client and more.

## Rationale

Broadly speaking, the SDK can be roughly categorized into four groups:

### 1. Core abstractions and utilities
Consists of common API interfaces like the Bluetooth generic API and the implementations on target platforms, as well as core utilities like cross-platform `local-storage` and `async-task-queue`, etc.

### 2. Services
Service packages define the details built on top of the abstract APIs as the business logic layer upon which app modules are built. They are on a higher level of abstraction and hide most of the implementation detail. Some of the most relevant packages are:

- `@kolibree/api-client`
- `@kolibree/toothbrush-client`
- `@kolibree/websocket-client`
- `@kolibree/jaw`
- ...

### 3. UX experiences
UX packages are precompiled code of brushing activities, like Guided Brushing, Test Brushing, etc. They are designed to be self-contained and delivered as UI components which can be embedded directly in a page in the same UI technology.

These packages are transpiled and generated from source Web Components by our proprietary transpiler. In other words, they're immediately available on the standard web (vanilla, React, Angular, Vue etc.) but need transpiler support to run on specific platforms, like Wechat Miniprogram and Tmall miniapps.

### 4. Developmental tools
Transpilers, custom bundlers and app builders. They are mostly used by the SDK developers to bundle and build apps.

## Sample usage

So let's look at how the SDK can be leveraged to build a hypothetical brushing activity as a typical scenario of a brushing app:

A brushing activity starts from connecting to a peripheral bluetooth device (smart toothbrush), so the first thing to do is to establish a connection:

``` javascript
// app.js
import { scanForPeripherals } from '@kolibree/ble-my-mini';
import { ToothbrushClient } from '@kolibree/toothbrush-client';

// `ble-my-mini` is a package of BLE api implementations on Tmall miniapps (not available yet)
// toothbrush client is a service for working with a connected toothbrush

// Scan for peripheral devices and subscribe to the scan result data stream
scanForPeripherals({ /* filters */ }).subscribe((advertisement) => {
  const { device } = advertisement;
  const tbClient = new ToothbrushClient(device);
  tbClient.connect().then(() => {
    // logic with a successful toothbrush connection
  });
});

// The reason why we don't use platform-specific bluetooth APIs is that 
// the other high-level packages like the UX packages are using generic
// APIs so that they can be reused on any platform as long as there are
// adapters/implementations available in the dependency graph.
```

Once the connection is established, the app can direct the user to the activity page which can be as simple as:

``` HTML
<!-- guided-brushing is provided by the SDK -->
<guided-brushing tbId="<deviceId>" tbModel="<deviceModel>" />
```

## TL; DR;

The Kolibree web sdk is a collection of essential NPM dependencies upon which a brushing app can be built to provide rich brushing activities with Colgate smart toothbrushes. All packages are available as NPM packages hosted on our private registry and they are standard-compliant with little idiosyncrasies.
