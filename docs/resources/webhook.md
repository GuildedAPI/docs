# Webhooks

Webhooks are an easy way to post messages to team channels. They do not require any authentication to use other than the webhook ID & token itself (keep tokens secret!).

### Webhook Object

###### Webhook Structure

| Field      | Type                                   | Description                                 |
|------------|----------------------------------------|---------------------------------------------|
| id         | uuid                                   | the id of the webhook                       |
| name       | string                                 | the default name of the webhook             |
| token?     | string                                 | the webhook's token. required for execution |
| channelId  | uuid                                   | the channel id this webhook is for          |
| teamId     | [team id](/resources/team#team-object) | the team's id this webhook is for           |
| iconUrl    | ?string                                | the default avatar of the webhook           |
| createdBy  | [user id](/resources/user#user-object) | the user's id who created this webhook      |
| createdAt  | ISO8601 timestamp                      | when this webhook was created               |
| deletedAt  | ?ISO8601 timestamp                     | when this webhook was deleted               |

###### Example Webhook

```json
{
  "id": "5b3723f8-c82e-404d-bb56-02bbfb242e47",
  "name": "cool webhook for cool people",
  "token": "yXEUjW1HskqGwsCWZBqGiYW4MGWAWq0sqCp8igYgQWcgNmYMu6gswa24CgoE2Akqk00YS8GYMkeqKUKKlAUYua",
  "channelId": "b1b9451a-f758-4e49-aa81-0b148939ffeb",
  "teamId": "4R5q39VR",
  "iconUrl": "https://s3-us-west-2.amazonaws.com/www.guilded.gg/UserAvatar/74bfc8be9425a926a1f48d9b078509bc-Large.png?w=450&h=450",
  "createdBy": "EdVMVKR4",
  "createdAt": "2021-03-18T23:39:21.320Z",
  "deletedAt": null
}
```

## Execute Webhook
<span class="http-verb">POST</span><span class="http-path">https://media.guilded.gg/webhooks/{[webhook.id](#webhook-object)}/{[webhook.token](#webhook-object)}</span>

!!! info
    This endpoint no longer supports GitHub payloads. See [guilded-webhook-proxy](https://github.com/shayypy/guilded-webhook-proxy) for an alternative.

Send a message through a webhook. Returns a [message](/resources/channel#message-object) object.

###### JSON/Form Params

| Field          | Type                                               | Description                                  | Required                   |
|----------------|----------------------------------------------------|----------------------------------------------|----------------------------|
| content        | string                                             | the message contents (up to 2000 characters) | one of content, embeds     |
| embeds         | array of [embeds](/resources/channel#embed-object) | up to 10 embeds                              | one of content, embeds     |
| files[n]\*     | file contents                                      | up to 10 files                               | false                      |
| payload_json\* | string                                             | JSON encoded body of non-file params         | `multipart/form-data` only |
| username       | string                                             | override the webhook's default username      | false                      |
| avatar_url     | string                                             | override the webhook's default avatar        | false                      |

\* See [Uploading Files](https://discord.dev/reference#uploading-files) for details.
