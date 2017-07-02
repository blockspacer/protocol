PRIVATE: P2P Conversations
==========================

Version 1.0 Maxim Sokhatsky

Endpoints
---------
* `p2p/RosterId/ParityRosterId` — Topic for client send MQTT
* `p2p/+/RosterId` — Topic for client subscribe MQTT
* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

* `{'Message',_,_,_,_,_,_,MsgId,From,To,Sync,Timings,Access,Likes,Payload,Mime,Status}`
* `{'Friend',Roster,User,Status}`
* `{'Confirm',Roster,User,Status}`
* `{'Revoke',Roster,User,Status}`

Overview
--------

PRIVATE API serves the peer-to-peer private conversations.

Protocol
--------

### Publish Private Message to a Friend

1. client sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `p2p/Roster/ParityRoster` once.
<!-- 1. client sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `events/Node/api/anon/ClientId/Token` once. -->
<!-- 2. server sends `{'Message',_,_,_,_,_,_,_,_,_,_,_,_,_,_,_,_}` to `actions/api/{PartyId,ClientId}/Token` twice -->

### Friend Request
1. client sends `{'Friend',Id,UserId,FriendId,Status}` to `p2p/RosterId/ParityRosterId` once.
<!-- 1. client sends `{'Friend',Id,User,Status}` to `events/Node/api/anon/ClientId/Token` once. -->

### Confirmation / Authorization

1. client sends `{'Confirm',Id,User,Status}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Confirm',Id,User,Status}` to `actions/api/PartyId/Token` once.

### Revoke

1. client sends `{'Revoke',Id,User,Status}` to `events/Node/api/anon/ClientId/Token` once.
2. server sends `{'Revoke',Id,User,Status}` to `actions/api/PartyId/Token` once.