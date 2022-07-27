# Users

Users are everywhere. They're in our servers, our DMs, our friends lists.. and in this documentation page!

### User Object

###### User Structure

| Field                  | Type                                                   | Description                                                           |
|------------------------|--------------------------------------------------------|-----------------------------------------------------------------------|
| id                     | [generic id](/reference#generic-object-ids)            | the user's id                                                         |
| name                   | string                                                 | the user's username, not unique across the platform                   |
| type                   | string                                                 | the [type of user](#user-types)                                       |
| subdomain              | ?string                                                | the user's unique profile url                                         |
| aliases                | array                                                  | the linked games on the user's profile                                |
| email                  | ?string                                                | the user's email. null if this is not you                             |
| serviceEmail           | ?string                                                | ?                                                                     |
| profilePicture         | string (url)                                           | the user's avatar url                                                 |
| profilePictureSm       | string (url)                                           | ^                                                                     |
| profilePictureLg       | string (url)                                           | ^                                                                     |
| profilePictureBlur     | string (url)                                           | ^                                                                     |
| profileBannerSm        | ?string (url)                                          | the user's banner url                                                 |
| profileBannerLg        | ?string (url)                                          | ^                                                                     |
| profileBannerBlur      | ?string (url)                                          | ^                                                                     |
| createdAt              | ISO8601 timestamp                                      | when the user's account was created                                   |
| joinDate? (deprecated) | ISO8601 timestamp                                      | when the user's account was created. `createdAt` may appear instead   |
| steamId                | ?string                                                | this user's steam id, if linked                                       |
| userStatus             | [user status object](#user-status-object)              | this user's current activity/"status"                                 |
| userPresenceStatus     | integer                                                | this [user's presence](#user-presence) (online, idle, etc)            |
| userTransientStatus    | [transient status object](#transient-status)           | this user's transient status (game, streaming, ?)                     |
| moderationStatus       | ?                                                      | ?                                                                     |
| aboutInfo              | [about info object](#about-info-object)                | this user's bio and tagline                                           |
| lastOnline             | ISO8601 timestamp                                      | when this user was last online                                        |
| stonks?                | integer                                                | number of "stonks" the user has gotten from inviting people           |
| flairInfos?            | array of [flair objects](#flair-object)                | the flairs the user has on their profile                              |
| teams?                 | boolean, array of [teams](/resources/team#team-object) | `false` if the user is not yourself, otherwise an array of your teams |

###### Example User

```json
{
  "id": "EdVMVKR4",
  "name": "shay",
  "type": "user",
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
  "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  // Some profilePicture and banner keys removed for brevity
  "createdAt": "2020-07-27T14:07:28.336Z",
  "steamId": null,
  "userStatus": {
    "content": null,
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
    "tagLine": "computer user"
  },
  "lastOnline": "2021-03-08T00:24:49.144Z",
  "serviceEmail": null,
  "userPresenceStatus": 1,
  "userTransientStatus": null,
  "stonks": 2
}
```

##### User Types

| Type | Description            |
|------|------------------------|
| user | the account is a human |
| bot  | the account is a bot   |

### User Presence

| Value | Meaning           |
|-------|-------------------|
| 1     | online            |
| 2     | idle              |
| 3     | dnd               |
| 4     | offline/invisible |

### Transient Status Object

| Field           | Type                                | Description                              |
|-----------------|-------------------------------------|------------------------------------------|
| id              | integer                             | the game's id (1622-3449)                            |
| gameId          | ?integer                            | (DEPRECATED) the game's id this status is for (see the game ids table below for known valid values) |
| name?           | string                              | (DEPRECATED) the game's name (should be included if gameId is null)                                 |
| type            | string                              | the type of status ("gamepresence", ?)   |
| startedAt       | ISO8601 timestamp                   | (DEPRECATED) when this status started                 |
| guildedClientId | [uuid](/reference#snowflakes-uuids) | (DEPRECATED) the client's id that this status is from |

###### Game IDs
Full list of Games can be found [here](https://github.com/GuildedAPI/docs/blob/main/docs/resources/games.md)  
If you want to use this list in your application, feel free to grab it in [JSON format from our datatables repo](https://git.guildedapi.com/datatables/blob/main/games.json).


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

### Flair Object

Represents a profile flair. Does not include stonks, those can be found at [`user.stonks`](#user-object).

###### Flair Structure

| Field  | Type    | Description                                                            | Known Values |
|--------|---------|------------------------------------------------------------------------|--------------|
| flair  | string  | name of the flair                                                      | `'gil_gang'` |
| amount | integer | quantity of this flair that the user has, probably intended for stonks | |

### Me Object

###### Me Structure

| Field           | Type                                                    | Description                                                              |
|-----------------|---------------------------------------------------------|--------------------------------------------------------------------------|
| teams           | array of [teams](/resources/team#team-object)           | a list of this user's teams                                              |
| user            | [user](#user-object)                                    | this user                                                                |
| updateMessage   | ?[update message](#update-message-object)               | guilded's update message                                                 |
| customReactions | array of [emojis](/resources/emoji#custom-emoji-object) | a list of emojis this user has access to                                 |
| reactionUsages  | array of [emoji uses](#emoji-use-object)                | a list of how many times a specific emoji has been used by the this user |
| friends         | array of [friends](#friend-object)                      | a list of friends, friend requests and friend requests sent by this user |

### Friend Object

###### Friend Structure

| Field        | Type                                   | Description                                         |
|--------------|----------------------------------------|-----------------------------------------------------|
| friendUserId | [user id](/reference/user#user-object) | id of the user                                      |
| friendStatus | string                                 | the current [status of friendship](#friend-status)  |
| createdAt    | ISO8601 timestamp                      | when the request was sent/when request got accepted |

###### Example Friend

```json
{
    "friendUserId": "R40Mp0Wd",
    "friendStatus": "accepted",
    "createdAt": "2021-01-01T15:00:00.000Z"
}
```

###### Friend Status

| Value     | Meaning                                |
|-----------|----------------------------------------|
| accepted  | the friend request was accepted        |
| requested | this user has sent a friend request    |
| pending   | a friend request was sent to this user |

### Emoji Use Object

###### Emoji Use Structure

| Field | Type    | Description                            |
|-------|---------|----------------------------------------|
| id    | integer | ID of the emoji                        |
| total | integer | how many times the emoji has been used |

###### Example Emoji Use

```json
{
    "id": 90025666,
    "total": 984
}
```

### Update Message Object

###### Update Message Structure

| Field         | Type              | Description                                 |
|---------------|-------------------|---------------------------------------------|
| id            | unsigned integer  | ID of the update message                    |
| content       | message content   | the content of the update message           |
| createdAt     | ISO8601 timestamp | when the post was created                   |
| updatedAt     | ISO8601 timestamp | when the post was edited                    |
| publishedAt   | ISO8601 timestamp | when the post was published to everyone     |
| ctaButtonText | string            | the text of the footer button               |
| ctaButtonLink | url               | the link of the footer button               |

## Get Current User
<span class="http-verb">GET</span><span class="http-path">/me</span>

Returns the [me](#me-object) object of the requester's account.

###### Query Params

| Field   | Type    | Description                                  | Required | Default Value |
|---------|---------|----------------------------------------------|----------|---------------|
| isLogin | boolean | whether or not you are logging in (probably) | false    | false         |

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
    When posting this to the API, this should go inside of a dictionary passed to [Modify Current User](#modify-current-user).

| Field   | Type   | Description                                                    |
|---------|--------|----------------------------------------------------------------|
| bio?    | string | the user's bio ("About")                                       |
| tagLine | string | the user's tagline (appears under their name on their profile) |

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

| Field | Type  | Description                                                                                          |
|-------|-------|------------------------------------------------------------------------------------------------------|
| users | array | array of `{"id": user.id}`, where [user.id](#user-object) is a user's id to create a DM channel with |

## Get User Posts
<span class="http-verb">GET</span><span class="http-path">/users/{[user.id](#user-object)}/posts</span>

Get the list of posts this user has on their profile.

###### Query Params

| Field    | Type    | Description                           | Required | Default Value |
|----------|---------|---------------------------------------|----------|---------------|
| maxPosts | integer | the maximum amount of posts to return | false    | 7             |
| offset   | integer | ?                                     | false    | 0             |

## Set User Transient Status
<span class="http-verb">POST</span><span class="http-path">/users/me/status/transient</span>

Set your transient status. Returns your new [transient status object](#transient-status-object) on success.

###### JSON Params

| Field  | Type     | Description                                                                         |
|--------|----------|-------------------------------------------------------------------------------------|
| id     | integer  | ?                                                                                   |
| gameId | ?integer | the game's id this status is for (see [game ids](#game-ids) for known valid values) |
| type   | string   | the type of transient status. should be one of `gamepresence`, ?                    |

## Delete User Transient Status
<span class="http-verb">DELETE</span><span class="http-path">/users/me/status/transient</span>

Remove your transient status.

## Get Referral Statistics
<span class="http-verb">GET</span><span class="http-path">/users/me/referrals</span>

Get your statistics for "stonks". From the client:

> For every 5 people you invite, you will get an exclusive Stonks flair to show off to everyone.

"Stonks" don't do anything on their own, and are simply a profile decoration.

###### Response Structure

| Field                | Type    | Description                                                                                        |
|----------------------|---------|----------------------------------------------------------------------------------------------------|
| referrals            | integer | how many people have used your invite link (https://guilded.gg?r={user.id}) to sign up for guilded |
| stonks               | integer | how many "stonks" you currently have                                                               |
| requiredForNextStonk | integer | how many more people need to sign up with your invite link for you to gain another "stonk"         |

###### Example Response

```json
{
  "referrals": 10,
  "stonks": 2,
  "requiredForNextStonk": 5
}
```
