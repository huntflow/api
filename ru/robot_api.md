# Токены, полученные через интерфейс Хантфлоу

<a name="robot_refresh_token"></a>
## Обновление токена
Важно! Данный способ работает **только** для токенов, полученных через интерфейс Хантфлоу в разделе "Токены"

Новый access_token может быть получен только тогда, когда действие текущего access_token истекло.

`POST /token/refresh`

В теле запроса необходимо передать JSON вида:
```json
{
  "refresh_token": "%secret_refresh_token%",
  "grant_type": "refresh_token"
}
```
Пример ответа:
```json
{
    "access_token": "%new_access_token%",
    "token_type": "bearer",
    "expires_in": 604800,
    "refresh_token_expires_in": 1209600,
    "refresh_token": "%new_refresh_token%"
}
```