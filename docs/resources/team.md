# Teams

The meat of communication on Guilded. They are referred to as "servers" in the UI.

### Team Object

###### Team Structure

| Field                 | Type                                                   | Description                                                                     |
|-----------------------|--------------------------------------------------------|---------------------------------------------------------------------------------|
| id                    | [generic id](/reference#ids)                           | the team's id                                                                   |
| name                  | string                                                 | the team's name                                                                 |
| subdomain             | ?string                                                | custom "url" of the team. equivalent to discord's `vanity_url_code`             |
| description           | ?string                                                | the team's description                                                          |
| bio (deprecated)      | ?string                                                | the team's bio - deprecated in favor of `description`                           |
| profilePicture        | ?string                                                | the team's avatar url                                                           |
| teamDashImage         | ?string                                                | the team's banner url                                                           |
| ownerId               | [user id](/reference#ids)                              | id of the team's owner                                                          |
| createdAt             | ISO8601 timestamp                                      | when the team was created                                                       |
| type                  | string                                                 | the [type of team](#team-types)                                                 |
| members?\*            | array of [members](#team-member-object)                | the members in this team                                                        |
| bots?\*\*             | array of flow-bots                                     | the flow-bots in this team                                                      |
| webhooks?\*\*         | array of [webhooks](/resources/webhook#webhook-object) | the webhooks in this team                                                       |
| baseGroup             | [group](/resources/group#group-object)                 | the team's base group                                                           |
| visibility            | string                                                 | the team's [visibility setting](#team-visibility)                               |
| isRecruiting          | boolean                                                | whether the team is accepting new applications                                  |
| isVerified            | boolean                                                | whether the team is verified                                                    |
| isPro                 | boolean                                                | whether the team is "pro" - probably deprecated                                 |
| isPublic              | boolean                                                | whether the team is public. true when `visibility` is `open-entry`              |
| isDiscoverable        | boolean                                                | whether the team will show up in search results & the server directory          |
| rolesById             | object                                                 | mapping of role id (string) to role, plus a duplicate `baseRole`                |
| userFollowsTeam       | boolean                                                | whether the current users follows the team                                      |
| isUserApplicant?      | boolean                                                | whether the current user has a pending application in the team                  |
| isUserInvited?        | boolean                                                | whether the current user has been invited to the team                           |
| isUserBannedFromTeam? | boolean                                                | whether the current user is banned from the team                                |
| followerCount         | ?integer                                               | the number of followers the team has (approximate)                              |
| memberCount           | ?string                                                | the number of members the team has (approximate)                                |
| onlineMemberCount?    | ?string                                                | the number of non-offline members in the team                                   |
| measurements?         | object                                                 | a [team measurements](#team-measurements-object) object for the team            |
| games?                | array of [games ids](/resources/user#game-ids)         | the games that the team plays                                                   |
| additionalGameInfo    | object                                                 | mapping of game id to `region` and `platformps4,xbox,pc`, both optional strings |

\* `members` will only contain the current user's member data for the [Get Team Info](#get-team-info) response.

\*\* `bots` and `webhooks` are not present for the [Get Team Info](#get-team-info) response.

###### Example Team

```json
{
    "id": "4R5q39VR",
    "name": "Guilded-API",
    "subdomain": "guilded-api",
    "bio": null,
    "status": null,
    "timezone": "America/New York (EST/EDT)",
    "description": "The unofficial resource hub for all API-related projects and development to do with the Guilded API. API docs: guildedapi.com",
    "type": "community",
    "visibility": "open-entry",
    "createdAt": "2020-07-31T18:10:35.302Z",
    "ownerId": "EdVMVKR4",
    "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamAvatar/a66e23924a4bc49fbf9242a98d955a7c-Large.png?w=450&h=450",
    "teamDashImage": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamBanner/62d94db23c910fa3a209b5edf2cf7387-Hero.png?w=1067&h=600",
    "homeBannerImageSm": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamBanner/62d94db23c910fa3a209b5edf2cf7387-Hero.png?w=1067&h=600",
    "homeBannerImageMd": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamBanner/62d94db23c910fa3a209b5edf2cf7387-Hero.png?w=1067&h=600",
    "homeBannerImageLg": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/TeamBanner/62d94db23c910fa3a209b5edf2cf7387-Hero.png?w=1067&h=600",
    "additionalInfo": {
        "platform": "native"
    },
    "additionalGameInfo": {},
    "teamPreferences": null,
    "socialInfo": {
        "twitter": "@GuildedAPI"
    },
    "isRecruiting": false,
    "isVerified": false,
    "isPro": false,
    "isPublic": true,
    "notificationPreference": null,
    "isDiscoverable": true,
    "baseGroup": {
        "id": "l3GmAe9d",
        "type": "team",
        "name": "Server home",
        "description": null,
        "avatar": null,
        "banner": null,
        "priority": 1,
        "teamId": "4R5q39VR",
        "gameId": null,
        "visibilityTeamRoleId": 23138884,
        "membershipTeamRoleId": 23138884,
        "isBase": true,
        "isPublic": true,
        "additionalGameInfo": {},
        "createdBy": null,
        "createdAt": "2020-07-31T18:10:35.376Z",
        "updatedBy": "EdVMVKR4",
        "customReactionId": null,
        "updatedAt": "2021-05-03T19:20:08.249Z",
        "deletedAt": null,
        "archivedAt": null,
        "archivedBy": null
    },
    "followingGroups": [],
    "rolesById": {
        "26302427": {
            "id": 26302427,
            "name": "Bot",
            "color": "#ffffff",
            "permissions": {"xp":1,"bots":1,"chat":503,"docs":15,"forms":18,"lists":63,"media":15,"voice":8179,"forums":123,"general":130100,"streams":51,"brackets":3,"calendar":31,"scheduling":11,"matchmaking":21,"recruitment":55,"announcements":7,"customization":49},
            "priority": 17,
            "teamId": "4R5q39VR",
            "createdAt": "2021-12-01T17:11:43.610Z",
            "updatedAt": "2022-03-02T22:47:11.473Z",
            "isBase": false,
            "discordRoleId": null,
            "discordSyncedAt": null,
            "isMentionable": false,
            "isSelfAssignable": false,
            "isDisplayedSeparately": false,
            "botScope": {"userId":null}
        },
        "baseRole": {
            "id": 23138884,
            "name": "Member",
            "color": "#ececee",
            "permissions": {"chat":243,"docs":2,"forms":16,"lists":2,"media":2,"voice":6211,"forums":67,"streams":51,"calendar":2,"scheduling":3,"announcements":2,"customization":16},
            "priority": 1,
            "teamId": "4R5q39VR",
            "createdAt": "2020-07-31T18:10:35.350Z",
            "updatedAt": "2022-03-02T22:47:11.473Z",
            "isBase": true,
            "discordRoleId": null,
            "discordSyncedAt": null,
            "isMentionable": true,
            "isSelfAssignable": false,
            "isDisplayedSeparately": true,
            "botScope": null
        }
    },
    "userFollowsTeam": false,
    "followerCount": 88,
    "memberCount": "1958",
    "members": [
        {
            "id": "EdVMVKR4",
            "name": "shay",
            "type": "user",
            "membershipRole": "admin",
            "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/c2da767cf9795e7c73facc399159fefc-Large.png?w=450&h=450",
            "roleIds": [23138891,23281097,23138936,23139118,23138883]
        }
    ],
    "upsell": null,
    "serverSubscriptionPlans": [],
    "teamPaymentInfo": null,
    "games": [],
    "bannerImages": {},
    "lfmStatusByGameId": {},
    "drawbridgeGateEnabled": false,
    "flair": [
        {
            "id": 3
        }
    ],
    "socialLinks": []
}
```

##### Team Types

Team types are probably used in server discovery.

| Value        |
|--------------|
| team         |
| organization |
| community    |
| clan         |
| guild        |
| friends      |
| streaming    |
| other        |

##### Team Visibility

Reflects the team's [privacy setting](https://www.guilded.gg/blog/server-privacy).

| Value      | Meaning                                                                                                           | Default |
|------------|-------------------------------------------------------------------------------------------------------------------|---------|
| private    | the team is not visible through its direct link and cannot show up in search results                              | false   |
| default    | the team is visible through its direct link and will show up in search results if `team.isDiscoverable` is `true` | true    |
| open-entry | `default`, but the team does not require an invite to join                                                        | false   |

### Team Measurements Object

This object contains some statistics about the team. When available, you should prefer this object's `numMembers` over `team.memberCount` because it is more accurate.

###### Team Measurements Structure

| Field                                    | Type              | Description                                                                                                             |
|------------------------------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------|
| numMembers                               | integer           | the number of members the team has                                                                                      |
| numFollowers                             | integer or string | the number of followers the team has                                                                                    |
| numRecentMatches                         | integer           | the number of matches the team has participated in recently                                                             |
| numRecentMatchWins                       | integer           | the number of matches the team has won recently                                                                         |
| matchmakingGameRanks                     | array             |                                                                                                                         |
| numFollowersAndMembers                   | integer           | `numMembers` + `numFollowers`                                                                                           |
| numMembersAddedInLastDay                 | integer           | the number of members who have joined the team in the last 24 hours                                                     |
| numMembersAddedInLastWeek                | integer           | the number of members who have joined the team in the last 7 days                                                       |
| numMembersAddedInLastMonth               | integer           | the number of members who have joined the team in the last 30 days                                                      |
| mostRecentMemberLastOnline               | integer           | unix timestamp of the last time a member was not offline in the team                                                    |
| subscriptionMonthsRemaining (deprecated) | ?integer          | the number of months remaining on the team's [guilded gold subscription](https://www.guilded.gg/p/new-feature-november) |

### Team Member Object

###### Team Member Structure

| Field               | Type                                                        | Description                                                               |
|---------------------|-------------------------------------------------------------|---------------------------------------------------------------------------|
| id                  | [generic id](/reference#ids)                                | the user's id                                                             |
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

| Field     | Type                                    | Description                                                    |
|-----------|-----------------------------------------|----------------------------------------------------------------|
| id        | [generic id](/reference#ids)            | the invite's id, referred to as an 'invite code' in the client |
| createdAt | ISO8601 timestamp                       | when the invite was created                                    |
| teamId    | [team id](#team-object)                 | the team's id that the invite is for                           |
| invitedBy | [user id](/resources/user#user-object)  | the user's id who created the invite                           |
| userBy    | ?[user id](/resources/user#user-object) | a user's id who used the invite?                               |
| gameId    | ?integer                                | the game's id that the invite is for                           |
| useCount  | integer                                 | how many times the invite has been used                        |

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

## Get Team Info
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](#team-object)}/info</span>

Returns a [team](#team-object) object wrapped in a `team` key on success. If the team is invalid or private, this returns HTTP 404.

## Get Team Channels
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](#team-object)}/channels</span>

Returns a list of team [channel](/resources/channel#channel-object) objects.

## List Team Members
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](#team-object)}/members</span>

Returns a list of partial [team members](#team-member-object), flow bots, and [webhooks](/topics/webhook#webhook-object) under `members`, `bots`, and `webhooks` keys respectively. A partial team member object includes, at minimum, `id` and `name`, but may also include one or multiple of `profilePicture`, `roleIds`, `userPresenceStatus`, and `nickname`.

## Detail Team Members
<span class="http-verb">POST</span><span class="http-path">/teams/{[team.id](#team-object)}/members/detail</span>

Returns an object of provided user IDs to [team members](#team-member-object).

###### JSON Params

| Field            | Type                                   | Description                                                                                   |
|------------------|----------------------------------------|-----------------------------------------------------------------------------------------------|
| userIds          | array of [generic ids](/reference#ids) | the members' IDs to get member details for                                                    |
| idsForBasicInfo? | array of [generic ids](/reference#ids) | if an ID is included here, its entry in the response will also include basic user information |

###### Example Response

```json
{
  "EdVMVKR4": {
    "userStatus": {
      "content": null,
      "customReactionId": 925765,
      "customReaction": {
        "id": 925765,
        "name": "blobspider",
        "png": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/b721e28333392c335fcff52eb27997fd-Full.webp?w=120&h=120",
        "webp": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/CustomReaction/b721e28333392c335fcff52eb27997fd-Full.webp?w=120&h=120",
        "apng": null
      }
    },
    "id": "EdVMVKR4",
    "name": "shay",
    "membershipRole": "admin",
    "profilePicture": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/c2da767cf9795e7c73facc399159fefc-Large.png?w=450&h=450",
    "userTransientStatus": null,
    "teamXp": 117,
    "lastOnline": "2022-04-29T21:10:42.120Z",
    "joinDate": "2020-07-31T18:10:35.302Z",
    "stonks": 56
  }
}
```
