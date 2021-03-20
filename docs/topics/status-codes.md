# Status Codes in the API

## Gateway

Specifics of the Gateway's close codes are not known - if custom codes are even used at all. For the time being, refer to [the standard WebSocket close codes](https://developer.mozilla.org/en-US/docs/Web/API/CloseEvent#status_codes) for what any given code you may recieve means.

## HTTP

Similarly to any other REST API, HTTP response codes are returned based on the success of each of your requests. The following table can be used as a reference. It is a modified version of [Discord's table for the same purpose](https://discord.com/developers/docs/topics/opcodes-and-status-codes#http-http-response-codes).

###### HTTP Response Codes

| Code                      | Meaning                                                                              |
|---------------------------|--------------------------------------------------------------------------------------|
| 200 (OK)                  | The request completed successfully.                                                  |
| 201 (CREATED)             | The entity was created successfully.                                                 |
| 204 (NO CONTENT)          | The request completed successfully and returned no content.                          |
| 304 (NOT MODIFIED)        | The entity was not modified (no action was taken).                                   |
| 400 (BAD REQUEST)         | The request was improperly formatted, or the server couldn't understand it.          |
| 401 (UNAUTHORIZED)        | Proper authorization was not passed or passed improperly.                            |
| 403 (FORBIDDEN)           | The authorization you passed is not allowed to access this resource.                 |
| 404 (NOT FOUND)           | The resource at the location specified doesn't exist.                                |
| 405 (METHOD NOT ALLOWED)  | The HTTP method used is not valid for the location specified.                        |
| 429 (TOO MANY REQUESTS)   | You are being rate limited.                                                          |
| 502 (GATEWAY UNAVAILABLE) | There was not a gateway available to process your request. Wait a bit and retry.     |
| 5xx (SERVER ERROR)        | The server had an error processing your request (these are rare).                    |
