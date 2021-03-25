# Gateway

The Gateway is Guilded's form of real-time communication over secure WebSockets. Clients will receive events and data over the gateway they are connected to and send data over the REST API. The API for interacting with Gateways is complex and fairly unforgiving, therefore it's highly recommended you read _all_ of the following documentation before writing a custom implementation.

## Payloads

###### Gateway Payload Structure

The only constant field you will receive is `type` - the event's name. Except when connecting, `type` _will not_ be `null`; it will simply not be included.

The rest of the event's data is included top-level with the event payload (e.g. no `data` field which contains it all). For reference purposes, your typical Gateway payload will look something like this:

```json
{
  "type": "EventName",
  "somethingId": "ffd717d5-73f1-11eb-8d50-6245b4f631c5",
  "listOfThings": [],
  "stackedContentObject": {
    "nodes": [{
      "leaves": [{
        "content": "hey go write something funny for the docs"
      }]
    }]
  },
  "happenedAt": "ISO8601.T.ime:sta:mpZ",
  "imageLink": "https://the-entire-url.to-the-file-on.aws/because_lol-Large.webp",
  "madeBy": "5A2B60"
}
```

## Connecting to the Gateway

### Connecting

###### Gateway URL Params

| Field      | Type    | Description                     | Accepted Values |
|------------|---------|---------------------------------|-----------------|
| jwt        | string  | Authentication stuff?           | 'undefined'     |
| EIO        | integer | Engine.IO version               | 3               |
| transport  | string  | The type of transport           | 'websocket'     |
| ?teamId \* | string  | The team's socket to connect to | a team ID       |

\* This is for team-specific events like member and channel updates. I have yet to get this to work personally, but it's what the client does.

###### Gateway Headers

