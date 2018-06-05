# Basic information

* [Getting information about the current user](#me)
* [Getting information about available organizations](#organizations)

<a name="me"></a>
## Getting information about the current user

`GET /me` returns information about the current user. In case of invalid authorization the server will return the message `403 Forbidden`.

```json
{
    "id": 3123123,
    "name": "John Doe",
    "position": "HRD",
    "email": "hello@huntflow.ru",
    "phone": "84956478221",
    "locale": "ru_RU"
}
```


 Name | Type | Description
 --- | --- | ---
 id | number | User ID
 name | string | User name
 position | string | User occupation
 email | string | Email adress
 phone | string | Phone number
 locale | string | The user's locale ([the guide on locales](dicts.md#locale))


<a name="organizations"></a>
## Getting information about available organizations

`GET /accounts` returns the list of user's available organizations.

```json
{
    "items": [
        {
            "id": 123,
            "name": "Tesla",
            "nick": "tesla",
            "member_type": "owner"
        },
        {
            "id": 125,
            "name": "HR systmes",
            "nick": "hr-systems",
            "member_type": "manager"
        }
    ]
}
```


Name | Type | Description
 --- | --- | ---
 id | number | Organization ID (used for organization data requests)
 name | string | The name of the organization
 nick | string | Short organization name (used on the website, e.g. /my/tellur)
 member_type | string | The role in the organization (the description of roles: [the guide on roles](dicts.md#member_type))
 
