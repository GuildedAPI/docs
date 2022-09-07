# Reference

Guilded's API features a WebSocket gateway for receiving live events, but it requires user authentication and thus will not be covered here.

###### Base URL

```
https://www.guilded.gg/api
```

## Error Messages

Your typical error message should look like this:

```json
{
    "code": "BadRequestError",
    "message": "That's no good. Try again with a different payload."
}
```

... along with an appropriate [HTTP status code](/topics/status-codes#http).

## IDs

### Types

Unlike Discord, Guilded does not use [Snowflakes](https://discord.com/developers/docs/reference#snowflakes) for any of its IDs - instead, you will find a mix of [UUIDs](https://wikipedia.org/wiki/Universally_unique_identifier), 8-character alphanumeric strings (hereafter "generic" IDs), and incremental & random integer IDs.

### Uniqueness of IDs by scope

| Resource                                                                  | ID Type     | Data Type | Scope       |
|---------------------------------------------------------------------------|-------------|-----------|-------------|
| All content replies                                                       | Incremental | integer   | per-content |
| Announcement                                                              | Generic     | string    | ?           |
| Availability                                                              | Incremental | integer   | global      |
| [CalendarEvent](https://guilded.gg/docs/api/calendarEvents/CalendarEvent) | Incremental | integer   | global      |
| [Channel](/resources/channel#channel-object)                              | UUID        | string    | global      |
| [Doc](https://guilded.gg/docs/api/docs/Doc)                               | Incremental | integer   | global      |
| [Emoji](/resources/emoji#custom-emoji-object) (emote)                     | Incremental | integer   | global      |
| [ForumTopic](https://guilded.gg/docs/api/forums/ForumTopic)               | Random      | integer   | ?           |
| [Group](/resources/group#group-object)                                    | Generic     | string    | global      |
| [ListItem](https://guilded.gg/docs/api/listItems/ListItem)                | UUID        | string    | ?           |
| Media                                                                     | Incremental | integer   | global      |
| [Message](/resources/channel#message-object)                              | UUID        | string    | ?           |
| Role                                                                      | Incremental | integer   | global      |
| [Team](/resources/team#team-object) (server)                              | Generic     | string    | global      |
| [User](/resources/user#user-object)                                       | Generic     | string    | global      |
| [Webhook](/resources/webhook#webhook-object)                              | UUID        | string    | global      |

## ISO8601 Date/Time

Guilded utilizes the [ISO8601 format](https://www.loc.gov/standards/datetime/iso-tc154-wg5_n0038_iso_wd_8601-1_2016-02-16.pdf) for most Date/Times returned. This format is referred to as type `ISO8601 timestamp` within tables in this documentation.

## Nullable and Optional Resource Fields

Resource fields that may contain a `null` value have types that are prefixed with a question mark.
Resource fields that are optional have names that are suffixed with a question mark.

###### Example Nullable and Optional Fields

| Field                        | Type    |
|------------------------------|---------|
| optional_field?              | string  |
| nullable_field               | ?string |
| optional_and_nullable_field? | ?string |

## HTTP API

### Rate Limiting

The HTTP API has very loose rate limits. Because of this, we do not know much about how rate limiting works on Guilded. A good rule of thumb is to not spam the API with repetitive requests and to simply stay reasonable.

## Message Formatting

Guilded uses markdown for writing messages in clients. Message are returned via the API in a very 'stacked' format. Each aspect of a message's content, such as text formatting, a mention, or anything else, is provided in a different leaf of the current node. Fortunately, this is not a format you have to *send*; you can instead use pure markdown by providing `markdown-plain-text` as your `type`.

todo: fact check above

## Image Formatting

###### Image Base URLs

```
https://img.guildedcdn.com/
https://s3-us-west-2.amazonaws.com/www.guilded.gg/
```

Guilded uses hashes for its images. These hashes are not returned alone in most HTTP API requests, however--instead you are returned a full URL on the Amazon AWS platform.

Below are the formats, size limitations, and CDN endpoints for images in Guilded. The returned format can be changed by changing the [extension name](#image-formats) at the end of the URL. The returned size can be changed by appending a `-{Size}` to the URL, just before the file extension. [See below](#sizes) for available size formats.

It is important to note that, while URLs using the CDN domain are shorter than, and serve the same purpose as, AWS URLs, they will not display properly in embeds (in constrast to AWS URLs).

###### Image Formats

| Name | Extension   |
|------|-------------|
| PNG  | .png        |
| aPNG | .png        |
| JPEG | .jpg, .jpeg |
| WebP | .webp       |
| GIF  | .gif        |

###### CDN Endpoints

| Type           | Path                     | Supports   |
|----------------|--------------------------|------------|
| User Avatar    | UserAvatar/hash.png      | PNG, GIF   |
| Profile Banner | UserBanner/hash.png      | PNG        |
| Image in Chat  | ContentMedia/hash.webp   | WebP       |
| Team Emoji     | CustomReaction/hash.webp | WebP, aPNG |
| Team Icon      | TeamAvatar/hash.png      | PNG        |
| Team Banner    | TeamBanner/hash.png      | PNG        |

###### Sizes

Your `-{Size}` can be one of `-Small`, `-Medium`, `-Large`, and sometimes `-Hero`. Banners can only be `Hero`, but avatars are never `Hero`.

###### Example CDN URL

The below example is for a "large" sized User Avatar on AWS:

```
https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/{hash}-Large.png
```