| Header | Description                | Accepted Values                                              |
|--------|----------------------------|--------------------------------------------------------------|
| cookie | your authentication cookie | a cookie gotten from [logging in via REST](/reference#quick-authentication-how-to) |

The first step in establishing connectivity to the gateway is to construct your gateway URL. Options are pretty limited, so you should end up with something like `wss://api.guilded.gg/socket.io/?jwt=undefined&EIO=3&transport=websocket`.

You can now open a websocket connection to the URL you have created (or copied from this page).

Once connected, the client should immediately receive [a "hello"-type payload](#example-gateway-hello), with information on the connection's heartbeat interval.

###### Example Gateway Hello

```json
{
  "sid": "your_session_id",
  "upgrades": [],
  "pingInterval": 25000,
  "pingTimeout": 5000
}
```

### Heartbeating

The client should now begin sending heartbeat payloads (the string '2') every `pingInterval` milliseconds, until the connection is eventually closed or terminated, from now until the end of time, or forever hold your peace. It is unknown whether Guilded will suddenly request you for a heartbeat, so documentation for this case is not available.

###### Example Gateway Heartbeat ACK

```json
"3"
```

### Disconnections

If the gateway ever issues a disconnect to your client, it will provide a close event code that you can use to properly handle the disconnection. A full list of these close codes can be found in the [Response Codes](#DOCS_TOPICS_OPCODES_AND_STATUS_CODES/gateway-close-event-codes) documentation.

When you close the connection to the gateway with the close code 1000 or 1001, your session will be invalidated and your bot will appear offline. If you simply close the TCP connection, or use a different close code, the bot session will remain active and timeout after a few minutes. This can be useful for a reconnect, which will resume the previous session.

todo: verify that above applies

## Rate Limiting

!!! info
    This section is about Gateway rate limits, not [HTTP API rate limits](/reference/#rate-limiting).

Currently unknown. See above.

<!--Clients are allowed to send 120 [gateway commands](#DOCS_TOPICS_GATEWAY/commands-and-events) every 60 seconds, meaning you can send an average of 2 commands per second. Clients who surpass this limit are immediately disconnected from the Gateway, and similarly to the HTTP API, repeat offenders will have their API access revoked. Clients also have a limit of [concurrent](#DOCS_TOPICS_GATEWAY/session-start-limit-object) [Identify](#DOCS_TOPICS_GATEWAY/identify) requests allowed per 5 seconds. If you hit this limit, the Gateway will respond with an [Opcode 9 Invalid Session](#DOCS_TOPICS_GATEWAY/invalid-session).-->

## Commands and Events

Commands are requests made to the gateway socket by a client.

###### Gateway Commands

| name                              | description                              |
|-----------------------------------|------------------------------------------|
| [Heartbeat](#heartbeating)        | maintains an active gateway connection   |
| [Trigger Typing](#trigger-typing) | shows your typing indicator in a channel |

Events are payloads sent over the socket to a client that correspond to events in Discord.

###### Gateway Events

!!! info
    The `TeamSocketReconnected` event may be misspelled.

| Name                                  | Description     |
|---------------------------------------|-----------------|
| [Hello](#example-gateway-hello)       | defines the heartbeat |
| DMChatChannelCreated                  | new dm channel created |
| ChatChannelBroadcastCall              | 
| ChatChannelBroadcastCallResponse      |
| ChatChannelUpdated                    | team channel was updated 
| ChatChannelTyping                     | user began typing in a channel 
| ChatChannelHidden                     | team channel was hidden
| ChatChannelNicknameChanged            | 
| ChatMessageCreated                    | message was sent
| ChatMessageUpdated                    | message was edited
| ChatMessageDeleted                    | message was deleted
| ChatMessagesDeleted                   | multiple messages were deleted at once
| ChatMessageReactionAdded              | user reacted to a message
| ChatMessageReactionDeleted            | user removed a reaction to a message
| ChatPinnedMessageCreated              | message was pinned
| ChatPinnedMessageDeleted              | message was unpinned
| TemporalChannelCreated                | thread was created
| TemporalChannelUsersAdded             | user was added to a thread
| TeamSocketReconnected                 | client reconnected to a team websocket
| TeamRolesUpdated                      | role was created/removed/edited in a team
| TeamXpAdded                           | 
| TeamXpSet                             | 
| TeamReactionsUpdated                  | 
| TeamContentReactionsAdded             | 
| TeamContentReactionsRemoved           | 
| TeamContentDeleted                    | 
| TeamMessagesDeleted                   | 
| TeamThreadCreated                     | 
| TeamThreadReplyCreated                |
| TeamEventModified                     | 
| TeamEventCreated                      | 
| TeamEventRemoved                      | 
| TeamMemberJoined                      | member joined a team
| TeamMemberRemoved                     | user removed from a team
| TeamMembersRemoved                    | user were pruned from a team
| TeamMutedMembersUpdated               | member was muted or unmuted
| TeamDeafenedMembersUpdated            | member was deafened or undeafened
| TeamMemberAliasUpdated                | member's nickname was set or reset
| TeamSubscriptionInfoUpdated           | probably something about guilded gold
| TeamStreamInfoUpdated                 | member started or stopped video streaming
| TeamContentOperationsApplied          | 
| TeamContentReplaced                   |
| TeamContentReplyAdded                 |
| TeamContentReplyRemoved               |
| TeamAvailabilitiesUpdated             |
| TeamChannelAvailabilitiesUpdated      |
| TeamChannelAvailabilitiesRemoved      |
| TeamMemberUpdated                     | member was updated
| TeamMemberSocialLinkUpdated           | social media info for a team was updated
| TeamGameAdded                         | game was added to a team
| TeamApplicationCreated                | user applied to join a team
| TeamApplicationRemoved                | user's application was deleted (or denied?)
| TeamApplicationUpdated                | user's application was updated
| TeamUpdated                           | team was updated
| TeamChannelCreated                    | new team channel was created
| TeamChannelUpdated                    | team channel was updated
| TeamChannelPrioritiesUpdated          | team channel permissions (or position?) was updated
| TeamChannelCategoryPrioritiesUpdated  | category permissions (or position?) was updated
| TeamChannelDeleted                    | team channel was deleted
| TeamChannelsDeleted                   | multiple team channels were deleted
| TeamChannelCategoryCreated            | category was created
| TeamChannelCategoryUpdated            | category was updated
| TeamChannelCategoryDeleted            | category was deleted
| TeamChannelCategoriesDeleted          | multiple categories were deleted
| TeamChannelCategoryGroupMoved         | category position was changed
| TeamWebhookCreated                    | webhook was added to a team
| TeamWebhookUpdated                    | webhook was updated
| TeamBotCreated                        | flow-bot was added to a team 
| TeamBotUpdated                        | flow-bot's info was updated 
| TeamBotFlowUpdated                    | flow-bot's flow was updated 
| TeamGroupPrioritiesUpdated            | 
| TeamGroupDeleted                      | group was deleted
| TeamGroupArchived                     | group was archived
| TeamGroupRestored                     | group was restored
| TeamGroupFollowed                     | user followed a group
| TeamStripeAccountOnboarded            | 
| TeamServerSubscriptionPlanCreated     | 
| TeamServerSubscriptionPlanUpdated     | 
| TeamServerSubscriptionPlanDeleted     | 
| TeamServerSubscriptionUpdated         | 
| UserSocketConnected                   | 
| UserSocketConnectError                | 
| UserSocketReconnected                 | 
| UserSocketReconnecting                | 
| UserAliasUpdated                      | 
| TeamMemberStreamUpdated               |
| UserPresenceManuallySet               |
| UserPresenceReceived                  |
| UserPinged                            |
| UserUpdated                           |
| UserTeamsUpdated                      |
| UserSocialLinkUpdated                 |
| UserTeamSectionUnreadCountIncremented |
| UserScannedMobileDownloadQr           |
| UserStreamsVisibilityUpdated          |
| TeamChannelContentCreated             |
| TeamChannelContentCreatedSilent       |
| TeamChannelContentReplyCreated        |
| TeamChannelContentReplyUpdated        |
| TeamChannelContentReplyDeleted        |
| TeamChannelContentUpdated             |
| TeamChannelContentDeleted             |
| TeamChannelArchived                   |
| TeamChannelRestored                   |
| TeamChannelVoiceParticipantAdded      |
| TeamChannelVoiceParticipantRemoved    |
| TeamChannelVoiceUserClientConnected   |
| TeamChannelStreamUserClientConnected  |
| TeamChannelStreamAdded                |
| TeamChannelStreamRemoved              |
| TeamChannelStreamActive               |
| TeamChannelStreamEnded                |
| TeamChannelVoiceUserMoved             |
| ChannelSeen                           | client marked channel as read
| ChannelContentSeen                    | ^ ?
| ChannelBadged                         |
| ChannelUnbadged                       |
| TeamGroupCreated                      |
| TeamGroupUpdated                      |
| TeamGroupDeletedForUser               |
| TeamUserGroupPrioritiesUpdated        |
| TeamGroupMarkedAsRead                 |
| MediaUploadProgress                   |
| VoiceChannelRegionPingReport          |

### Event Names

Event names are sent with each word capitalized and with no spaces/separators. For instance, [Message Create](#ChatMessageCreated) would be `ChatMessageCreated`. Most of them are not exactly easily guessable, so the literal event name is preferred in this documentation for utility purposes.

#### Trigger Typing

Begin "typing" in a specific channel. Send your payload as a string prefixed with "42". Like so:

```json
42["ChatChannelTyping", {"channelId": "channelId"}]
```

### Connecting

#### Hello

Sent on connection to the websocket. Defines the heartbeat interval that the client should heartbeat to.

###### Hello Structure

| Field        | Type    | Description                                                     |
|--------------|---------|-----------------------------------------------------------------|
| sid          | string  | your session's id                                               |
| upgrades     | list    | possibly stuff like guilded gold that you've bought             |
| pingInterval | integer | the interval (in milliseconds) the client should heartbeat with |
| pingTimeout  | integer | how long (in milliseconds) guilded will wait without a heartbeat before disconnecting the client |

###### Example Hello

```json
{
    "sid": "your_session_id",
    "upgrades": [],
    "pingInterval": 25000,
    "pingTimeout": 5000
}
```

todo: fact check most stuff below

### Channels

#### DMChatChannelCreated

Someone has created a new DM channel with you. Congrats! This is also sent when _you_ create the channel, which is less exciting. You'll get a [channel](/resources/channel#channel-object) object in the payload.


<!--### Channels

#### Channel Create

Sent when a new guild channel is created, relevant to the current user. The inner payload is a [channel](#DOCS_RESOURCES_CHANNEL/channel-object) object.

#### Channel Update

Sent when a channel is updated. The inner payload is a [channel](#DOCS_RESOURCES_CHANNEL/channel-object) object. This is not sent when the field `last_message_id` is altered. To keep track of the last_message_id changes, you must listen for [Message Create](#DOCS_TOPICS_GATEWAY/message-create) events.

#### Channel Delete

Sent when a channel relevant to the current user is deleted. The inner payload is a [channel](#DOCS_RESOURCES_CHANNEL/channel-object) object.

#### Channel Pins Update

Sent when a message is pinned or unpinned in a text channel. This is not sent when a pinned message is deleted.

###### Channel Pins Update Event Fields

| Field               | Type               | Description                                                  |
|---------------------|--------------------|--------------------------------------------------------------|
| guild_id?           | snowflake          | the id of the guild                                          |
| channel_id          | snowflake          | the id of the channel                                        |
| last_pin_timestamp? | ?ISO8601 timestamp | the time at which the most recent pinned message was pinned  |

### Guilds

#### Guild Create

This event can be sent in three different scenarios:

1.  When a user is initially connecting, to lazily load and backfill information for all unavailable guilds sent in the [Ready](#DOCS_TOPICS_GATEWAY/ready) event. Guilds that are unavailable due to an outage will send a [Guild Delete](#DOCS_TOPICS_GATEWAY/guild-delete) event.
2.  When a Guild becomes available again to the client.
3.  When the current user joins a new Guild.

The inner payload is a [guild](#DOCS_RESOURCES_GUILD/guild-object) object, with all the extra fields specified.

> warn
> If your bot does not have the `GUILD_PRESENCES` [Gateway Intent](#DOCS_TOPICS_GATEWAY/gateway-intents), or if the guild has over 75k members, members and presences returned in this event will only contain your bot and users in voice channels.

#### Guild Update

Sent when a guild is updated. The inner payload is a [guild](#DOCS_RESOURCES_GUILD/guild-object) object.

#### Guild Delete

Sent when a guild becomes or was already unavailable due to an outage, or when the user leaves or is removed from a guild. The inner payload is an [unavailable guild](#DOCS_RESOURCES_GUILD/unavailable-guild-object) object. If the `unavailable` field is not set, the user was removed from the guild.

#### Guild Ban Add

Sent when a user is banned from a guild.

###### Guild Ban Add Event Fields

| Field    | Type                                              | Description     |
|----------|---------------------------------------------------|-----------------|
| guild_id | snowflake                                         | id of the guild |
| user     | a [user](#DOCS_RESOURCES_USER/user-object) object | the banned user |

#### Guild Ban Remove

Sent when a user is unbanned from a guild.

###### Guild Ban Remove Event Fields

| Field    | Type                                              | Description       |
|----------|---------------------------------------------------|-------------------|
| guild_id | snowflake                                         | id of the guild   |
| user     | a [user](#DOCS_RESOURCES_USER/user-object) object | the unbanned user |

#### Guild Emojis Update

Sent when a guild's emojis have been updated.

###### Guild Emojis Update Event Fields

| Field    | Type      | Description                                           |
|----------|-----------|-------------------------------------------------------|
| guild_id | snowflake | id of the guild                                       |
| emojis   | array     | array of [emojis](#DOCS_RESOURCES_EMOJI/emoji-object) |

#### Guild Integrations Update

Sent when a guild integration is updated.

###### Guild Integrations Update Event Fields

| Field    | Type      | Description                                     |
|----------|-----------|-------------------------------------------------|
| guild_id | snowflake | id of the guild whose integrations were updated |

#### Guild Member Add

> warn
> If using [Gateway Intents](#DOCS_TOPICS_GATEWAY/gateway-intents), the `GUILD_MEMBERS` intent will be required to receive this event.

Sent when a new user joins a guild. The inner payload is a [guild member](#DOCS_RESOURCES_GUILD/guild-member-object) object with an extra `guild_id` key:

###### Guild Member Add Extra Fields

| Field    | Type      | Description     |
|----------|-----------|-----------------|
| guild_id | snowflake | id of the guild |

#### Guild Member Remove

> warn
> If using [Gateway Intents](#DOCS_TOPICS_GATEWAY/gateway-intents), the `GUILD_MEMBERS` intent will be required to receive this event.

Sent when a user is removed from a guild (leave/kick/ban).

###### Guild Member Remove Event Fields

| Field    | Type                                              | Description              |
|----------|---------------------------------------------------|--------------------------|
| guild_id | snowflake                                         | the id of the guild      |
| user     | a [user](#DOCS_RESOURCES_USER/user-object) object | the user who was removed |

#### Guild Member Update

> warn
> If using [Gateway Intents](#DOCS_TOPICS_GATEWAY/gateway-intents), the `GUILD_MEMBERS` intent will be required to receive this event.

Sent when a guild member is updated. This will also fire when the user object of a guild member changes.

###### Guild Member Update Event Fields

| Field          | Type                                              | Description                                                                                                                            |
|----------------|---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| guild_id       | snowflake                                         | the id of the guild                                                                                                                    |
| roles          | array of snowflakes                               | user role ids                                                                                                                          |
| user           | a [user](#DOCS_RESOURCES_USER/user-object) object | the user                                                                                                                               |
| nick?          | ?string                                           | nickname of the user in the guild                                                                                                      |
| joined_at      | ISO8601 timestamp                                 | when the user joined the guild                                                                                                         |
| premium_since? | ?ISO8601 timestamp                                | when the user starting [boosting](https://support.discord.com/hc/en-us/articles/360028038352-Server-Boosting-) the guild               |
| pending?       | boolean                                           | whether the user has not yet passed the guild's [Membership Screening](#DOCS_RESOURCES_GUILD/membership-screening-object) requirements |

#### Guild Members Chunk

Sent in response to [Guild Request Members](#DOCS_TOPICS_GATEWAY/request-guild-members).
You can use the `chunk_index` and `chunk_count` to calculate how many chunks are left for your request.

###### Guild Members Chunk Event Fields

| Field       | Type                                                                       | Description                                                                                 |
|-------------|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| guild_id    | snowflake                                                                  | the id of the guild                                                                         |
| members     | array of [guild member](#DOCS_RESOURCES_GUILD/guild-member-object) objects | set of guild members                                                                        |
| chunk_index | integer                                                                    | the chunk index in the expected chunks for this response (0 <= chunk\_index < chunk\_count) |
| chunk_count | integer                                                                    | the total number of expected chunks for this response                                       |
| not_found?  | array                                                                      | if passing an invalid id to `REQUEST_GUILD_MEMBERS`, it will be returned here               |
| presences?  | array of [presence](#DOCS_TOPICS_GATEWAY/presence) objects                 | if passing true to `REQUEST_GUILD_MEMBERS`, presences of the returned members will be here  |
| nonce?      | string                                                                     | the nonce used in the [Guild Members Request](#DOCS_TOPICS_GATEWAY/request-guild-members)   |

#### Guild Role Create

Sent when a guild role is created.

###### Guild Role Create Event Fields

| Field    | Type                                                  | Description         |
|----------|-------------------------------------------------------|---------------------|
| guild_id | snowflake                                             | the id of the guild |
| role     | a [role](#DOCS_TOPICS_PERMISSIONS/role-object) object | the role created    |

#### Guild Role Update

Sent when a guild role is updated.

###### Guild Role Update Event Fields

| Field    | Type                                                  | Description         |
|----------|-------------------------------------------------------|---------------------|
| guild_id | snowflake                                             | the id of the guild |
| role     | a [role](#DOCS_TOPICS_PERMISSIONS/role-object) object | the role updated    |

#### Guild Role Delete

Sent when a guild role is deleted.

###### Guild Role Delete Event Fields

| Field    | Type      | Description     |
|----------|-----------|-----------------|
| guild_id | snowflake | id of the guild |
| role_id  | snowflake | id of the role  |

### Invites

### Invite Create

Sent when a new invite to a channel is created.

###### Invite Create Event Fields

| Field             | Type                                                    | Description                                                                                                        |
|-------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| channel_id        | snowflake                                               | the channel the invite is for                                                                                      |
| code              | string                                                  | the unique invite [code](#DOCS_RESOURCES_INVITE/invite-object)                                                     |
| created_at        | timestamp                                               | the time at which the invite was created                                                                           |
| guild_id?         | snowflake                                               | the guild of the invite                                                                                            |
| inviter?          | [user](#DOCS_RESOURCES_USER/user-object) object         | the user that created the invite                                                                                   |
| max_age           | integer                                                 | how long the invite is valid for (in seconds)                                                                      |
| max_uses          | integer                                                 | the maximum number of times the invite can be used                                                                 |
| target_user?      | partial [user](#DOCS_RESOURCES_USER/user-object) object | the target user for this invite                                                                                    |
| target_user_type? | integer                                                 | the [type of user target](#DOCS_RESOURCES_INVITE/invite-object-target-user-types) for this invite                  |
| temporary         | boolean                                                 | whether or not the invite is temporary (invited users will be kicked on disconnect unless they're assigned a role) |
| uses              | integer                                                 | how many times the invite has been used (always will be 0)                                                         |

### Invite Delete

Sent when an invite is deleted.

###### Invite Delete Event Fields

| Field      | Type      | Description                                                    |
|------------|-----------|----------------------------------------------------------------|
| channel_id | snowflake | the channel of the invite                                      |
| guild_id?  | snowflake | the guild of the invite                                        |
| code       | string    | the unique invite [code](#DOCS_RESOURCES_INVITE/invite-object) |

### Messages

#### Message Create

Sent when a message is created. The inner payload is a [message](#DOCS_RESOURCES_CHANNEL/message-object) object.

#### Message Update

Sent when a message is updated. The inner payload is a [message](#DOCS_RESOURCES_CHANNEL/message-object) object.

> warn
> Unlike creates, message updates may contain only a subset of the full message object payload (but will always contain an id and channel_id).

#### Message Delete

Sent when a message is deleted.

###### Message Delete Event Fields

| Field      | Type      | Description           |
|------------|-----------|-----------------------|
| id         | snowflake | the id of the message |
| channel_id | snowflake | the id of the channel |
| guild_id?  | snowflake | the id of the guild   |

#### Message Delete Bulk

Sent when multiple messages are deleted at once.

###### Message Delete Bulk Event Fields

| Field      | Type                | Description             |
|------------|---------------------|-------------------------|
| ids        | array of snowflakes | the ids of the messages |
| channel_id | snowflake           | the id of the channel   |
| guild_id?  | snowflake           | the id of the guild     |

#### Message Reaction Add

Sent when a user adds a reaction to a message.

###### Message Reaction Add Event Fields

| Field      | Type                                                         | Description                                                                                                     |
|------------|--------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| user_id    | snowflake                                                    | the id of the user                                                                                              |
| channel_id | snowflake                                                    | the id of the channel                                                                                           |
| message_id | snowflake                                                    | the id of the message                                                                                           |
| guild_id?  | snowflake                                                    | the id of the guild                                                                                             |
| member?    | [member](#DOCS_RESOURCES_GUILD/guild-member-object) object   | the member who reacted if this happened in a guild                                                              |
| emoji      | a partial [emoji](#DOCS_RESOURCES_EMOJI/emoji-object) object | the emoji used to react - [example](#DOCS_RESOURCES_EMOJI/emoji-object-gateway-reaction-standard-emoji-example) |

#### Message Reaction Remove

Sent when a user removes a reaction from a message.

###### Message Reaction Remove Event Fields

| Field      | Type                                                         | Description                                                                                                     |
|------------|--------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| user_id    | snowflake                                                    | the id of the user                                                                                              |
| channel_id | snowflake                                                    | the id of the channel                                                                                           |
| message_id | snowflake                                                    | the id of the message                                                                                           |
| guild_id?  | snowflake                                                    | the id of the guild                                                                                             |
| emoji      | a partial [emoji](#DOCS_RESOURCES_EMOJI/emoji-object) object | the emoji used to react - [example](#DOCS_RESOURCES_EMOJI/emoji-object-gateway-reaction-standard-emoji-example) |

#### Message Reaction Remove All

Sent when a user explicitly removes all reactions from a message.

###### Message Reaction Remove All Event Fields

| Field      | Type      | Description           |
|------------|-----------|-----------------------|
| channel_id | snowflake | the id of the channel |
| message_id | snowflake | the id of the message |
| guild_id?  | snowflake | the id of the guild   |

#### Message Reaction Remove Emoji

Sent when a bot removes all instances of a given emoji from the reactions of a message.

###### Message Reaction Remove Emoji

| Field      | Type                                                       | Description                |
|------------|------------------------------------------------------------|----------------------------|
| channel_id | snowflake                                                  | the id of the channel      |
| guild_id?  | snowflake                                                  | the id of the guild        |
| message_id | snowflake                                                  | the id of the message      |
| emoji      | [partial emoji object](#DOCS_RESOURCES_EMOJI/emoji-object) | the emoji that was removed |

### Presence

#### Presence Update

> warn
> If you are using [Gateway Intents](#DOCS_TOPICS_GATEWAY/gateway-intents), you _must_ specify the `GUILD_PRESENCES` intent in order to receive Presence Update events

A user's presence is their current state on a guild. This event is sent when a user's presence or info, such as name or avatar, is updated.

> warn
> The user object within this event can be partial, the only field which must be sent is the `id` field, everything else is optional. Along with this limitation, no fields are required, and the types of the fields are **not** validated. Your client should expect any combination of fields and types within this event.

###### Presence Update Event Fields

| Field         | Type                                                              | Description                                  |
|---------------|-------------------------------------------------------------------|----------------------------------------------|
| user          | [user](#DOCS_RESOURCES_USER/user-object) object                   | the user presence is being updated for       |
| guild_id      | snowflake                                                         | id of the guild                              |
| status        | string                                                            | either "idle", "dnd", "online", or "offline" |
| activities    | array of [activity](#DOCS_TOPICS_GATEWAY/activity-object) objects | user's current activities                    |
| client_status | [client_status](#DOCS_TOPICS_GATEWAY/client-status-object) object | user's platform-dependent status             |

#### Client Status Object

Active sessions are indicated with an "online", "idle", or "dnd" string per platform. If a user is offline or invisible, the corresponding field is not present.

| Field    | Type   | Description                                                                           |
|----------|--------|---------------------------------------------------------------------------------------|
| desktop? | string | the user's status set for an active desktop (Windows, Linux, Mac) application session |
| mobile?  | string | the user's status set for an active mobile (iOS, Android) application session         |
| web?     | string | the user's status set for an active web (browser, bot account) application session    |

#### Activity Object

###### Activity Structure

| Field           | Type                                                                          | Description                                                                                                               |
|-----------------|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| name            | string                                                                        | the activity's name                                                                                                       |
| type            | integer                                                                       | [activity type](#DOCS_TOPICS_GATEWAY/activity-object-activity-types)                                                      |
| url?            | ?string                                                                       | stream url, is validated when type is 1                                                                                   |
| created_at      | integer                                                                       | unix timestamp of when the activity was added to the user's session                                                       |
| timestamps?     | [timestamps](#DOCS_TOPICS_GATEWAY/activity-object-activity-timestamps) object | unix timestamps for start and/or end of the game                                                                          |
| application_id? | snowflake                                                                     | application id for the game                                                                                               |
| details?        | ?string                                                                       | what the player is currently doing                                                                                        |
| state?          | ?string                                                                       | the user's current party status                                                                                           |
| emoji?          | ?[emoji](#DOCS_TOPICS_GATEWAY/activity-object-activity-emoji) object          | the emoji used for a custom status                                                                                        |
| party?          | [party](#DOCS_TOPICS_GATEWAY/activity-object-activity-party) object           | information for the current party of the player                                                                           |
| assets?         | [assets](#DOCS_TOPICS_GATEWAY/activity-object-activity-assets) object         | images for the presence and their hover texts                                                                             |
| secrets?        | [secrets](#DOCS_TOPICS_GATEWAY/activity-object-activity-secrets) object       | secrets for Rich Presence joining and spectating                                                                          |
| instance?       | boolean                                                                       | whether or not the activity is an instanced game session                                                                  |
| flags?          | integer                                                                       | [activity flags](#DOCS_TOPICS_GATEWAY/activity-object-activity-flags) `OR`d together, describes what the payload includes |

> info
> Bots are only able to send `name`, `type`, and optionally `url`.

###### Activity Types

| ID | Name      | Format              | Example                              |
|----|-----------|---------------------|--------------------------------------|
| 0  | Game      | Playing {name}      | "Playing Rocket League"              |
| 1  | Streaming | Streaming {details} | "Streaming Rocket League"            |
| 2  | Listening | Listening to {name} | "Listening to Spotify"               |
| 4  | Custom    | {emoji} {name}      | ":smiley: I am cool"                 |
| 5  | Competing | Competing in {name} | "Competing in Arena World Champions" |

> info
> The streaming type currently only supports Twitch and YouTube. Only `https://twitch.tv/` and `https://youtube.com/` urls will work.

###### Activity Timestamps

| Field  | Type    | Description                                              |
|--------|---------|----------------------------------------------------------|
| start? | integer | unix time (in milliseconds) of when the activity started |
| end?   | integer | unix time (in milliseconds) of when the activity ends    |

###### Activity Emoji

| Field     | Type      | Description                    |
|-----------|-----------|--------------------------------|
| name      | string    | the name of the emoji          |
| id?       | snowflake | the id of the emoji            |
| animated? | boolean   | whether this emoji is animated |

###### Activity Party

| Field | Type                                           | Description                                       |
|-------|------------------------------------------------|---------------------------------------------------|
| id?   | string                                         | the id of the party                               |
| size? | array of two integers (current_size, max_size) | used to show the party's current and maximum size |

###### Activity Assets

| Field        | Type   | Description                                                       |
|--------------|--------|-------------------------------------------------------------------|
| large_image? | string | the id for a large asset of the activity, usually a snowflake     |
| large_text?  | string | text displayed when hovering over the large image of the activity |
| small_image? | string | the id for a small asset of the activity, usually a snowflake     |
| small_text?  | string | text displayed when hovering over the small image of the activity |

###### Activity Secrets

| Field     | Type   | Description                               |
|-----------|--------|-------------------------------------------|
| join?     | string | the secret for joining a party            |
| spectate? | string | the secret for spectating a game          |
| match?    | string | the secret for a specific instanced match |

###### Activity Flags

| Name         | Value  |
|--------------|--------|
| INSTANCE     | 1 << 0 |
| JOIN         | 1 << 1 |
| SPECTATE     | 1 << 2 |
| JOIN_REQUEST | 1 << 3 |
| SYNC         | 1 << 4 |
| PLAY         | 1 << 5 |

###### Example Activity

```json
{
  "details": "24H RL Stream for Charity",
  "state": "Rocket League",
  "name": "Twitch",
  "type": 1,
  "url": "https://www.twitch.tv/discordapp"
}
```

###### Example Activity with Rich Presence

```json
{
  "name": "Rocket League",
  "type": 0,
  "application_id": "379286085710381999",
  "state": "In a Match",
  "details": "Ranked Duos: 2-1",
  "timestamps": {
    "start": 15112000660000
  },
  "party": {
    "id": "9dd6594e-81b3-49f6-a6b5-a679e6a060d3",
    "size": [2, 2]
  },
  "assets": {
    "large_image": "351371005538729000",
    "large_text": "DFH Stadium",
    "small_image": "351371005538729111",
    "small_text": "Silver III"
  },
  "secrets": {
    "join": "025ed05c71f639de8bfaa0d679d7c94b2fdce12f",
    "spectate": "e7eb30d2ee025ed05c71ea495f770b76454ee4e0",
    "match": "4b2fdce12f639de8bfa7e3591b71a0d679d7c93f"
  }
}
```

> warn
> Clients may only update their game status 5 times per 20 seconds.

#### Typing Start

Sent when a user starts typing in a channel.

###### Typing Start Event Fields

| Field      | Type                                                       | Description                                               |
|------------|------------------------------------------------------------|-----------------------------------------------------------|
| channel_id | snowflake                                                  | id of the channel                                         |
| guild_id?  | snowflake                                                  | id of the guild                                           |
| user_id    | snowflake                                                  | id of the user                                            |
| timestamp  | integer                                                    | unix time (in seconds) of when the user started typing    |
| member?    | [member](#DOCS_RESOURCES_GUILD/guild-member-object) object | the member who started typing if this happened in a guild |

#### User Update

Sent when properties about the user change. Inner payload is a [user](#DOCS_RESOURCES_USER/user-object) object.

### Voice

#### Voice State Update

Sent when someone joins/leaves/moves voice channels. Inner payload is a [voice state](#DOCS_RESOURCES_VOICE/voice-state-object) object.

#### Voice Server Update

Sent when a guild's voice server is updated. This is sent when initially connecting to voice, and when the current voice instance fails over to a new server.

###### Voice Server Update Event Fields

| Field    | Type      | Description                               |
|----------|-----------|-------------------------------------------|
| token    | string    | voice connection token                    |
| guild_id | snowflake | the guild this voice server update is for |
| endpoint | string    | the voice server host                     |

###### Example Voice Server Update Payload

```json
{
  "token": "my_token",
  "guild_id": "41771983423143937",
  "endpoint": "smart.loyal.discord.gg"
}
```

### Webhooks

#### Webhooks Update

Sent when a guild channel's webhook is created, updated, or deleted.

###### Webhook Update Event Fields

| Field      | Type      | Description       |
|------------|-----------|-------------------|
| guild_id   | snowflake | id of the guild   |
| channel_id | snowflake | id of the channel |
-->