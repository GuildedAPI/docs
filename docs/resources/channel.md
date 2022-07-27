# Channels

### Channel Object

Represents a team (server) or DM channel.

###### Team Channel Structure

| Field               | Type               | Description                                                                                              |
|---------------------|--------------------|----------------------------------------------------------------------------------------------------------|
| id                  | uuid               | the id of this channel                                                                                   |
| type                | string             | the [type of channel](#channel-types)                                                                    |
| createdAt           | ISO8601 timestamp  | when this channel was created                                                                            |
| createdBy           | user id            | who created this channel                                                                                 |
| updatedAt           | ?ISO8601 timestamp | when this channel was last modified                                                                      |
| name                | string             | the name of this channel                                                                                 |
| contentType         | string             | the [content type of channel](#channel-content-types) (text, voice, etc)                                 |
| archivedAt          | ?ISO8601 timestamp | if this channel is archived, this is when it was archived                                                |
| parentChannelId     | ?uuid              | if this channel has a parent channel, this is that channel's id                                          |
| autoArchiveAt       | ?ISO8601 timestamp | when this channel will be automatically archived (only applicable to threads)                            |
| deletedAt           | ?ISO8601 timestamp | if this channel is deleted, this is when it was deleted                                                  |
| archivedBy          | ?user id           | if this channel is archived, this is who archived it                                                     |
| description         | ?string            | the channel description                                                                                  |
| createdByWebhookId  | ?uuid              | if a webhook created this channel, this is the webhook's id                                              |
| archivedByWebhookId | ?uuid              | if a webhook archived this channel, this is the webhook's id                                             |
| teamId              | ?team id           | if this is a team channel (not a DM channel), this is the team's id                                      |
| channelCategoryId   | ?uuid              | if this channel is in a category, this is the category's id                                              |
| addedAt             | ?ISO8601 timestamp | ???                                                                                                      |
| channelId           | ?uuid              | same as `id`                                                                                             |
| isRoleSynced        | ?bool              | if this channel's roles are synced with the linked Discord guild's roles                                 |
| roles               | ?array             | a list of roles that (have overwrites in \| are synced with) this channel                                |
| userPermissions     | ?array             | a list of users who have overwrites in this channel                                                      |
| tournamentRoles     | ?array             | ???                                                                                                      |
| isPublic            | bool               | whether this channel is marked as public (can be seen without being a member of the team)                |
| priority            | integer            | ???                                                                                                      |
| groupId             | ?group id          | if this is a team channel, this is the group's id this channel belongs to                                |
| settings            | ???                | ???                                                                                                      |
| groupType           | string             | ???                                                                                                      |
| rolesById           | dictionary         | mapping of roleId: role object with undescript permissions                                               |
| tournamentRolesById | dictionary         | ???                                                                                                      |
| createdByInfo       | dictionary         | contains the `id`, `name`, `profilePicture`, and `profilePictureSm` of the user who created this channel |

###### Channel Types

| Type | Description                    |
|------|--------------------------------|
| Team | a channel within a server      |
| DM   | a direct message between users |

###### Channel Content Types

| Type         | Description              |
|--------------|--------------------------|
| announcement | an announcement channel  |
| chat\*       | a chat channel or thread |
| doc          | a docs channel           |
| forum        | a forum channel          |
| media        | a media channel          |
| list         | a list channel           |
| scheduling   | a scheduling channel     |
| streaming\*  | a streaming channel      |
| voice\*      | a voice channel          |

\* Voice channels and streaming channels are messageable just like chat channels. Threads inside these channels will have the `chat` content type despite their parent's content type.

###### Example Team Chat Channel

```json
{
  "priority": 4,
  "id": "d2242862-401c-42f8-9ce2-0c75977475a6",
  "type": "Team",
  "name": "general",
  "description": "wacky hi-jinks ensue!",
  "settings": null,
  "roles": null,
  "rolesById": {},
  "tournamentRolesById": {},
  "teamId": "4R5q39VR",
  "channelCategoryId": null,
  "addedAt": null,
  "channelId": "d2242862-401c-42f8-9ce2-0c75977475a6",
  "isRoleSynced": null,
  "isPublic": false,
  "groupId": "WD56qLmd",
  "createdAt": "2020-07-31T18:51:19.563Z",
  "createdBy": "EdVMVKR4",
  "updatedAt": "2020-07-31T18:51:19.563Z",
  "contentType": "chat",
  "archivedAt": null,
  "parentChannelId": null,
  "autoArchiveAt": null,
  "deletedAt": null,
  "archivedBy": null,
  "createdByWebhookId": null,
  "archivedByWebhookId": null,
  "userPermissions": null,
  "tournamentRoles": null,
  "voiceParticipants": [],
  "userStreams": []
}
```

### Message Object

Represents a message sent in a channel within Guilded.

###### Message Structure

| Field      | Type                                                 | Description                                                                 |
|------------|------------------------------------------------------|-----------------------------------------------------------------------------|
| id         | uuid                                                 | id of the message                                                           |
| channelId  | [channel id](#channel-object)                        | id of the channel the message was sent in                                   |
| createdBy  | user id                                              | the author's id of this message                                             |
| content    | [message content](#message-content-structure) object | contents of the message (including text, embeds, mentions, and attachments) |
| createdAt  | ISO8601 timestamp                                    | when this message was sent                                                  |
| editedAt   | ?ISO8601 timestamp                                   | when this message was edited (or null if never)                             |
| deletedAt  | ?ISO8601 timestamp                                   | when this message was deleted (or null if it has not been deleted)          |
| reactions  | array of [reaction](#reaction-object) objects        | reactions to the message                                                    |
| isPinned   | boolean                                              | whether this message is pinned                                              |
| pinnedBy   | ?user id                                             | if the message is pinned, this is the user's id who pinned it               |
| webhookId  | ?user id                                             | if the message is generated by a webhook, this is the webhook's id          |
| type       | string                                               | the [type of message](#message-types)                                       |

###### Example Message

```json
{
  "id": "b943384a-d951-4323-8e26-8e3e6b7c431a",
  "content": {
    "object": "value",
    "document": {
      "object": "document",
      "data": {},
      "nodes": [
        {
          "object": "block",
          "type": "paragraph",
          "data": {},
          "nodes": [
            {
              "object": "text",
              "leaves": [
                {
                  "object": "leaf",
                  "text": "cool message with an ",
                  "marks": []
                }
              ]
            },
            {
              "object": "inline",
              "type": "reaction",
              "data": {
                "reaction": {
                  "id": 90001815,
                  "customReactionId": 90001815
                }
              },
              "nodes": [
                {
                  "object": "text",
                  "leaves": [
                    {
                      "object": "leaf",
                      "text": ":tada:",
                      "marks": []
                    }
                  ]
                }
              ]
            },
            {
              "object": "text",
              "leaves": [
                {
                  "object": "leaf",
                  "text": " emoji, some ",
                  "marks": []
                },
                {
                  "object": "leaf",
                  "text": "bold formatting",
                  "marks": [
                    {
                      "object": "mark",
                      "type": "italic",
                      "data": {}
                    }
                  ]
                },
                {
                  "object": "leaf",
                  "text": ", maybe even a ",
                  "marks": []
                }
              ]
            },
            {
              "object": "inline",
              "type": "reaction",
              "data": {
                "reaction": {
                  "id": 294637,
                  "customReactionId": 294637
                }
              },
              "nodes": [
                {
                  "object": "text",
                  "leaves": [
                    {
                      "object": "leaf",
                      "text": ":thinkingwithblobs:",
                      "marks": []
                    }
                  ]
                }
              ]
            },
            {
              "object": "text",
              "leaves": [
                {
                  "object": "leaf",
                  "text": " custom emoji?",
                  "marks": []
                }
              ]
            }
          ]
        }
      ]
    }
  }
}
```

##### Message Types

| Type    | Description                                               |
|---------|-----------------------------------------------------------|
| default | a message sent by a user                                  |
| system  | a system message, usually noting an action done by a user |

### Unfurl Embed Object

###### Unfurl Embed Structure

!!! info
    All of the below fields are optional and nullable.

| Field         | Type   | Description                                  |
|---------------|--------|----------------------------------------------|
| ogTitle       | string | og:title                                     |
| ogDescription | string | og:description                               |
| ogSiteName    | string | og:site_name                                 |
| ogUrl         | string | og:url                                       |
| ogImage.url   | string | proxied og:image                             |
| siteType      | string | the [type of site](#unfurl-embed-site-types) |

###### Example Unfurl Embed

```json
{
  "ogTitle": "Guilded - Chat for Gaming Communities",
  "ogDescription": "Guilded upgrades your group chat and equips your server with integrated event calendars, forums, and more – 100% free.",
  "ogSiteName": "Guilded - Chat for Gaming Communities",
  "ogUrl": "https://www.guilded.gg/",
  "ogImage": {
    "url": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/ExternalOGEmbedImage/11066c41a2569c628f119f2b2e32aa00-Full.png?w=1024&h=1024"
  },
  "siteType": "guilded"
}
```

### Unfurl Embed Site Types

| Value    | Domain         |
|----------|----------------|
| facebook | facebook.com   |
| github   | github.com     |
| guilded  | guilded.gg     |
| reddit   | www.reddit.com |
| twitch   | twitch.tv      |
| twitter  | twitter.com/\* |
| vimeo    | vimeo.com      |
| youtube  | youtube.com/\* |

### Embed Object

Embeds objects in Guilded are the same as in Discord, with the exception of `iconUrl` being preferred over `icon_url` for `embed.author`s and `embed.footer`s.

###### Embed Structure

| Field        | Type                                                   | Description                |
|--------------|--------------------------------------------------------|----------------------------|
| title?       | string                                                 | title of embed             |
| description? | string                                                 | description of embed       |
| url?         | string                                                 | url of embed               |
| timestamp?   | ISO8601 timestamp                                      | timestamp of embed content |
| color?       | integer                                                | color code of the embed    |
| footer?      | [embed footer](#embed-footer-structure) object         | footer information         |
| image?       | [embed image](#embed-image-structure) object           | image information          |
| thumbnail?   | [embed thumbnail](#embed-thumbnail-structure) object   | thumbnail information      |
| video?       | [embed video](#embed-video-structure) object           | video information          |
| provider?    | [embed provider](#embed-provider-structure) object     | provider information       |
| author?      | [embed author](#embed-author-structure) object         | author information         |
| fields?      | array of [embed field](#embed-field-structure) objects | fields information         |

###### Embed Thumbnail Structure

| Field   | Type    | Description                                    |
|---------|---------|------------------------------------------------|
| url?    | string  | source url of thumbnail. only supports http(s) |
| height? | integer | height of thumbnail                            |
| width?  | integer | width of thumbnail                             |

###### Embed Video Structure

| Field   | Type    | Description                |
|---------|---------|----------------------------|
| url?    | string  | source url of video        |
| height? | integer | height of video            |
| width?  | integer | width of video             |

###### Embed Image Structure

| Field   | Type    | Description                                |
|---------|---------|--------------------------------------------|
| url?    | string  | source url of image. only supports http(s) |
| height? | integer | height of image                            |
| width?  | integer | width of image                             |

###### Embed Provider Structure

| Field | Type   | Description      |
|-------|--------|------------------|
| name? | string | name of provider |
| url?  | string | url of provider  |

###### Embed Author Structure

| Field       | Type   | Description                               |
|-------------|--------|-------------------------------------------|
| name?       | string | name of author                            |
| url?        | string | url of author                             |
| iconUrl?\*  | string | url of author icon. only supports http(s) |
| icon_url?\* | string | url of author icon. only supports http(s) |

\* You may provide either `iconUrl` or `icon_url`, but `icon_url` will only show up on mobile devices. For this reason, `iconUrl` is recommend instead.

###### Embed Footer Structure

| Field       | Type   | Description                               |
|-------------|--------|-------------------------------------------|
| text        | string | footer text                               |
| iconUrl?\*  | string | url of footer icon. only supports http(s) |
| icon_url?\* | string | url of footer icon. only supports http(s) |

\* You may provide either `iconUrl` or `icon_url`, but `icon_url` will only show up on mobile devices. For this reason, `iconUrl` is recommend instead.

###### Embed Field Structure

| Field   | Type    | Description                                     |
|---------|---------|-------------------------------------------------|
| name    | string  | name of the field                               |
| value   | string  | value of the field                              |
| inline? | boolean | whether or not this field should display inline |

## Get Embed for URL
<span class="http-verb">GET</span><span class="http-path">/content/embed_info</span>

Generate embed data for a specific URL. Returns a special [unfurl embed object](#unfurl-embed-object) on success. On failure, this will return an empty dictionary with status code 200.

###### Query Params

| Field | Type   | Description                  | Required |
|-------|--------|------------------------------|----------|
| url   | string | the url to get an embed for  | true     |

## Embed Limits

Leading and trailing whitespace characters are included in the following limits; they are kept intact despite not being visible in the desktop client.

| Field                                                    | Limit            |
|----------------------------------------------------------|------------------|
| title                                                    | 256 characters   |
| description                                              | 2048 characters  |
| fields                                                   | 25 field objects |
| [field.name](/resources/channel#embed-field-structure)   | 256 characters   |
| [field.value](/resources/channel#embed-field-structure)  | 1024 characters  |
| [footer.text](/resources/channel#embed-footer-structure) | 2048 characters  |
| [author.name](/resources/channel#embed-author-structure) | 256 characters   |

There is no explicit character limit for the sum of all the above fields, however utilizing every text slot you may fit a total of 36,608 characters per embed.

## Get Channel
<span class="http-verb">GET</span><span class="http-path">/content/route/metadata?route=//channels/{[channel.id](/resources/channel#channel-object)}/chat</span>

Get a channel by ID. Returns a [channel](#channel-object) object wrapped in `metadata.channel` on success.

| Field     | Type                          | Description                         | Required |
|-----------|-------------------------------|-------------------------------------|----------|
| route     | string (metadata path)        | the channel to get the message from | true     |

## Get Channel Messages
<span class="http-verb">GET</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/messages</span>

Returns the messages for a channel. Only available if the channel is public. Returns an array of [message](/resources/channel#message-object) objects and a boolean `hasPastMessages` detailing if there are messages preceeding this array on success.

###### Query String Params

| Field      | Type              | Description                                               | Required | Default |
|------------|-------------------|-----------------------------------------------------------|----------|---------|
| limit      | integer           | max number of messages to return (0-2^63)\*               | false    | 51      |
| beforeDate | ISO8601 timestamp | filters messages by those made before a certain timestamp | false    | now     |
| afterDate  | ISO8601 timestamp | filters messages by those made after a certain timestamp  | false    | epoch?  |

\* `limit` is interpreted as a bigint value and therefore can be as high as 2^63. Anything over this value will quickly return a 500 error code.

## Get Channel Message
<span class="http-verb">GET</span><span class="http-path">/content/route/metadata?route=//channels/{[channel.id](/resources/channel#channel-object)}/chat?messageId={[message.id](/resources/channel#message-object)}</span>

Get a specific message in the channel. Only available if the message's channel is public. On success, returns a [message](#message-object) object (`metadata.message`) and the message's [channel](#channel-object) object (`metadata.channel`).

###### Query String Params

| Field     | Type                          | Description                         | Required |
|-----------|-------------------------------|-------------------------------------|----------|
| route     | string (metadata path)        | the channel to get the message from | true     |
| messageId | [message id](#message-object) | the message to get                  | true     |

## Get Pinned Messages
<span class="http-verb">GET</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/pins</span>

Returns all pinned messages in the channel as an array of [message](/resources/channel#message-object) objects inside a `message` key.
