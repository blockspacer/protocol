AUTH: Contact Registration
==========================

Version 1.0 Maxim Sokhatsky

Endpoints
--------

* `actions/api/ClientId` — MQTT
* `events/Node/api/anon/ClientId/Token` — MQTT

Tuples
------

* `{'Auth',Token,UserId,Phone,DevKey,Type,SmsCode,Attempts,Services}`
* `{'Contact',Id,Container,FeedId,Prev,Next,Feeds,Username,Phone,Names,Surnames,Status}`

Overview
--------

AUTH API is dedicated for process of user registration with SMS confirmation.

Protocol
--------

### Auth Issuing / SMS sending

`_` is optional. By default is `[]`.
1. client sends `{'Auth',_,_,Phone,_,_,_,_,_}` to `events/2/api/anon/ClientId/Token` once.
2. server sends `{io,{ok, login},{'Token',Token}}`
             or `{io,{ok, sms_send},{'Token',Token}}`
             or `{io,{error, not_verified},<<>>}`
             or `{io,{error, mismatch_user_data},<<>>}`
             or `{io,{error, Error},{'Token',Token}}}`
             to `actions/api/ClientId/VNode` once.
                          

### User Confirmation

1. client sends `{{verify, SmsCode},{'Token', Token}}` once.
2. server sends `{io,{ok, verified},{'Token',Token}}`
             or `{io,{error,attempts_expired},{'Token',Token}}`
             or `{io,{error,invalid_sms_code}, {'Token',Token}}`
             to `actions/api/ClientId` once.

### Resend User Confirmation
1. client sends `{resend_sms, {'Token', Token}}` once.
2. server sends `{io,{ok, sms_send},{'Token',Token}}`
             or `{io,{error, actual_session}, <<>>}`
             or `{io,{error, session_not_found}, <<>>}`



### User Delete (temporary, only for development)

1. client sends `{delete_user, Phone}` once.
2. server sends `{io,{ok, delete}, {'Token', Token}}` once. Temporary, only for development