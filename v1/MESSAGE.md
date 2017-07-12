MESSAGE: Sending and Receiving Messages
=======================================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/1/api/:client` — MQTT
* `events/1/:node/api/anon/:client/:token` — MQTT

Tuples
------

```erlang
-record('Typing',   {author=[] :: list(),
                     room=[] :: list()}).
```

```erlang
-record('History',  {roster_id= [] :: [] | binary(),
                     contact_id=[] :: list(),
                     data=[] :: list(#'Message'{}),
                     status=[] :: atom() | []}).
```

```erlang
-record('Message',  {id=[] :: [] | integer(),
                     container=Container :: atom(),
                     feed_id=[] :: term(),
                     prev=[] :: [] | integer(),
                     next=[] :: [] | integer(),
                     feeds=[] :: list()
                     msg_id = [] :: [] | binary(),
                     from = [] :: [] | binary(),
                     to = [] :: [] | binary(),
                     sync = [] :: [],
                     created = [] :: [],
                     access = [] :: [],
                     starred = [] :: [],
                     payload = <<>> :: [] | binary(),
                     mime = [],
                     seen_by = [],
                     status = [] :: [] | atom()}).
```
Overview
--------

MESSAGE API deliver messages.

Protocol
--------

### Sending Message

```
1. client sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `<<>>` to `actions/api/:client` issuer and
          sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}`
             to `actions/1/api/:client` counterparty.
```

### Retrieve History

```
1. client sends `{'History',Id,Cursor,_,_}`
             to `events/1/:node/api/anon/:client/:token` once.
```

```
2. server sends `{'History',Id,_,Messages,_}`
             to `actions/1/api/:client` once or more.
```