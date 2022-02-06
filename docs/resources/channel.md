# Channels

### Channel Object

Represents a team or DM channel.

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

###### DM Channel Structure

| Field               | Type               | Description                                                        |
|---------------------|--------------------|--------------------------------------------------------------------|
| id                  | uuid               | the id of this channel                                             |
| type                | string             | the [type of channel](#channel-types)                              |
| name                | ?string            | the name of this channel (group dms?)                              |
| description         | ?string            | the description of this channel (group dms???)                     |
| users               | array              | list of [users](/resources/user#user-object) in this dm (including yourself) |
| createdAt           | ISO8601 timestamp  | when this channel was created                                      |
| createdBy           | [user id](/resources/user#user-object) | the user's id who created this dm                        |
| updatedAt           | ISO8601 timestamp  | when this channel was last updated                                 |
| contentType         | string             | the [content type of channel](#channel-content-types). should always be 'chat' |
| archivedAt          | ?ISO8601 timestamp | when this channel was archived                                      |
| autoArchiveAt       | ?ISO8601 timestamp | when this channel will automatically be archived                    |
| archivedBy          | ?[user id](/resources/user#user-object) | the user's id who archived this channel                 |
| parentChannelId     | ?uuid              | this channel's parent id                                            |
| deletedAt           | ?ISO8601 timestamp | when this channel was deleted                                       |
| createdByWebhookId  | ?[webhook id](/resources/webhook#webhook-object) | the webhook's id that created this channel  |
| archivedByWebhookId | ?[webhook id](/resources/webhook#webhook-object) | the webhook's id that archived this channel |
| dmType              | string             | the dm type of channel. known values: 'Default'                     |
| ownerId             | [user id](/resources/user#user-object) | the user's id who owns this dm                  |
| voiceParticipants?  | array              | list of [users](/resources/user#user-object) in the dm's voice call |

###### Channel Types

| Type | Description                    |
|------|--------------------------------|
| Team | a channel within a server      |
| DM   | a direct message between users |

###### Channel Content Types

| Type  | Description           |
|-------|-----------------------|
| chat  | a chat (text) channel |
| voice | a voice channel       |
| forum | a forum channel       |
| doc   | a docs channel        |

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

###### Example DM Channel

```json
{
  "id": "f695a133-3812-4db4-b733-64a1efdf8fd2",
  "type": "DM",
  "name": null,
  "description": null,
  "users": [
    {
      "id": "EdVMVKR4",
      "name": "shay",
      "badges": null,
      "nickname": null,
      "addedAt": "2021-03-18T19:46:18.536991+00:00",
      "isOwner": true,
      "channelId": "f695a133-3812-4db4-b733-64a1efdf8fd2",
      "removedAt": null,
      "userStatus": {
        "content": {
          "object": "value",
          "document": {
            "data": {},
            "nodes": [
              {
                "data": {},
                "type": "paragraph",
                "nodes": [
                  {
                    "leaves": [
                      {
                        "text": "g.gg/guilded-api",
                        "marks": [],
                        "object": "leaf"
                      }
                    ],
                    "object": "text"
                  },
                  {
                    "data": {
                      "reaction": {
                        "id": 294745,
                        "customReaction": {
                          "id": 294745,
                          "name": "blobcouple",
                          "png": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/448cff53087b93e72298bae9d47708f1-Full.webp?w=120&h=120",
                          "webp": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/448cff53087b93e72298bae9d47708f1-Full.webp?w=120&h=120",
                          "apng": null
                        },
                        "customReactionId": 294745
                      }
                    },
                    "type": "reaction",
                    "nodes": [
                      {
                        "leaves": [
                          {
                            "text": ":blobcouple:",
                            "marks": [],
                            "object": "leaf"
                          }
                        ],
                        "object": "text"
                      }
                    ],
                    "object": "inline"
                  },
                  {
                    "leaves": [
                      {
                        "text": "g.gg/blob-emoji ",
                        "marks": [],
                        "object": "leaf"
                      }
                    ],
                    "object": "text"
                  },
                  {
                    "data": {
                      "reaction": {
                        "id": 294677,
                        "customReaction": {
                          "id": 294677,
                          "name": "blobpats",
                          "png": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/ff03be6efe491bc934b1c217d70da9ce-Full.webp?w=120&h=120",
                          "webp": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/ff03be6efe491bc934b1c217d70da9ce-Full.webp?w=120&h=120",
                          "apng": null
                        },
                        "customReactionId": 294677
                      }
                    },
                    "type": "reaction",
                    "nodes": [
                      {
                        "leaves": [
                          {
                            "text": ":blobpats:",
                            "marks": [],
                            "object": "leaf"
                          }
                        ],
                        "object": "text"
                      }
                    ],
                    "object": "inline"
                  }
                ],
                "object": "block"
              }
            ],
            "object": "document"
          }
        },
        "customReactionId": 294765,
        "customReaction": {
          "id": 294765,
          "name": "ablobwobwork",
          "png": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/d7cf013a4d01460a81186e65f1f8c12a-Full.webp?w=120&h=120&ia=1",
          "webp": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/d7cf013a4d01460a81186e65f1f8c12a-Full.webp?w=120&h=120&ia=1",
          "apng": null
        }
      },
      "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
      "profileBannerBlur": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserBanner/acaa9d0f78dd8cdd93f3ce44d14c0260-Hero.png?w=1500&h=500",
      "userPresenceStatus": 1
    },
    {
      "id": "2d2Wg8Pm",
      "name": "GAPI Testing Account",
      "badges": null,
      "nickname": null,
      "addedAt": "2021-03-18T19:46:18.536991+00:00",
      "isOwner": false,
      "channelId": "f695a133-3812-4db4-b733-64a1efdf8fd2",
      "removedAt": null,
      "userStatus": {
        "content": null,
        "customReactionId": null
      },
      "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/ca66e6624c1e6706c7c200b3759572cf-Large.png?w=450&h=450",
      "profileBannerBlur": null,
      "userPresenceStatus": "2"
    }
  ],
  "DMType": "Default",
  "lastMessage": {
    "id": "a70946a5-5053-44b0-8573-fd652052426e",
    "content": {
      "object": "value",
      "document": {
        "data": {},
        "nodes": [
          {
            "data": {},
            "type": "paragraph",
            "nodes": [
              {
                "leaves": [
                  {
                    "text": "hey docs! ",
                    "marks": [],
                    "object": "leaf"
                  }
                ],
                "object": "text"
              },
              {
                "data": {
                  "reaction": {
                    "id": 90001815,
                    "customReactionId": 90001815,
                    "customReaction": {
                      "id": 90001815,
                      "name": "tada",
                      "png": null,
                      "webp": null,
                      "apng": null
                    }
                  }
                },
                "type": "reaction",
                "nodes": [
                  {
                    "leaves": [
                      {
                        "text": ":tada:",
                        "marks": [],
                        "object": "leaf"
                      }
                    ],
                    "object": "text"
                  }
                ],
                "object": "inline"
              },
              {
                "leaves": [
                  {
                    "text": " ",
                    "marks": [],
                    "object": "leaf"
                  }
                ],
                "object": "text"
              }
            ],
            "object": "block"
          }
        ],
        "object": "document"
      }
    },
    "type": "default",
    "createdBy": "EdVMVKR4",
    "createdAt": "2021-03-18T19:46:25.697Z",
    "editedAt": null,
    "deletedAt": null,
    "webhookId": null,
    "botId": null
  },
  "createdAt": "2021-03-18T19:46:18.536Z",
  "createdBy": "EdVMVKR4",
  "updatedAt": "2021-03-18T19:46:18.536Z",
  "contentType": "chat",
  "archivedAt": null,
  "parentChannelId": null,
  "autoArchiveAt": null,
  "deletedAt": null,
  "archivedBy": null,
  "createdByWebhookId": null,
  "archivedByWebhookId": null,
  "dmType": "Default",
  "ownerId": "EdVMVKR4",
  "voiceParticipants": []
}
```

![example dm channel message](/images/example_dm_channel_message.png)

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
| type       | string                                               | type of message. usually `default`                                          |

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
<span class="http-verb">GET</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}</span>

Get a channel by ID. Returns a [channel object](#channel-object).

## Delete/Close Channel
<span class="http-verb">DELETE</span><span class="http-path">/teams/{team.id}/groups/{group.id}/channels/{[channel.id](/resources/channel#channel-object)}</span>

Delete a channel. Deleting a category does not delete its child channels; they will have their `categoryId` nullified.

## Get Channel Messages
<span class="http-verb">GET</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/messages</span>

Returns the messages for a channel. Does not require authentication. Returns an array of [message](/resources/channel#message-object) objects and a boolean `hasPastMessages` detailing if there are messages preceeding this array on success.

Query string parameter `limit` has been tested up to 50,000.

###### Query String Params

| Field  | Type    | Description                              | Required | Default |
|--------|---------|------------------------------------------|----------|---------|
| limit  | integer | max number of messages to return (0-???) | false    | 51      |

## Get Channel Message
<span class="http-verb">GET</span><span class="http-path">/content/route/metadata?route=//channels/{[channel.id](/resources/channel#channel-object)}/chat?messageId={[message.id](/resources/channel#message-object)}</span>

Get a specific message in the channel. Returns a [message object](#message-object) on success.

###### Query String Params

| Field     | Type                          | Description                         | Required |
|-----------|-------------------------------|-------------------------------------|----------|
| route     | string (metadata path)        | the channel to get the message from | true     |
| messageId | [message id](#message-object) | the message to get                  | true     |

## Send Message
<span class="http-verb">POST</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/messages</span>

Post a message to a team channel or DM channel. Returns a [message](#message-object) object. Fires a [Message Create](/topics/gateway#ChatMessageCreated) Gateway event.

The actual uploading of attachments is a separate endpoint. See [how to upload files](/reference#upload-a-file). Use a dynamicMediaTypeId of `ContentMedia`.

###### JSON Params

| Field         | Type                                          | Description                                                                   |
|---------------|-----------------------------------------------|-------------------------------------------------------------------------------|
| messageId     | [uuid](/reference#snowflakes-uuids)           | the id for this message                                                       |
| content       | message content (see below)                   | the message contents (up to 4,000 characters of text)                         |
| repliesToIds? | array of [uuids](/reference#snowflakes-uuids) | up to 5 messages ids to reply to                                              |
| isSilent?     | boolean                                       | this reply should notify the authors of the messages it is replying to        |
| isPrivate?    | boolean                                       | this reply should be "private" (only visible to people involved in the reply) |

!!! note
    Setting both `isSilent` and `isPrivate` to `true` (a private reply with no mention) will not send the reply to the author of the message(s).

###### Example Request Body

```json
{
  "messageId": "b943384a-d951-4323-8e26-8e3e6b7c431a",
  "repliesToIds": ["6354da62-09b9-11ec-9907-6245b4f631c5", "71fd7df4-09b9-11ec-8127-6245b4f631c5"],
  "isPrivate": true,
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

## Edit Message
<span class="http-verb">PUT</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/messages/{[message.id](/resources/channel#message-object)}</span>

Edit a previously sent message.

!!! note
    Due to how the content structure works, sending new content will entirely replace the old content. This means that in order to keep old nodes, you will have to send them with the new request.

###### JSON Params

!!! note
    This endpoint takes the same structure of `content` as [Send Message](#send-message), so for brevity, its elaboration is not included here.

| Field   | Type            | Description                                               |
|---------|-----------------|-----------------------------------------------------------|
| content | message content | the new message contents (up to 4,000 characters of text) |

## Delete Message
<span class="http-verb">DELETE</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/messages/{[message.id](/resources/channel#message-object)}</span>

Delete a message. Fires a [Message Delete](/topics/gateway#ChatMessageDeleted) Gateway event.

## Add Reaction
<span class="http-verb">POST</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/messages/{[message.id](/resources/channel#message-object)}/reactions/{reaction.id}</span>

React to a message with an emoji. Returns a pretty empty `[null, [], []]` on success.

## Delete Own Reaction
<span class="http-verb">DELETE</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/messages/{[message.id](/resources/channel#message-object)}/reactions/{reaction.id}</span>

Delete your reaction to a message. Returns a pretty empty `[[], []]` on success.

## Get Pinned Messages
<span class="http-verb">GET</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/pins</span>

Returns all pinned messages in the channel as an array of [message](/resources/channel#message-object) objects inside a `message` key.

## Pin Message
<span class="http-verb">POST</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/pins</span>

!!! warning
    The max pinned messages is ???.

Pin a message in a channel.

###### JSON Params

| Field     | Type                          | Description             |
|-----------|-------------------------------|-------------------------|
| messageId | [message id](#message-object) | the message's id to pin | 

## Unpin Message
<span class="http-verb">DELETE</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/pins/{[message.id](/resources/channel#message-object)}</span>

Delete a pinned message in a channel.

## Create Thread
<span class="http-verb">POST</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/threads</span>

Create a new thread in a channel. Returns the created thread object on success.

###### JSON Params

| Field                | Type                                | Description                                                                                                                         |
|----------------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| name                 | string                              | the name that this thread should have                                                                                               |
| message              | [message](#example-request-body)    | the message to send when creating this thread                                                                                       |
| channelId            | [uuid](/reference#snowflakes-uuids) | the id that this thread will have                                                                                                   |
| threadMessageId      | [uuid](/reference#snowflakes-uuids) | ?                                                                                                                                   |
| initialThreadMessage | [message](#message-object)          | the message that this thread is starting on                                                                                         |
| contentType          | string                              | the [channel content type](#channel-content-types) that this thread should be. can probably only be 'chat', but try others and see! |
| confirmed            | boolean                             | ?                                                                                                                                   |

## Leave Thread
<span class="http-verb">DELETE</span><span class="http-path">/users/{[user.id](/resources/user#user-object)}/channels/{[thread.id](/resources/channel#channel-object)}</span>

!!! info
    `user.id` should be the current user's ID. Attempting to use this endpoint to remove someone else from a thread results in a 403 Forbidden response.

Leave a thread. Guilded will also stop sending you notifications from the thread.

## Archive Team Thread
<span class="http-verb">PUT</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/groups/{[group.id](/resources/group#group-object)}/channels/{[thread.id](/resources/channel#channel-object)}/archive</span>

!!! info
    `group.id` can be `undefined` if this is the base group.

Archive a thread. The thread can be restored at any time with [Restore Thread](#restore-thread).

## Restore Team Thread
<span class="http-verb">PUT</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/groups/{[group.id](/resources/group#group-object)}/channels/{[thread.id](/resources/channel#channel-object)}/restore</span>

!!! info
    `group.id` can be `undefined` if this is the base group.

Restore a thread after it has been archived. It can be archived again with [Archive Thread](#archive-thread)

## Delete Team Thread
<span class="http-verb">DELETE</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/groups/{[group.id](/resources/group#group-object)}/channels/{[thread.id](/resources/channel#channel-object)}</span>

!!! info
    `group.id` can be `undefined` if this is the base group.

!!! info
    This endpoint can also be used to delete normal channels.

Delete a thread in a channel.

## Post Team Announcement
<span class="http-verb">POST</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/announcements</span>

!!! warning
    This posts an announcement to no specific channel. To post to a specific channel, see [Post Announcement](#post-announcement)

Post an announcement to a team's overview page.

###### JSON Params

| Field   | Type                 | Description                                                                        |
|---------|----------------------|------------------------------------------------------------------------------------|
| title   | string               | the announcement's title                                                           |
| content | announcement content | the announcement's content (similar to [message content](#message-content-object)) |

## Post Announcement
<span class="http-verb">POST</span><span class="http-path">/channels/{[channel.id](/resources/channel#channel-object)}/announcements</span>

Post an announcement to an announcement channel.

###### JSON Params

| Field                 | Type                 | Description                                                                        |
|-----------------------|----------------------|------------------------------------------------------------------------------------|
| title                 | string               | the announcement's title                                                           |
| content               | announcement content | the announcement's content (similar to [message content](#message-content-object)) |
| dontSendNotifications | boolean              | whether to "notify all users" when the announcement is posted                      |

