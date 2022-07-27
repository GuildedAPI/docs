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

### Emoji Pack Object

!!! info
    Emoji packs can include a maximum of 50 emojis.

###### Emoji Pack Structure

| Field  | Type                    | Description                                                                                  |
|--------|-------------------------|----------------------------------------------------------------------------------------------|
| name   | string                  | name of the emoji pack                                                                       |
| author | string                  | author of the emoji pack                                                                     |
| emojis | array of partial emojis | list of emojis to import. each item should include a `name` (alphanumberic only) and a `url` |

###### Example Emoji Pack

```json
{
  "name": "Guilded emojis",
  "author": "emoji.gg",
  "emotes": [
    {
      "name": "gil_wut",
      "url": "https://emoji.gg/assets/emoji/6017_gil_wut.png"
    },
    {
      "name": "gil_woah",
      "url": "https://emoji.gg/assets/emoji/9837_gil_woah.png"
    },
    {
      "name": "gil_thumb",
      "url": "https://emoji.gg/assets/emoji/8264_gil_thumb.png"
    },
    {
      "name": "gil_think",
      "url": "https://emoji.gg/assets/emoji/2294_gil_think.png"
    },
    {
      "name": "gil_sweat",
      "url": "https://emoji.gg/assets/emoji/5722_gil_sweat.png"
    },
    {
      "name": "gil_smug",
      "url": "https://emoji.gg/assets/emoji/7756_gil_smug.png"
    },
    {
      "name": "gil_shrug",
      "url": "https://emoji.gg/assets/emoji/3769_gil_shrug.png"
    },
    {
      "name": "gil_scared",
      "url": "https://emoji.gg/assets/emoji/2002_gil_scared.png"
    },
    {
      "name": "gil_salute",
      "url": "https://emoji.gg/assets/emoji/7259_gil_salute.png"
    },
    {
      "name": "gil_ree",
      "url": "https://emoji.gg/assets/emoji/6400_gil_ree.png"
    },
    {
      "name": "gil_stare",
      "url": "https://emoji.gg/assets/emoji/9969_gil_stare.png"
    },
    {
      "name": "gil_notlikethis",
      "url": "https://emoji.gg/assets/emoji/1616_gil_notlikethis.png"
    },
    {
      "name": "gil_lul",
      "url": "https://emoji.gg/assets/emoji/5606_gil_lul.png"
    },
    {
      "name": "gil_lol",
      "url": "https://emoji.gg/assets/emoji/7890_gil_lol.png"
    },
    {
      "name": "gil_wave",
      "url": "https://emoji.gg/assets/emoji/2469_gil_wave.png"
    },
    {
      "name": "gil_heh",
      "url": "https://emoji.gg/assets/emoji/6192_gil_heh.png"
    },
    {
      "name": "guilded",
      "url": "https://emoji.gg/assets/emoji/7391_guilded.png"
    },
    {
      "name": "gil_facepalm",
      "url": "https://emoji.gg/assets/emoji/7256_gil_facepalm.png"
    },
    {
      "name": "gil_eyes",
      "url": "https://emoji.gg/assets/emoji/6197_gil_eyes.png"
    },
    {
      "name": "gil_doge",
      "url": "https://emoji.gg/assets/emoji/1037_gil_doge.png"
    },
    {
      "name": "gil_deal_with_it",
      "url": "https://emoji.gg/assets/emoji/2315_gil_deal_with_it.png"
    },
    {
      "name": "gil_dab",
      "url": "https://emoji.gg/assets/emoji/5387_gil_dab.png"
    },
    {
      "name": "gil_cry",
      "url": "https://emoji.gg/assets/emoji/4846_gil_cry.png"
    },
    {
      "name": "cheer_gil",
      "url": "https://emoji.gg/assets/emoji/8188_cheer_gil.png"
    }
  ]
}
```

## Get Team Emojis
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/customReactions</span>

Returns a list of [emoji](#custom-emoji-object) objects for the given team.

###### Query Params

| Field            | Type                                   | Description | Required | Default |
|------------------|----------------------------------------|-------------|----------|---------|
| maxItems         | integer                                | maximum number of emojis to return                        | | |
| when[upperValue] | ISO8601 timestamp                      | return emojis created after this time                     | false | |
| when[lowerValue] | ISO8601 timestamp                      | return emojis created before this time                    | false | |
| createdBy        | [user id](/resources/user#user-object) | return emojis created by this user's id                   | false | |
| searchTerm       | string                                 | search emoji names with a string                          | false | |
| beforeId         | [emoji id](#custom-emoji-object)       | return emojis created before this emoji was created (potentially something to do with pagination) | false | |

## Emoji Packs

This section has been moved to [Media Endpoints](/topics/media#get-custom-emoji-pack).

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
