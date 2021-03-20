# Webhooks

Webhooks are an easy way to post messages to team channels. They do not require any authentication to use other than the webhook URL itself (keep those secret!).

### Webhook Object

Used to represent a webhook.

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

## Create Webhook
<span class="http-verb">POST</span><span class="http-path">/webhooks</span>

Create a new webhook. Returns a [webhook](#webhook-object) object on success.

###### JSON Params

| Field     | Type                                            | Description                               |
|-----------|-------------------------------------------------|-------------------------------------------|
| name      | string                                          | name of the webhook (1-??? characters)    |
| channelId | [channel id](/resources/channel#channel-object) | the channel's id to create the webhook in |

## Get Channel Webhooks
<span class="http-verb">GET</span><span class="http-path">/teams/{[team.id](/resources/team#team-object)}/channels/{[channel.id](/resources/channel#channel-object)}/webhooks</span>

!!! bug
    This endpoint returns an empty list; `{"webhooks": []}`. Instead, use [team.webhooks](/resources/team#get-team).

Get a list of [webhook](#webhook-object) objects.

## Modify Webhook
<span class="http-verb">PUT</span><span class="http-path">/webhooks/{[webhook.id](#webhook-object)}</span>

Modify a webhook. Returns the updated [webhook](#webhook-object) object on success.

!!! info
    The client performs this immediately after [creating a webhook](#create-webhook) in order to actually fill all its fields appropriately.

!!! info
    The client will always send `name` and `channelId` even if they were not changed. Try it out and see if they're required!

###### JSON Params

| Field      | Type                                            | Description                                        |
|------------|-------------------------------------------------|----------------------------------------------------|
| name       | string                                          | the default name of the webhook                    |
| iconUrl    | image url                                       | image url for the default webhook avatar           |
| channelId  | [channel id](/resources/channel#channel-object) | the new channel id this webhook should send to     |

## Delete Webhook
<span class="http-verb">DELETE</span><span class="http-path">/webhooks/{[webhook.id](#webhook-object)}</span>

Delete a webhook. Returns a partial [webhook](#webhook-object) object on success with only `id` and `deletedAt` fields.

## Execute Webhook
<span class="http-verb">POST</span><span class="http-path">https://media.guilded.gg/webhooks/{[webhook.id](#webhook-object)}/{[webhook.token](#webhook-object)}</span>

!!! info
    This endpoint is supposedly [similar enough](/images/webhooks_identical.png) to Discord's [`POST /webhooks/{webhook.id}/{webhook.token}`](https://discord.dev/resources/webhook#execute-webhook) that all you have to do is "change a url". However, many of the same fields are not supported (although they will not raise).

!!! info
    You do not have to append `/github` onto your webhook URL for use with GitHub repos and organizations. The URL by itself will work as intended.

!!! warning
    It is recommended not to choose all events for GitHub webhooks, as many of them are not supported and will result in an `Error: event type [...] not supported` whenever it happens.

Send a message through a webhook.

###### JSON/Form Params

| Field   | Type   | Description                                               | Required               |
|---------|--------|-----------------------------------------------------------|------------------------|
| content | string | the message contents (up to 2000 characters)              | one of content, embeds |
| embeds  | array  | up to 10 [embed](/resources/channel#embed-object) objects | one of content, embeds |
