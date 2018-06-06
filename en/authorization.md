# Authorization

The authorization is performed by OAuth 2.0 protocol ([RFC 6749](http://tools.ietf.org/html/rfc6749)).

Registered application can request an access to the data from a user without getting and saving login and password. 

<a name="oauth"></a>
## Getting an authorization
At first the application should ask a user to follow the link to a page:
```
https://huntflow.ru/oauth/authorize?response_type=code&client_id={client_id}&state={state}&redirect_uri={redirect_uri}
```

Required parameters:

* `response_type=code` — states a type of getting an authorization using an authorization code
* `client_id={client_id}` — application ID

Optional parameters:

* `state={state}` — if stated will be added to the response. That elliminates the chance of being hacked 
by cross-site request forgery ([RFC 6749. Section 10.12](http://tools.ietf.org/html/rfc6749#section-10.12))

* `redirect_uri={redirect_uri}` — The address for user redirection after authorization. 
The address from the application settings are used by default.

<a name="oauth_process"></a>
### Authorization process

If a user is not authentificated at the website, they will be shown a login form. After authentification 
the user will get a form with the application access request to the user’s data.

If a user declines an access request, the user will be redirected to the stated redirect_uriс ?error=access_denied и state={state} 
(if it was stated).


<a name="oauth_authorization_code"></a>
### Successful acquisition of a temporary `authorization_code`

If the access is granted the redirect will show the following temporary `authorization_code`:

```http
HTTP/1.1 302 Found
Location: {redirect_uri}?code={authorization_code}
```

If the user is authentificated at the website and got the access to the application before, described above `authorization_code` 
will return (without showing login form and authorization). 

<a name="oauth_tokens"></a>
### Getting an access and refresh tokens

After getting an `authorization_code` the application has to make a server to server POST-request 
to `https://huntflow.ru/oauth/token` to exchange the `authorization_code` to the `access_token` 

The request should contain:

```
grant_type=authorization_code&client_id={client_id}&client_secret={client_secret}&code={authorization_code}&redirect_uri={redirect_uri}
```

If at `authorization_code` acquisition `redirect_uri` was stated, the request must pass this value (the lines are compared), otherwise this parameter is not necessary. If the `redirect_uri` is not stated while requesting `/oauth/authorize`,  then when it is passed in the second request  (`/oauth/token`) the server will return an error.

The request body should be passed in a standard `application/x-www-form-urlencoded` with a relevant header `Content-Type`. 

The following JSON will return:

```json
{
  "access_token": "{access_token}",
  "token_type": "bearer",
  "expires_in": 259200,
  "refresh_token": "{refresh_token}",
}
```

`authorization_code` has relatively short lifetime, so if it is expired, it is necessary to request a new one.

If the `authorization_code` exchange failed, the return will be `400 Bad Request` with the body:

```json
{
    "error": "...",
    "error_description": "..."
}
```

* `error` error will get  one of the values [described in RFC 6749 standard](http://tools.ietf.org/html/rfc6749#section-5.2).
 For instance, `invalid_request`, if one of the required parameters was not passed. 
* `error_description` will contain additional description of an error.


<a name="oauth_refresh_token"></a>

## Updating a pair of access and refresh tokens

`access_token` also has a lifetime (the key `expires_in`, in seconds), when it expires, the application will request `refresh_token` to get a new one. 

The request should be done in `application/x-www-form-urlencoded`. 

```
POST https://huntflow.ru/oauth/token
grant_type=refresh_token&refresh_token={refresh_token}
```

The response will be identical to the one when the token is given for the first time:

```json
{
  "access_token": "{access_token}",
  "token_type": "bearer",
  "expires_in": 259200,
  "refresh_token": "{refresh_token}",
}
```

`refresh_token` can be used only once and only when the `access_token` expires. 

After getting a new pair of `access_token` and `refresh_token`, they should be used in the following API requests
and requests for prolonging a token.



<a name="oauth_check_access_token"></a>

### Using and checking an `access_token`

The application should use received `access_token` for authorization passing it in the header as: 

```Authorization: Bearer <access_token>```

To test a token use the method [/me](user.md#me).

```http
GET /me HTTP/1.1
User-Agent: App/1.0 (incaseoffire@example.com)
Host: api.huntflow.ru
Authorization: Bearer <access_token>
```

[Authorization errors description](errors.md#oauth).
