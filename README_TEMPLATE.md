# websocket-as-promised

[![Build Status](https://travis-ci.org/vitalets/websocket-as-promised.svg?branch=master)](https://travis-ci.org/vitalets/websocket-as-promised)
[![npm](https://img.shields.io/npm/v/websocket-as-promised.svg)](https://www.npmjs.com/package/websocket-as-promised)
[![license](https://img.shields.io/npm/l/websocket-as-promised.svg)](https://www.npmjs.com/package/websocket-as-promised)

> [Promise]-based [W3C WebSocket] wrapper

This library allows to use promises when connecting, disconnecting and messaging with WebSocket server.

## Installation
```bash
npm install websocket-as-promised --save
```

## Usage in browser
```js
const WebSocketAsPromised = require('websocket-as-promised');

const wsp = new WebSocketAsPromised();

// connect
wsp.open('ws://echo.websocket.org')
  .then(() => console.log('Connected.'));

// send data and expect response message
wsp.request({foo: 'bar'})
  .then(response => console.log('Response message received', response));

// disconnect
wsp.close()
  .then(() => console.log('Disconnected.'));

```

## Usage in Node.js
As there is no built-in WebSocket client in Node.js, you should use a third-party module.
The most popular W3C compatible solution is [websocket](https://www.npmjs.com/package/websocket):
```js
const W3CWebSocket = require('websocket').w3cwebsocket;
const WebSocketAsPromised = require('websocket-as-promised');

const wsp = new WebSocketAsPromised({
  createWebSocket: url => new W3CWebSocket(url)
});
wsp.open('ws://echo.websocket.org')
  .then(...)

```

## Messaging
1. if you want to send message and expect server's response - you should use `.request()` method that returns promise:
    ```js
    wsp.request({foo: 'bar'}); // returns promise
    // actually sends message with unique id: {id: 'xxxxx', foo: 'bar'}
    // promise waits response message with the same id: {id: 'xxxxx', response: 'ok'}
    ```

2. if you want to just send data and do not expect any response - use `.send()` method that does not return promise:
    ```js
    wsp.send({foo: 'bar'}); // returns undefined
    ```

## API

{{>main}}

## License
MIT @ [Vitaliy Potapov](https://github.com/vitalets)

[W3C WebSocket]: https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API
[Promise]: https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise 