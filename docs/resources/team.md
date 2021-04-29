# Teams

The meat of communication on Guilded. They are referred to as "servers" in the UI, and sometimes "guilds".

### Team Object

###### Team Structure

| Field          | Type                                         | Description                                                        |
|----------------|----------------------------------------------|--------------------------------------------------------------------|
| id             | [generic id](/reference#generic-object-ids)  | team id                                                            |
| name           | string                                       | team name                                                          |
| subdomain      | ?string                                      | custom "url" of the team. equivalent to vanity_url_code on discord |
| bio            | ?string                                      | team bio                                                           |
| profilePicture | ?string                                      | team icon url                                                      |
| ownerId        | [user id](/reference#generic-object-ids)     | id of owner                                                        |
| members?       | array of [members](#team-member-object)      | the members in this team                                           |
| bots?          | array of flow-bots                           | the flow-bots in this team                                         |
| webhooks?      | array of [webhooks](/resources/webhook#webhook-object) | the webhooks in this team                                |

###### Example Team

Refer to [Get Team](#get-team)'s example response

### Team Member Object

###### Team Member Structure

| Field               | Type                                                        | Description                                                               |
|---------------------|-------------------------------------------------------------|---------------------------------------------------------------------------|
| id                  | [generic id](/reference#generic-object-ids)                 | the user's id                                                             |
| name                | string                                                      | the user's username, not unique across the platform                       |
| nickname            | string                                                      | the member's team-specific nickname                                       |
| badges              | ?array                                                      | the badges that this member has ("GuildedStaff", "PartnerProgram", ?)     |
| membershipRole      | string                                                      | ?                                                                         |
| profilePicture      | string (url)                                                | the user's avatar url                                                     |
| profileBannerBlur   | ?string (url)                                               | the user's banner url                                                     |
| joinDate            | ISO8601 timestamp                                           | when this member joined their team                                        |
| userStatus          | [user status object](/resources/user#user-status-object)    | this user's current activity/"status"                                     |
| userPresenceStatus  | integer                                                     | this [user's presence](/resources/user#user-presence) (online, idle, etc) |
| userTransientStatus | [transient status object](/resources/user#transient-status) | this user's transient status (game, streaming, ?)                         |
| aliases             | array                                                       | the linked games on the user's profile                                    |
| lastOnline          | ISO8601 timestamp                                           | when the user was last online                                             |
| roleIds             | array of integer                                            | the roles' ids that the member has                                        |
| subscriptionType    | ?integer(?)                                                 | guilded gold-related value                                                |
| socialLinks         | array of dictionaries                                       | each item has `type` - the platform, `handle` - the user's username on that platform, and `additionalInfo` - unknown |
| teamXp              | integer                                                     | how much xp the member has in this team                                   |

###### Example Team Member

```json
{
  "id": "EdVMVKR4",
  "name": "shay",
  "nickname": null,
  "badges": null,
  "joinDate": "2020-07-31T18:10:35.302Z",
  "membershipRole": "admin",
  "lastOnline": "2021-03-07T05:22:09.597Z",
  "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "profileBannerBlur": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserBanner/acaa9d0f78dd8cdd93f3ce44d14c0260-Hero.png?w=1500&h=500",
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
  "roleIds": [
    23138883,
    23138891,
    23138936,
    23139118,
    23281097
  ],
  "subscriptionType": null,
  "socialLinks": [
    {
      "type": "patreon",
      "handle": "Shahayhay ",
      "additionalInfo": {}
    },
    {
      "type": "twitter",
      "handle": "GuildedAPI",
      "additionalInfo": {}
    }
  ],
  "aliases": [],
  "userPresenceStatus": 1,
  "userTransientStatus": null,
  "teamXp": 0
}
```

### Ban Object

###### Ban Structure

| Field     | Type                                            | Description                                          |
|-----------|-------------------------------------------------|------------------------------------------------------|
| reason    | string                                          | the reason for the ban (can be empty, won't be null) |
| userId    | [user id](/resources/user#user-object)          | the banned user's id                                 |
| bannedBy  | [user id](/resources/user#user-object)          | the moderator who banned this user                   |
| createdAt | ISO8601 timestamp                               | when this ban was created                            |

###### Example Ban

```json
{
  "reason": "something funny here, idk",
  "userId": "2d2Wg8Pm",
  "bannedBy": "EdVMVKR4",
  "createdAt": "2021-03-13T18:15:53.622Z"
}
```

### Invite Object

###### Invite Structure

| Field     | Type                                        | Description                                                    |
|-----------|---------------------------------------------|----------------------------------------------------------------|
| id        | [generic id](/reference#generic-object-ids) | the invite's id, referred to as an 'invite code' in the client |
| createdAt | ISO8601 timestamp                           | when the invite was created                                    |
| teamId    | [team id](#team-object)                     | the team's id that the invite is for                           |
| invitedBy | [user id](/resources/user#user-object)      | the user's id who created the invite                           |
| userBy    | ?[user id](/resources/user#user-object)     | a user's id who used the invite?                               |
| gameId    | ?integer                                    | the game's id that the invite is for                           |
| useCount  | integer                                     | how many times the invite has been used                        |

###### Example Invite

```json
{
  "id": "XENGAmn2",
  "createdAt": "2021-02-26T22:43:52.380Z",
  "teamId": "4R5q39VR",
  "invitedBy": "EdVMVKR4",
  "usedBy": null,
  "gameId": null,
  "useCount": 0
}
```

## Check Team Name Availability
<span class="http-verb">GET</span><span class="http-path">/dryrun/teamname/{name}</span>

Check the availability of a team name. Returns `{"exists": false}` if the name is available, and `{"exists": true}` if it is not.

## Create Team
<span class="http-verb">POST</span><span class="http-path">/teams</span>

Create a new team. Returns a [team](#team-object) object wrapped in a `team` key on success.

###### JSON Params

| Field    | Type                                   | Description                                                                                         |
|----------|----------------------------------------|-----------------------------------------------------------------------------------------------------|
| teamName | string                                 | name of the team (3-30 characters, [not unique across the platform](#check-team-name-availability)) |
| userId?  | [user id](/resources/user#user-object) | the user's id who is creating this team                                                             |

## Get Team
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](#team-object)}</span>

!!! Warning
    Performing this request on a ~380-member team resulted in a 343 KB file. Expect Guilded to return less information in a single request in the future.

    The inefficiency of Guilded's client-server communication (and thus how much you should expect this change) cannot be overstated. For example, try this request on the [Guilded Official](https://guilded.gg/guilded-official) server (id: wlVr3Ggl) and see how large the resulting request is (**7.6 MB** as of Mar. 9, 2021).

Returns the [team](#team-object) object for the given id.

###### Example Response

!!! Info
    Many list items were redacted for brevity, such as most of the member list (the entire list is returned) and most of the roles (only the base role, which is included twice, and one custom role, are visible now).

```json
{
    "team": {
        "id": "4R5q39VR",
        "name": "Guilded-API",
        "subdomain": "guilded-api",
        "bio": null,
        "status": null,
        "timezone": "America/New York (EST/EDT)",
        "description": "The unofficial resource hub for all API-related projects and development to do with the Guilded API.",
        "type": "community",
        "games": [],
        "characteristics": null,
        "measurements": {
            "numMembers": 424,
            "numFollowers": "21",
            "numRecentMatches": 0,
            "numRecentMatchWins": 0,
            "matchmakingGameRanks": [],
            "numFollowersAndMembers": 445,
            "numMembersAddedInLastDay": 0,
            "numMembersAddedInLastWeek": 15,
            "mostRecentMemberLastOnline": 1615082895464,
            "numMembersAddedInLastMonth": 102,
            "subscriptionMonthsRemaining": null
        },
        "members": [
            {
                "id": "EdVMVKR4",
                "name": "shay",
                "nickname": null,
                "badges": null,
                "joinDate": "2020-07-31T18:10:35.302Z",
                "membershipRole": "admin",
                "lastOnline": "2021-03-07T05:22:09.597Z",
                "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
                "profileBannerBlur": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserBanner/acaa9d0f78dd8cdd93f3ce44d14c0260-Hero.png?w=1500&h=500",
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
                "roleIds": [
                    23138883,
                    23138891,
                    23138936,
                    23139118,
                    23281097
                ],
                "subscriptionType": null,
                "socialLinks": [
                    {
                        "type": "patreon",
                        "handle": "Shahayhay ",
                        "additionalInfo": {}
                    },
                    {
                        "type": "twitter",
                        "handle": "GuildedAPI",
                        "additionalInfo": {}
                    }
                ],
                "aliases": [],
                "userPresenceStatus": 1,
                "userTransientStatus": null,
                "teamXp": 0
            }
        ],
        "memberCount": null,
        "webhooks": [
            {
                "id": "56a9dde0-aba3-4afd-85b8-05cda9942c93",
                "name": "GitHub",
                "channelId": "80bbb4b1-a880-490c-87e9-5c7edf179d5a",
                "teamId": "4R5q39VR",
                "iconUrl": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/3f8e4273b8b9dcacd57379a637a773f4-Large.png?w=450&h=450",
                "createdBy": "EdVMVKR4",
                "createdAt": "2020-08-16T04:12:56.664Z",
                "deletedAt": null
            }
        ],
        "bots": [
            {
                "id": "da26bb64-0c17-422f-bd81-6e2dff6b7daa",
                "name": "bearger",
                "enabled": true,
                "flows": [
                    {
                        "id": "aabc65b8-50ce-42e2-aa0d-c601b1b1ea26",
                        "enabled": true,
                        "error": false,
                        "botId": "da26bb64-0c17-422f-bd81-6e2dff6b7daa",
                        "teamId": "4R5q39VR",
                        "createdBy": "EdVMVKR4",
                        "createdAt": "2020-11-11T05:57:50.811348+00:00",
                        "deletedAt": null,
                        "triggerType": "BotTriggerSendMessageToTeamChannel",
                        "triggerMeta": {
                            "content": "-hello"
                        },
                        "actionType": "SendMessageToTeamChannel",
                        "actionMeta": {
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
                                                            "text": "hello ",
                                                            "marks": [],
                                                            "object": "leaf"
                                                        }
                                                    ],
                                                    "object": "text"
                                                },
                                                {
                                                    "data": {
                                                        "templateParameterId": "BotActionTriggeringUser"
                                                    },
                                                    "type": "template-parameter",
                                                    "nodes": [
                                                        {
                                                            "leaves": [
                                                                {
                                                                    "text": "$TriggeringUser",
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
                                                            "text": "! i'm bearger, one of the first custom bots made with Guilded's public API (though it seems to just be server flows at the moment). ",
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
                            }
                        }
                    }
                ],
                "teamId": "4R5q39VR",
                "iconUrl": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/26584f8d26eeec8396d28f22678f06dc-Large.png?w=450&h=450",
                "createdBy": "EdVMVKR4",
                "createdAt": "2020-11-11T05:57:50.811Z",
                "deletedAt": null
            }
        ],
        "rolesById": {
            "23138883": {
                "id": 23138883,
                "name": "Admin",
                "color": "#8ad1ff",
                "priority": 13,
                "permissions": {
                    "xp": 1,
                    "chat": 119,
                    "docs": 15,
                    "forms": 18,
                    "lists": 63,
                    "media": 15,
                    "voice": 8179,
                    "forums": 123,
                    "general": 31796,
                    "streams": 51,
                    "calendar": 31,
                    "scheduling": 11,
                    "matchmaking": 1,
                    "recruitment": 55,
                    "announcements": 7,
                    "customization": 49
                },
                "isBase": false,
                "teamId": "4R5q39VR",
                "createdAt": "2020-07-31T18:10:35.328621+00:00",
                "updatedAt": "2021-03-06T18:03:16.052954+00:00",
                "isMentionable": false,
                "discordRoleId": null,
                "discordSyncedAt": null,
                "isSelfAssignable": false,
                "isDisplayedSeparately": false
            },
            "23138884": {
                "id": 23138884,
                "name": "Member",
                "color": "#ececee",
                "priority": 1,
                "permissions": {
                    "chat": 115,
                    "docs": 2,
                    "forms": 16,
                    "lists": 2,
                    "media": 2,
                    "voice": 6211,
                    "forums": 67,
                    "streams": 51,
                    "calendar": 2,
                    "scheduling": 3,
                    "announcements": 2,
                    "customization": 16
                },
                "isBase": true,
                "teamId": "4R5q39VR",
                "createdAt": "2020-07-31T18:10:35.350927+00:00",
                "updatedAt": "2021-03-06T18:03:16.052954+00:00",
                "isMentionable": true,
                "discordRoleId": null,
                "discordSyncedAt": null,
                "isSelfAssignable": false,
                "isDisplayedSeparately": true
            },
            "baseRole": {
                "id": 23138884,
                "name": "Member",
                "color": "#ececee",
                "priority": 1,
                "permissions": {
                    "chat": 115,
                    "docs": 2,
                    "forms": 16,
                    "lists": 2,
                    "media": 2,
                    "voice": 6211,
                    "forums": 67,
                    "streams": 51,
                    "calendar": 2,
                    "scheduling": 3,
                    "announcements": 2,
                    "customization": 16
                },
                "isBase": true,
                "teamId": "4R5q39VR",
                "createdAt": "2020-07-31T18:10:35.350927+00:00",
                "updatedAt": "2021-03-06T18:03:16.052954+00:00",
                "isMentionable": true,
                "discordRoleId": null,
                "discordSyncedAt": null,
                "isSelfAssignable": false,
                "isDisplayedSeparately": true
            }
        },
        "rolesVersion": 32,
        "bannerImages": {},
        "createdAt": "2020-07-31T18:10:35.302Z",
        "ownerId": "EdVMVKR4",
        "rankNames": null,
        "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamAvatar/a66e23924a4bc49fbf9242a98d955a7c-Large.png?w=450&h=450",
        "teamDashImage": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamBanner/62d94db23c910fa3a209b5edf2cf7387-Hero.png?w=1067&h=600",
        "additionalInfo": {
            "platform": "native"
        },
        "teamPreferences": null,
        "additionalGameInfo": {},
        "discordGuildId": null,
        "discordServerName": null,
        "customizationInfo": {},
        "socialInfo": {
            "twitter": "@GuildedAPI"
        },
        "homeBannerImageSm": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamBanner/62d94db23c910fa3a209b5edf2cf7387-Hero.png?w=1067&h=600",
        "homeBannerImageMd": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamBanner/62d94db23c910fa3a209b5edf2cf7387-Hero.png?w=1067&h=600",
        "homeBannerImageLg": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamBanner/62d94db23c910fa3a209b5edf2cf7387-Hero.png?w=1067&h=600",
        "insightsInfo": {},
        "alphaInfo": {},
        "isRecruiting": true,
        "isVerified": false,
        "isPublic": true,
        "alwaysShowTeamHome": false,
        "isPro": false,
        "autoSyncDiscordRoles": false,
        "notificationPreference": null,
        "followingGroups": null,
        "baseGroup": {
            "id": "l3GmAe9d",
            "name": "Server home",
            "description": null,
            "priority": 1,
            "type": "team",
            "avatar": null,
            "banner": null,
            "teamId": "4R5q39VR",
            "gameId": null,
            "visibilityTeamRoleId": 23138884,
            "membershipTeamRoleId": 23138884,
            "isBase": true,
            "additionalGameInfo": {},
            "createdBy": null,
            "createdAt": "2020-07-31T18:10:35.376187+00:00",
            "updatedBy": null,
            "updatedAt": null,
            "deletedAt": null,
            "customReactionId": null,
            "isPublic": true,
            "archivedAt": null,
            "archivedBy": null
        },
        "subscriptionInfo": null,
        "followerCount": 21,
        "userFollowsTeam": false,
        "isUserApplicant": false,
        "hasImportedDiscordServer": false,
        "isUserInvited": false,
        "lfmStatusByGameId": {},
        "flair": [
            {
                "id": 1,
                "amount": 15
            }
        ],
        "isUserBannedFromTeam": false,
        "mutedMembers": [],
        "deafenedMembers": [],
        "upsell": null,
        "serverSubscriptionsGateEnabled": false,
        "serverSubscriptionPlans": [],
        "teamPaymentInfo": null,
        "socialLinks": []
    }
}
```

## Modify Team
<span class="http-verb">PUT</span><span class="http-path">/teams/{[team.id](#team-object)}/games/null/settings</span>

Modify a team's settings. Returns `{"updated": true}` on success. Fires a [Team Update](/topics/gateway#TeamUpdated) Gateway event.

!!! Info
    All parameters to this endpoint are optional

###### JSON Params

| Field                         | Type                                      | Description                      |
|-------------------------------|-------------------------------------------|----------------------------------|
| name                          | string                                    | new name for the team            |
| gameId                        | integer                                   | the main game's id for the team  |
| description                   | string                                    | the "about" section for the team |

## Disband Team
<span class="http-verb">DELETE</span><span class="http-path">/teams/{[team.id](#team-object)}</span>

Delete a team permanently. User must be owner. Returns `{"success": true}` on success.

## Get Team Channels
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](#team-object)}/channels</span>

Returns a list of team [channel](/resources/channel#channel-object) objects.

## Create Team Channel
<span class="http-verb">POST</span><span class="http-path">/teams/{[team.id](#team-object)}/groups/{group.id}/channels</span>

Create a new [channel](/resources/channel#channel-object) object. Requires the `MANAGE_CHANNELS` permission. Returns the new [channel](/resources/channel#channel-object) object on success. Fires a [Channel Create](/topics/gateway#TeamChannelCreated) Gateway event.

###### JSON Params

| Field             | Type    | Description                                                                       | Required | Default |
|-------------------|---------|-----------------------------------------------------------------------------------|----------|---------|
| name              | string  | channel name (1-??? characters, can include spaces)                               | true     |         |
| contentType       | string  | the [type of channel](/channels#channel-content-types)                            | true     |         |
| channelCategoryId | integer | the category's ID to create this channel under. omit to create without a category | false    | null    |
| isPublic          | boolean | whether or not this channel should be visible to users who aren't in the team     | false    | false   |

## Get Team Member
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](#team-object)}/members/{[user.id](/resources/user#user-object)}</span>

Returns a [team member](#team-member-object) object for the specified user.

## List Team Members
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](#team-object)}/members</span>

Returns a list of partial [team members](#team-member-object), flow bots, and [webhooks](/topics/webhook#webhook-object) under `members`, `bots`, and `webhooks` keys respectively. A partial team member object includes, at minimum, `id` and `name`, but may also include one or multiple of `profilePicture`, `roleIds`, `userPresenceStatus`, and `nickname`.

## Change Team Member Nickname
<span class="http-verb">PUT</span><span class="http-path">/teams/{[team.id](#team-object)}/members/{[user.id](/resources/user#user-object)}/nickname</span>

Change a team member's nickname.

###### JSON Params

| Field    | Type   | Description                         |
|----------|--------|-------------------------------------|
| nickname | string | value to set the user's nickname to |

## Reset Team Member Nickname
<span class="http-verb">DELETE</span><span class="http-path">/teams/{[team.id](#team-object)}/members/{[user.id](/resources/user#user-object)}/nickname</span>

Reset a team member's nickname.

## Set Team Member XP
<span class="http-verb">PUT</span><span class="http-path">/teams/{[team.id](#team-object)}/members/{[user.id](/resources/user#user-object)}/xp</span>

Set a team member's XP.

###### JSON Params

| Field  | Type    | Description                                                                    |
|--------|---------|--------------------------------------------------------------------------------|
| amount | integer | the amount of xp to give this user (overrides previous value, does not add on) |

## Remove Team Member
<span class="http-verb">DELETE</span><span class="http-path">/teams/{[team.id](#team-object)}/members/{[user.id](/resources/user#user-object)}</span>

Remove a member from a team. This is the same endpoint as [Leave Team](/resources/user#leave-team), but is documented here as well because it can also be used for kicking.

## Get Team Bans
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](#team-object)}/members/ban</span>

Returns a list of [ban](#ban-object) objects for the users banned from this guild. Requires the `BAN_MEMBERS` permission.

## Create Team Ban
<span class="http-verb">DELETE</span><span class="http-path">/teams/{[team.id](#team-object)}/members/ban</span>

Create a team ban (ban somebody), and optionally delete previous messages sent by the banned user.

###### JSON Params

| Field                 | Type                                       | Description                                          |
|-----------------------|--------------------------------------------|------------------------------------------------------|
| deleteHistoryOptions? | integer                                    | [delete history options](#delete-history-options)    |
| afterDate             | ?ISO8601 timestamp                         | the date from which to delete this user's messages   |
| memberId              | [member id](/reference#generic-object-ids) | the user's id to ban                                 |
| teamId                | [team id](/reference#generic-object-ids)   | the team's id to ban this user on (the current team) |
| reason                | string                                     | reason for the ban. can be empty                     |

## Remove Team Ban
<span class="http-verb">PUT</span><span class="http-path">/teams/{[team.id](#team-object)}/members/{[user.id](/users#user-object)}/ban</span>

Remove the ban for a user. Returns an empty dictionary on success.

###### JSON Params

| Field    | Type                                       | Description                                              |
|----------|--------------------------------------------|----------------------------------------------------------|
| memberId | [member id](/reference#generic-object-ids) | the user's id to unban                                   |
| teamId   | [team id](/reference#generic-object-ids)   | the team's id to unban this user from (the current team) |

###### Delete History Options

| Value | Meaning     |
|-------|-------------|
| -1    | no messages |
| 0     | |
| 1     | |

## Join Team
<span class="http-verb">PUT</span><span class="http-path">/teams/{[team.id](#team-object)}/members/{[user.id](/resources/user#user-object)}/join</span>

Join a team that has open entry enabled. `user.id` should be the client user's own id.

## Get Team Invites
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](#team-object)}/hash_invites</span>

Returns an array of [invite object](#invite-object)s under an `invites` key on success.

###### Example Response

```json
{
  "invites": [
    {
      "id": "XENGAmn2",
      "createdAt": "2021-02-26T22:43:52.380Z",
      "teamId": "4R5q39VR",
      "invitedBy": "EdVMVKR4",
      "usedBy": null,
      "gameId": null,
      "useCount": 0
    },
    {
      "id": "4E6rm5AE",
      "createdAt": "2020-08-16T20:47:12.733Z",
      "teamId": "4R5q39VR",
      "invitedBy": "w4WMRR1m",
      "usedBy": "8d8aEOgm",
      "gameId": null,
      "useCount": 1
    },
    {
      "id": "Mkd7zGN2",
      "createdAt": "2020-08-14T12:45:09.322Z",
      "teamId": "4R5q39VR",
      "invitedBy": "w4WMRR1m",
      "usedBy": "EdVMVnR4",
      "gameId": null,
      "useCount": 1
    }
  ]
}
```

## Create Team Invite
<span class="http-verb">POST</span><span class="http-path">/teams/{[team.id](#team-object)}/invites</span>

Returns `{"invite": {"id": invite_code}}`, where invite_code is the generated invite code on success (a string, does not include `https://guilded.gg/i/`).

###### JSON Params

| Field  | Type                    | Description                           |
|--------|-------------------------|---------------------------------------|
| gameId | ?integer                | the game's id to make this invite for |
| teamId | [team id](#team-object) | the team's id to make this invite for |

## Get Invite
<span class="http-verb">GET</span><span class="http-path">/invites/{[invite.id](#invite-object)}</span>

Get an invite by its id (invite code). Returns an object with a [`team`](#team-object) (the team the invite is for) and a [`user`](/resources/user#user-object) (the user who created the invite) on success.

###### Query Params

| Field       | Type    | Description                                                                     | Required | Default |
|-------------|---------|---------------------------------------------------------------------------------|----------|---------|
| loadAvatars | boolean | whether to include `invite.team.members`. it is `false` if `true` is not passed | false    | true    |

## Use Invite
<span class="http-verb">PUT</span><span class="http-path">/invites/{[invite.id](#invite-object)}</span>

Join a team via an invite. Returns a special object on success, similar to a regular [invite object](#invite-object) but with extra data and an unrecognized `id` value.

###### Query Params

| Field                 | Type                    | Description                                | Required | Default                         |
|-----------------------|-------------------------|--------------------------------------------|----------|---------------------------------|
| teamId                | [team id](#team-object) | the invite team's id                       | false    | [invite.teamId](#invite-object) |
| includeLandingChannel | boolean                 | whether to include `invite.landingChannel` | false    | false                           |

###### Response Example

```json
{
  "id": 2830449,
  "createdAt": "2021-04-21T18:51:51.558Z",
  "teamId": "4R5q39VR",
  "invitedBy": "EdVMVKR4",
  "usedBy": null,
  "additionalInfo": {
    "source": "clientRequest"
  },
  "gameId": null,
  "useCount": 0,
  "landingChannel": {
    "groupId": "l3GmAe9d",
    "id": "ae12b25a-eb79-48e5-a5a4-1ce83c62f86d",
    "contentType": "chat"
  }
}
```
