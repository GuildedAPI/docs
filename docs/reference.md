# Reference

On a conceptual level, Discord and Guilded's APIs both function similarly. To quote [Discord's reference documentation](https://discord.com/developers/docs/reference):
> [The API] is based around two core layers, a HTTPS/REST API for general operations, and persistent secure WebSocket based connection for sending and subscribing to real-time events.

###### Base URL

```
https://www.guilded.gg/api
```

## Error Messages

Your typical error message should look like this:

```json
{
    "code": "YouDidBadError",
    "message": "you did bad. here is how you did bad. do better."
}
```

... along with an appropriate [HTTP status code](/topics/status-codes#http).

## Authentication

As mentioned in [the Intro page](/#userbots-you), there is no bot API. In order to automate an application on the API, you would have to use a user account.

For the HTTP API, authentication is not passed through the headers, but the JSON body. 

## Quick Authentication How-To
<span class="http-verb">POST</span><span class="http-path">/login</span>

Logs you into Guilded. Important data here is the returned `guilded_mid` cookie, which you need to pass as a `guildedClientId` query argument when [connecting to the gateway](/topics/gateway#connecting-to-the-gateway).

###### JSON Parameters

| Field    | Type   | Description            | Required |
|----------|--------|------------------------|----------|
| email    | string | the account's email    | true     |
| password | string | the account's password | true     |

## Encryption

The REST and WebSocket APIs are 'secure' (HTTPS, WSS). I am unsure of any other steps Guilded takes to secure data, if any.

## Snowflakes & UUIDs

Unlike Discord, Guilded uses [UUIDs](https://wikipedia.org/wiki/Universally_unique_identifier) for many of its unique IDs ([Channels](/resources/channel#channel-object) (all types), [Messages](/resources/channel#message-object), and [Webhooks](/resources/webhook#webhook-object) use UUIDs). Because of this, none of the same properties apply as to Snowflakes.

### Generating UUIDs

For some endpoints, you will have to Bring Your Own UUID (BYOU). Worry not - this is typically a trivial task. Most languages should come with a way to generate a compliant UUID with ease, or have a 3rd-party library for such a purpose:

| Method                                                                                                             | Language   |
|--------------------------------------------------------------------------------------------------------------------|------------|
| [uuid.uuid1()](https://docs.python.org/3/library/uuid.html#uuid.uuid1)                                             | Python     |
| [uuid.uuidv4()](https://www.npmjs.com/package/uuid)                                                                | JavaScript |
| [uuid-random.uuid()](https://www.npmjs.com/package/uuid-random)                                                    | JavaScript |
| [System.Guid.NewGuid()](https://docs.microsoft.com/dotnet/api/system.guid.newguid)                                 | C#         |
| [Uuid::new_v4()](https://docs.rs/uuid/0.8.2/uuid/struct.Uuid.html#method.new_v4)                                   | Rust       |
| [UUID.random](https://crystal-lang.org/api/1.0.0/UUID.html)                                                        | Crystal    |
| [UUID.randomUUID()](https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/util/UUID.html#randomUUID()) | Java       |

## Generic Object IDs

These are 8-character, probably-meaningless IDs that Guilded uses in lieu of Snowflakes or UUIDs. They are used for objects like [Teams](/resources/team#team-object), [Groups](/resources/group#group-object), and [Users](/resources/user#user-object).

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

The HTTP API has very loose rate limits, most likely due to it not being written for automation. Because of this, we do not know much about how rate limiting works on Guilded. A good rule of thumb is to not spam the API with repetitive requests and to simply stay reasonable.

## Gateway (WebSocket) API

The Gateway API is used for maintaining persistent, stateful websocket connections between your client and Guilded servers. These connections are used for receiving real-time events your client can use to track and update local state. For information on opening Gateway connections, please see the [Gateway API](/topics/gateway) section.

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

## Upload a File
<span class="http-verb">POST</span><span class="http-path">https://media.guilded.gg/media/upload</span>

Returns `{"url": "https://a_url_to_your_uploaded_file.png"}` on success.

###### Form Data

| Field | Type        | Description          |
|-------|-------------|----------------------|
| file  | binary data | the data of the file |

###### Query Args

| Field              | Type   | Description                              | Required | Accepted Values                                    |
|--------------------|--------|------------------------------------------|----------|----------------------------------------------------|
| dynamicMediaTypeId | string | the type of media that you are uploading | true     | a valid [dynamic media type](#dynamic-media-types) |

###### Dynamic Media Types

| Type           | Description         |
|----------------|---------------------|
| CustomReaction | custom server emoji |
| ContentMedia   | attachment in chat  |
| UserAvatar     | user avatar         |
| UserBanner     | profile banner      |
| TeamAvatar     | team icon           |
| TeamBanner     | team banner         |
| GroupAvatar    | group icon          |

###### Binary File Data

This can probably be handled by your HTTP library/something built-in to your lang. Here are some code examples:

<!-- Code examples do not use syntax highlighting because it looks awful. Will be fixed soon, maybe. No colors > bad colors -->

**Python**

```
# with aiohttp

import aiohttp
async def upload_file(path_to_file, media_type):
    file = open(path_to_file, 'rb')  # or another bytes-like object
    async with aiohttp.ClientSession() as session:
        response = await session.post(
            'https://media.guilded.gg/media/upload',
            data={'file': file},
            params={'dynamicMediaTypeId': media_type}
        )
        data = await response.json()
        return data['url']


# with requests

import requests
def upload_file(path_to_file, media_type):
    file = open(path_to_file, 'rb')  # or another bytes-like object
    response = requests.post(
        'https://media.guilded.gg/media/upload',
        data={'file': file},
        params={'dynamicMediaTypeId': media_type}
    )
    data = response.json()
    return data['url']
```
