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

It is important to note the Gateway's base URL here (which is different from [the REST API's](/reference)):

```
wss://api.guilded.gg
```

### Connecting

###### Gateway URL Params

| Field           | Type    | Description                     | Accepted Values                          |
|-----------------|---------|---------------------------------|------------------------------------------|
| jwt             | string  | authentication stuff?           | 'undefined'                              |
| EIO             | integer | Engine.IO version               | 3                                        |
| transport       | string  | the type of transport           | 'websocket'                              |
| guildedClientId | string  | used for authentication         | the `guilded_mid` cookie received when [logging in via REST](/reference#quick-authentication-how-to) |
| ?teamId \*      | string  | the team's socket to connect to | a [team ID](/resources/team#team-object) |

\* This is for team-specific events like member and channel updates. In order to receive a team's events, you will need to open a separate connection for it, which you should treat just like the main websocket - heartbeat and all.

The first step in establishing connectivity to the gateway is to construct your gateway URL. Options are pretty limited, so you should end up with something like `wss://api.guilded.gg/socket.io/?jwt=undefined&EIO=3&transport=websocket&guildedClientId={guilded_mid_header}` (assuming you don't want to connect to a team websocket).

You can now open a websocket connection to the URL you have created.

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

!!! todo
    verify that above applies

## Rate Limiting

!!! info
    This section is about Gateway rate limits, not [HTTP API rate limits](/reference/#rate-limiting).

Currently unknown. See above.

## Commands and Events

Commands are requests made by a client over the gateway.

###### Gateway Commands

| name                              | description                              |
|-----------------------------------|------------------------------------------|
| [Heartbeat](#heartbeating)        | maintains an active gateway connection   |
| [Trigger Typing](#trigger-typing) | shows your typing indicator in a channel |

Events are payloads sent over the gateway to a client that correspond to events in Guilded.

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

!!! todo
    fact check most stuff below

### Channels

#### DMChatChannelCreated

Someone has created a new DM channel with you. Congrats! This is also sent when _you_ create the channel, which is less exciting. You'll get a [channel](/resources/channel#channel-object) object in the payload.
