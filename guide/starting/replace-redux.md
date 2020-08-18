# Replacing Redux to Logux Redux

If you already have Redux application, this guide will help you to add [Logux Redux] to project. Logux Redux can work with any UI framework. We will use React only as an example.

If you are starting new project use [special guide].

[special guide]: ./new-redux-client.md
[Logux Redux]: https://github.com/logux/redux


## Server

Before starting the client, you need to create Logux Server:

* [How to create the server] on Node.js.
* [How to create a proxy] to use HTTP server on any other language.

[How to create the server]: ./node-server.md
[How to create a proxy]: ./proxy-server.md


## Adding Logux Redux

Install Logux Redux:

```sh
npm i @logux/redux
```

</details>

Find store definition in your project. Look for `createStore` function call. Often you can find it at `src/index.js` or `src/store/index.js`.

```diff
- import { createStore } from 'redux'
+ import { createLoguxCreator } from '@logux/redux'
```

```diff
+ const createStore = createLoguxCreator({
+   subprotocol: '1.0.0',
+   server: process.env.NODE_ENV === 'development'
+     ? 'ws://localhost:31337'
+     : 'wss://logux.example.com',
+   userId: 'todo',  // TODO: We will fill it in Authentication recipe
+   token: '' // TODO: We will fill it in Authentication recipe
+ })
  const store = createStore(reducer, preloadedState, enhancer)
+ store.client.start()
```


## Synchronization UI

To see the state of the synchronization process, we will add some helpers. They are all optional, but they are great for a start.

Install Logux Client:

```sh
npm i @logux/client
```

Use helpers where you create the store.

```diff
  import { createLoguxCreator } from '@logux/redux'
+ import { badge, badgeEn, log } from '@logux/client'
+ import { badgeStyles } from '@logux/client/badge/styles'
```

```diff
  const store = createStore(reducer)
+ badge(store.client, { messages: badgeEn, styles: badgeStyles })
+ log(store.client)
```


## Check the Result

1. Open three terminals.
2. Start your Logux server in one terminal by `npm start` in server directory.
3. Start your back-end server in the second terminal.
4. Start your client in the third terminal by `npm start` in client directory.

If badge style doesn’t fit your website style, you can always tweak it or replace with your component. See [`badge()`](https://logux.io/redux-api/#globals-badge) and [`status()`](https://logux.io/redux-api/#globals-status) API.

[Next chapter](../architecture/core.md)
