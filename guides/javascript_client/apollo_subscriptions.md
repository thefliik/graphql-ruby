---
layout: guide
search: true
section: JavaScript Client
title: Apollo Subscriptions
desc: GraphQL subscriptions with GraphQL-Ruby and Apollo Client
index: 2
---

`graphql-ruby-client` includes support for Apollo client subscriptions over {% internal_link "Pusher", "/subscriptions/pusher_implementation" %} or {% internal_link "ActionCable", "/subscriptions/action_cable_implementation" %}.

To use it, require `subscriptions/addGraphQLSubscriptions` and call the function with your network interface and transport client (example below).

See the {% internal_link "Subscriptions guide", "/subscriptions/overview" %} for information about server-side setup.

## Pusher

Pass `{pusher: pusherClient}` to use Pusher:

```js
// Load Pusher and create a client
var Pusher = require("pusher-js")
var pusherClient = new Pusher(appKey, options)

// Add subscriptions to the network interface with the `pusher:` options
var addGraphQLSubscriptions = require("graphql-ruby-client/subscriptions/addGraphQLSubscriptions")
addGraphQLSubscriptions(myNetworkInterface, {pusher: pusherClient})

// Optionally, add persisted query support:
var OperationStoreClient = require("./OperationStoreClient")
RailsNetworkInterface.use([OperationStoreClient.apolloMiddleware])
```

## ActionCable

By passing `{cable: cable}`, all `subscription` queries will be routed to ActionCable.

For example:

```js
// Load ActionCable and create a consumer
var ActionCable = require('actioncable')
var cable = ActionCable.createConsumer()
window.cable = cable

// Load ApolloClient and create a network interface
var apollo = require('apollo-client')
var RailsNetworkInterface = apollo.createNetworkInterface({
 uri: '/graphql',
 opts: {
   credentials: 'include',
 },
 headers: {
   'X-CSRF-Token': $("meta[name=csrf-token]").attr("content"),
 }
});

// Add subscriptions to the network interface
var addGraphQLSubscriptions = require("graphql-ruby-client/subscriptions/addGraphQLSubscriptions")
addGraphQLSubscriptions(RailsNetworkInterface, {cable: cable})

// Optionally, add persisted query support:
var OperationStoreClient = require("./OperationStoreClient")
RailsNetworkInterface.use([OperationStoreClient.apolloMiddleware])
```
