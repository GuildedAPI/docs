# Users

Users are everywhere. They're in our servers, our DMs, our friends lists.. and in this documentation page!

### User Status Object

###### User Status Structure

| Field            | Type                                                       | Description                                   |
|------------------|------------------------------------------------------------|-----------------------------------------------|
| content          | ?[user status content object](#user-status-content-object) | the actual content of the status              |
| customReactionId | ?integer                                                   | the emoji's id that goes alongside the status |
| customReaction?  | [emoji object](/resources/team#team-emoji-object)          | the emoji that goes alongside the status      |

###### Example User Status

```json
{
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
}
```

![example user status in the client](/images/example_status.png)

### User Status Content Object

Status content is an example of "stacked" content, much like message data. It's pretty difficult to portray its structure in a table, so I've simply not done that. The useful part of the object is the `document.nodes[0].nodes` list. Refer to [this example](#example-user-status) and parse it as you will.

### User Object

###### User Structure

| Field               | Type                                        | Description                                                |
|---------------------|---------------------------------------------|------------------------------------------------------------|
| id                  | [generic id](/reference#generic-object-ids) | the user's id                                              |
| name                | string                                      | the user's username, not unique across the platform        |
| subdomain           | ?string                                     | the user's unique profile url                              |
| aliases             | array                                       | the linked games on the user's profile                     |
| email               | ?string                                     | the user's email. null if this is not you                  |
| serviceEmail        | ?string                                     | ?                                                          |
| profilePicture      | string (url)                                | the user's avatar url                                      |
| profilePictureSm    | string (url)                                | ^                                                          |
| profilePictureLg    | string (url)                                | ^                                                          |
| profilePictureBlur  | string (url)                                | ^                                                          |
| profileBannerSm     | ?string (url)                               | the user's banner url                                      |
| profileBannerLg     | ?string (url)                               | ^                                                          |
| profileBannerBlur   | ?string (url)                               | ^                                                          |
| joinDate            | ISO8601 timestamp                           | when this user's account was created                       |
| steamId             | ?string                                     | this user's steam id, if linked                            |
| userStatus          | [user status object](#user-status-object)   | this user's current activity/"status"                      |
| userPresenceStatus  | integer                                     | this [user's presence](#user-presence) (online, idle, etc) |
| userTransientStatus | [transient status object](/resources/user#transient-status) | this user's transient status (game, streaming, ?) |
| moderationStatus    | ?                                           | ?                                                          |
| aboutInfo           | [about info object](#about-info-object)     | this user's bio and tagline                                |
| lastOnline          | ISO8601 timestamp                           | when this user was last online                             |

###### User Presence

Passing a value of <1 or >4 will render with a transparent "presence circle" in the client.

| Value | Meaning           |
|-------|-------------------|
| 1     | online            |
| 2     | idle              |
| 3     | dnd               |
| 4     | offline/invisible |

###### Transient Status

| Field           | Type              | Description                            |
|-----------------|-------------------|----------------------------------------|
| id              | integer           |                                        |
| gameId          | ?integer          |                                        |
| type            | enum              | the type of status ("gamepresence", ?) |
| startedAt       | ISO8601 timestamp | when this status started               |
| guildedClientId | uuid              |                                        |

###### Example User

```json
{
  "id": "EdVMVKR4",
  "name": "shay",
  "subdomain": "shayy",
  "aliases": [
    {
      "alias": "shay",
      "discriminator": null,
      "name": "shay",
      "createdAt": "2021-03-03T21:02:19.901444+00:00",
      "userId": "EdVMVKR4",
      "gameId": 220015,
      "socialLinkSource": null,
      "additionalInfo": {},
      "editedAt": "2021-03-03T21:02:19.901444+00:00",
      "socialLinkHandle": null,
      "playerInfo": null
    }
  ],
  "email": null,
  "profilePictureSm": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "profilePictureLg": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "profilePictureBlur": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "profileBannerBlur": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserBanner/acaa9d0f78dd8cdd93f3ce44d14c0260-Hero.png?w=1500&h=500",
  "profileBannerLg": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserBanner/acaa9d0f78dd8cdd93f3ce44d14c0260-Hero.png?w=1500&h=500",
  "profileBannerSm": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserBanner/acaa9d0f78dd8cdd93f3ce44d14c0260-Hero.png?w=1500&h=500",
  "joinDate": "2020-07-27T14:07:28.336Z",
  "steamId": null,
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
  "moderationStatus": null,
  "aboutInfo": {
    "bio": "i own the guilded API server, go join for bot discussion, libraries, and general programming whatnot guilded.gg/guilded-api",
    "tagLine": "computer user"
  },
  "lastOnline": "2021-03-08T00:24:49.144Z",
  "serviceEmail": null,
  "userPresenceStatus": 1,
  "userTransientStatus": null
}
```

## Get Current User
<span class="http-verb">GET</span><span class="http-path">/me</span>

Returns the [user](#user-object) object of the requester's account.

## Get User
<span class="http-verb">GET</span><span class="http-path">/users/{[user.id](#user-object)}</span>

Returns a [user](#user-object) object for a given user ID.

## Modify Current User
<span class="http-verb">PUT</span><span class="http-path">/users/{[user.id](#user-object)}/profilev2</span>

Modify the requester's user account settings. Returns a [user](#user-object) object on success.

!!! info
    All parameters to this endpoint are optional.

###### JSON Params

| Field     | Type                                  | Description                           |
|-----------|---------------------------------------|---------------------------------------|
| name      | string                                | new username                          |
| avatar    | string                                | new avatar from url                   |
| subdomain | string                                | new subdomain (profile url/slug)      |
| aboutInfo | [aboutInfo object](#aboutInfo-params) | new data about your profile           |

###### aboutInfo Params

!!! info
    All parameters here are nullable. When posting this to the API, this should go inside of a dictionary passed to [Modify Current User](#modify-current-user).

| Field   | Type   | Description                                                  |
|---------|--------|--------------------------------------------------------------|
| bio     | string | the user's bio ("About")                                     |
| tagLine | string | the user's tagline (appears under their name on the profile) |

## Leave Team
<span class="http-verb">DELETE</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/members/{[user.id](#user-object)}</span>

Leave a team. Returns an empty dictionary on success.

## Get User DMs
<span class="http-verb">GET</span><span class="http-path">/users/{[user.id](#user-object)}/channels</span>

Returns a list of [DM channel](/resources/channel#dm-channel-structure) objects on success.

## Create DM Channel
<span class="http-verb">POST</span><span class="http-path">/users/{[user.id](#user-object)}/channels</span>

!!! info
    `user.id` should be the current user's ID, **not** the user who you are starting a DM with.

!!! warning
    You should not use this endpoint to DM everyone in a server. DMs should generally be initiated by a user action. For notifying a large number of users, consider a role mention.

Returns a `{"channel": dm_channel_object}`, where [dm_channel_object](/resources/channel#dm-channel-structure) is the DM channel that was created.

###### JSON Params

| Field | Type  | Description                                                                                                        |
|-------|-------|--------------------------------------------------------------------------------------------------------------------|
| users | array | list of `{"id": user.id}`, where [user.id](#user-object) is a user's id to create a DM channel with |

###### JSON Params

| Field         | Type             | Description                                                            |
| ------------- | ---------------- | ---------------------------------------------------------------------- |
| access_tokens | array of strings | access tokens of users that have granted your app the `gdm.join` scope |
| nicks         | dict             | a dictionary of user ids to their respective nicknames                 |

## Get User Connections
<span class="http-verb">GET</span><span class="http-path">/users/@me/connections</span>

Returns a list of [connection](#connection-object) objects. Requires the `connections` OAuth2 scope.
