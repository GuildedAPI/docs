# Media Endpoints

!!! warning
    Endpoints on this page should use the base URL `https://media.guilded.gg`, *not* `https://www.guilded.gg/api`.

## Dynamic Media Types

| Type                      | Description                                                   |
|---------------------------|---------------------------------------------------------------|
| CustomReaction            | custom server emoji                                           |
| ContentMedia (deprecated) | any media - deprecated in favor of `ContentMediaGenericFiles` |
| ContentMediaGenericFiles  | any media or file                                             |
| UserAvatar                | user avatar                                                   |
| UserBanner                | profile banner                                                |
| TeamAvatar                | team icon                                                     |
| TeamBanner                | team banner                                                   |
| GroupAvatar               | group icon                                                    |

## Binary File Data

This can probably be handled by your HTTP library/something built-in to your language. Here are some code examples:

<!-- Code examples do not use syntax highlighting because it looks awful. -->

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

## Upload Media
<span class="http-verb">POST</span><span class="http-path">/media/upload</span>

Upload a media file to Guilded.

###### Form Data

| Field | Type                             | Description          |
|-------|----------------------------------|----------------------|
| file  | [binary data](#binary-file-data) | the data of the file |

###### Query Args

| Field              | Type   | Description                              | Required | Accepted Values                                    |
|--------------------|--------|------------------------------------------|----------|----------------------------------------------------|
| dynamicMediaTypeId | string | the type of media that you are uploading | true     | a valid [dynamic media type](#dynamic-media-types) |

###### JSON Response

| Field | Type   | Description                           |
|-------|--------|---------------------------------------|
| url   | string | the full CDN url to the uploaded file |

## Upload File
<span class="http-verb">POST</span><span class="http-path">/media/file_upload</span>

Upload a generic non-media file to Guilded.

###### Form Data

| Field | Type                             | Description          |
|-------|----------------------------------|----------------------|
| file  | [binary data](#binary-file-data) | the data of the file |

###### Query Args

| Field              | Type   | Description                              | Required | Accepted Values          |
|--------------------|--------|------------------------------------------|----------|--------------------------|
| dynamicMediaTypeId | string | the type of media that you are uploading | true     | ContentMediaGenericFiles |

###### JSON Response

| Field | Type   | Description                           |
|-------|--------|---------------------------------------|
| url   | string | the full CDN url to the uploaded file |

## Get Custom Emoji Pack
<span class="http-verb">POST</span><span class="http-path">/media/fetch_emote_pack</span>

Get the contents of an emoji pack.

###### JSON Params

| Field | Type   | Description                                                                    |
|-------|--------|--------------------------------------------------------------------------------|
| url   | string | the emoji pack to get (must return a valid JSON-formatted emoji pack response) |

## Upload Custom Emoji Pack
<span class="http-verb">POST</span><span class="http-path">/media/import_emote_pack</span>

Upload the contents of an emoji pack to Guilded. Returns an array of `{url: url, name: name}` on success, where `url` is the URL to the uploaded image on Guilded's CDN, and `name` is the name of the emoji from the emoji pack.

###### JSON Params

| Field             | Type   | Description                                                                       |
|-------------------|--------|-----------------------------------------------------------------------------------|
| url               | string | the emoji pack to import (must return a valid JSON-formatted emoji pack response) |
| uploadTrackingId? | string | unknown. looks like "r-0123456-7891234"                                           |
