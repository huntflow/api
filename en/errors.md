# Errors and response codes

API uses reporting by HTTP response codes that the application should process adequately. 

Apart of the response code the body the response contains additional information, 
that helps the developer undestand the reason of an error. 

Errors are passed as:

```json
{
    "errors": [
        {
            "type": "applicants",
            "value": "first_name"
        },
        {
            "type": "applicants",
            "value": "last_name"
        }
    ]
}
```

The key `type` is present on every object and contains text identifier of the error class. 
The key `value` is optional. It specifies the error data.

While sending `POST/PUT` requests, that require sending forms, the error response object 
can contain the object `fields` with human friendly information about the error:

```json
{
    "errors": [],
    "fields": {
        "first_name": [
          "First name or last name required"
        ]
    }
}
```


## System error

If the service cannot process the request, `500 Internal Server Error` will return and the `type` field will contain
`server_error`.

Rarely the errors with 5xx codes will return without valid JSON in the body. 
The application should only concider the response code in these cases.


## Common request errors

<a name="user-agent"></a>
### Wrong User-Agent

More about [User-Agent header](general.md#request-requirements).

HTTP code | type | value | description
----------|------|-------|-----------
400 | bad_user_agent |  | User-Agent header is not passed


## Authorization errors

More about [authorization](authorization.md).


<a name="oauth"></a>
### Authorization usage errors

If the authorized [request is made](authorization.md#oauth_check_access_token) to the API and the authorization is invalid, 
the error with the `type` and `oauth` will return with one of the following `values`:

HTTP code | type | value | description
----------|------|-------|-----------
401 | oauth | bad_authorization | authorization token doesn't exist or invalid
401 | oauth | token_expired | access_token has expired [access_token refresh](authorization.md#oauth_refresh_token) is required 
401 | oauth | token_revoked | the token in called back by a user. The application has to [request a new authorization](authorization.md)
