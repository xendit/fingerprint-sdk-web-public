# Xendit Fingerprint Web SDK

Web SDK for device identification and fingerprinting with Xendit services.

## Running The Sample App

Install with NPM

```
$ npm install
```

Update `MY_PUBLIC_API_KEY` value in `src/index.html` with a valid key provided
by [Xendit Dashboard](https://dashboard.xendit.co/)

Run the sample app

```
$ npm start
```

The sample app is now running on http://localhost:1234

> If HTTP 401 errors are occurring in the browser JS console, ensure the public API key used is valid.

## Basic Usage

Initialize the SDK with your public API key on application start up and
perform a scan.

> The SDK must be initialized before use.

```js
// Initialize the SDK on every page load
XenditFingerprintSDK.init({
    apiKey: 'MY_PUBLIC_API_KEY',
});
// Run a scan immediately after initialization
XenditFingerprintSDK.scan();
```

The session ID is retrievable from either `XenditFingerprintSDK.init()` on SDK
initialization or from the `XenditFingerprintSDK.getSessionID()` convenience method
any time after initialization.

Both of these functions returns a Session ID of type `string`.

```js
// On SDK init
var sessionID = XenditFingerprintSDK.init('MY_PUBLIC_API_KEY');

// After SDK init
var sessionID = XenditFingerprintSDK.getSessionID();
```

This Session ID can then be passed on to other Xendit APIs that support device
fingerprinting. Please refer to the respective API's documentation for further info.

## Requirements

Supported web browsers:

-   Chrome 49+
-   Firefox 52+
-   Edge 93+
-   Desktop Safari 12.1+
-   Mobile Safari 10.3+
-   Samsung Internet 14.0+
-   Android Browser 4.4+

## Installation

In a browser:

```html
<script src="https://cdn.jsdelivr.net/npm/xendit-fingerprint-sdk-web/dist/xendit-fingerprint-sdk-web.js">
```

Using NPM:

```sh
$ npm install --save xendit-fingerprint-sdk-web
```

## Methods

Asynchronous SDK methods have been labeled `async`, these return a `Promise` object. Refer to the
returns section of each method for the resolved type.

### `init()`

The SDK must be initialized before it can be used.

-   Initialization must be ran on every web page load.
-   The SDK will associate a session ID for the webpage until it's closed.
-   A new session ID will be assigned on every page load.
-   Single page apps (SPAs) will have the same session ID if JS based routing is used.
    Because the web page is only loaded once.

```javascript
const sessionId = XenditFingerprintSDK.init({ apiKey: publicKey });
```

#### **Parameters**

| Name        | Type               | Required | Description            |
| ----------- | ------------------ | -------- | ---------------------- |
| initOptions | InitOptions Object | Yes      | Initialization options |

> Do not use your private API key!

`InitOptions` Object

| Name   | Type   | Required | Description                                                                         |
| ------ | ------ | -------- | ----------------------------------------------------------------------------------- |
| apiKey | string | Yes      | A valid public API key provided by [Xendit Dashboard](https://dashboard.xendit.co/) |

#### **Returns**

| Name      | Type   | Description          |
| --------- | ------ | -------------------- |
| sessionId | string | Generated session ID |

### `async scan()`

Scans the web browser and sends the device fingerprint data to Xendit.

-   Scan should be called immediately after initializing the SDK.
-   Scan can be called multiple times within a session.
-   Do not `await` the scan, let it run in the background. This avoids blocking
    any foreground application code execution.

```javascript
XenditFingerprintSDK.scan();
// Or
XenditFingerprintSDK.scan(customerEventName, customerEventID);
```

#### **Parameters**

| Name              | Type   | Required | Description                                                                                                         |
| ----------------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------------- |
| customerEventName | string | No       | Optional event name to associate with this scan. Recommended to use snake case formatting. e.g. `'some_event_name'` |
| customerEventID   | string | No       | Optional identifier associated with the event. e.g. user account ID                                                 |

### `getSessionID()`

Convenience method to retrieve Session ID after SDK initialization.

```javascript
const sessionId = XenditFingerprintSDK.getSessionID();
```

#### **Returns**

| Name      | Type   | Description          |
| --------- | ------ | -------------------- |
| sessionID | string | Generated session ID |

### `setEnabled()`

Enables or disables the SDK.

-   When disabled, the scan method will do nothing. No data will be sent to Xendit.
-   When enabled, the SDK will resume operating as normal.
-   The SDK can be disabled before initializing.

```javascript
// Disables the SDK
XenditFingerprintSDK.setEnabled(false);
// Re-enables the SDK
XenditFingerprintSDK.setEnabled(true);
```

#### **Parameters**

| Name   | Type    | Required | Description                                          |
| ------ | ------- | -------- | ---------------------------------------------------- |
| enable | boolean | Yes      | `true` enables the SDK.<br>`false` disables the SDK. |
