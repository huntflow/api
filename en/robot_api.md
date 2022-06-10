# Tokens received via Huntflow interface

<a name="robot_refresh_token"></a>
## Token refresh
Warning! This method works **only** for tokens from "Tokens" tab in Huntflow interface.

Token can only be refreshed when access token expired.

`POST /v2/token/refresh`

Request body:
```json
{
  "refresh_token": "%secret_refresh_token%",
}
```
Response example:
```json
{
    "access_token": "%new_access_token%",
    "token_type": "bearer",
    "expires_in": 604800,
    "refresh_token_expires_in": 1209600,
    "refresh_token": "%new_refresh_token%"
}
```