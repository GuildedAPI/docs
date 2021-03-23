# Custom Reactions/Emojis

A "custom reaction" refers to a custom server emoji in Guilded. However, since this name is silly and confusing, this page will refer to this concept as "emojis", "custom emoijs", or "team emojis".

### Custom Emoji Object

###### Custom Emoji Structure

| Field           | Type                                    | Description                                                  |
|-----------------|-----------------------------------------|--------------------------------------------------------------|
| id              | integer                                 | emoji's id (cannot be used for forming a cdn url)            |
| name            | string                                  | emoji's name                                                 |
| createdBy       | [user id](/resources/user#user-object)  | user's id that created this emoji                            |
| createdAt       | ISO8601 timestamp                       | when this emoji was created                                  |
| png             | string                                  | aws url to this emoji's image file                           |
| webp            | string                                  | aws url to this emoji's image file                           |
| apng            | string                                  | aws url to this emoji's image file                           |
| aliases         | array of strings                        | an array of aliases this emoji can be used with              |
| teamId          | [team id](/resources/team#team-object)  | team's id that this emoji is from                            |
| isDeleted       | boolean                                 | whether this emoji has been deleted                          |
| discordEmojiId  | ?snowflake                              | the discord emoji's id this emoji corresponds to             |
| discordSyncedAt | ?ISO8601 timestamp                      | when this emoji was last synced with its discord counterpart |

###### Emoji Example

```json
{
  "id": 239839,
  "name": "beepboop",
  "png": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/36b263b547cf38de0d55ef01e1ab55ab-Full.webp?w=120&h=120",
  "webp": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/36b263b547cf38de0d55ef01e1ab55ab-Full.webp?w=120&h=120",
  "apng": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/36b263b547cf38de0d55ef01e1ab55ab-Full.webp?w=120&h=120",
  "aliases": [],
  "createdAt": "2020-07-28T06:48:56.525Z",
  "createdBy": "EdVMVKR4",
  "teamId": "NRgqXnPE",
  "isDeleted": false,
  "discordEmojiId": null,
  "discordSyncedAt": "2020-07-28T06:49:03.668Z"
}
```

## Get Team Emojis
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/customReactions</span>

Returns a list of [emoji](#custom-emoji-object) objects for the given team.

###### Query Params

| Field            | Type                                   | Description | Required | Default |
|------------------|----------------------------------------|-------------|----------|---------|
| maxItems         | integer                                | maximum number of emojis to return                  | | |
| when[upperValue] | ISO8601 timestamp                      | return emojis created after this time                     | false | |
| when[lowerValue] | ISO8601 timestamp                      | return emojis created before this time                    | false | |
| createdBy        | [user id](/resources/user#user-object) | return emojis created by this user's id             | | |
| searchTerm       | string                                 | search emoji names with a string                    | | |
| beforeId         | [emoji id](#custom-emoji-object)       | return emojis created before this emoji was created (potentially something to do with pagination) | false | |

## Create Team Emoji
<span class="http-verb">POST</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/customReactions</span>

Create a new emoji for the team. Returns the new [emoji](#custom-emoji-object) object on success.

!!! info
    You can request this endpoint without any image URL and recieve an HTTP 200 response, but no emoji will actually show up or be usable in the team.

###### JSON Params

| Field | Type   | Description                                                                |
|-------|--------|----------------------------------------------------------------------------|
| name  | string | name of the emoji                                                          |
| png?  | string | url to the image you uploaded ([CustomReaction](/reference#upload-a-file)) |
| webp? | string | url to the image you uploaded ([CustomReaction](/reference#upload-a-file)) |
| apng? | string | url to the image you uploaded ([CustomReaction](/reference#upload-a-file)) |

## Modify Team Emoji
<span class="http-verb">PATCH</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/customReactions/{[emoji.id](#custom-emoji-object)}</span>

Modify the given emoji.

###### JSON Params

| Field | Type   | Description       |
|-------|--------|-------------------|
| name  | string | name of the emoji |

## Delete Team Emoji
<span class="http-verb">DELETE</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/customReactions/{[emoji.id](#custom-emoji-object)}</span>

Delete the given emoji.

## Get Team Emoji Creators
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/customReactionCreators</span>

Get a list of users who have created emojis in this team.

!!! info
    This will return each user once. To see how many emojis a specific user has uploaded, consider filtering the response from [Get Team Emojis](#get-team-emojis)

###### Example Response

```json
[
  {"userId": "EdVMVKR4"},
  {"userId": "lAeMagzA"}
]
```
