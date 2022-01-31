
# Token

Token simplifies integrations with the Huntflow API without additional authorization. The most frequent use is 
server scripts or applications integration with Huntflow (e.g. collecting responses from a corporate website).

You can issue API token at Huntflow system settings.

:warning: Token usage requires higher level of security for storage and unauthorized access issues.


<a name="get_token"></a>
## Getting a new token
You can issue new API token at Huntflow system settings.
You'll get: **access**_token and **refresh**_token. Access token lifetime - 7 days, refresh token lifetime - 14 days.

After access_token lifetime end you should [reissue new one with a refresh_token](token.md#refresh_token).


<a name="use_token"></a>
## Token usage

The application must use the aquired token in requests, stating it in a header as: `Authorization: Bearer 
<access_token>`
To test a token, use the method [/me](user.md#me).

```GET /me HTTP/1.1
User-Agent: App/1.0 (incaseoffire@example.com)
Host: api.huntflow.ru
Accept: */*
Authorization: Bearer <access_token>
```

<a name="refresh_token"></a>
## Token refresh
:warning: This method works **only** for tokens that was issued from "API Tokens" tab in Huntflow interface.

Token can only be refreshed when access token expired.

`POST /token/refresh`

Request body:
```json
{
  "refresh_token": "<refresh_token>",
  "grant_type": "refresh_token"
}
```
Response example:
```json
{
    "access_token": "<new_access_token>",
    "token_type": "bearer",
    "expires_in": 604800,
    "refresh_token_expires_in": 1209600,
    "refresh_token": "<new_refresh_token>"
}
```