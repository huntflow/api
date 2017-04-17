# Авторизация

Авторизация осуществляется по протоколу OAuth 2.0 ([RFC 6749](http://tools.ietf.org/html/rfc6749)).

Зарегистрированное приложение может запрашивать у пользователя
разрешение доступа к его данным без получения и хранения
логина и пароля.

<a name="oauth"></a>
## Получение авторизации
В начале приложению необходимо направить пользователя (открыть страницу)
по адресу:

```
https://huntflow.ru/oauth/authorize?response_type=code&client_id={client_id}&state={state}&redirect_uri={redirect_uri}
```

Обязательные параметры:

* `response_type=code` — указание на способ получение авторизации, используя
  authorization code
* `client_id={client_id}` — идентификатор приложения


Необязательные параметры:

* `state={state}` — в случае указания, будет включен в ответ, что
  позволяет исключить возможность взлома путём подделки межсайтовых
  запросов ([RFC 6749. Section 10.12](http://tools.ietf.org/html/rfc6749#section-10.12))
* `redirect_uri={redirect_uri}` — адрес для перенаправления пользователя после
  авторизации. По умолчанию используется адрес из настроек приложения.


<a name="oauth_process"></a>
### Процесс авторизации

Если пользователь не аутентифицирован на сайте, то ему будет показана форма логина. После прохождения аутентификации, пользователю
будет выведена форма с запросом разрешения доступа приложения к данным пользователя.

Если пользователь не разрешает доступ приложению, пользователь будет
перенаправлен на указанный `redirect_uri` с `?error=access_denied` и
`state={state}` (если он был указан).


<a name="oauth_authorization_code"></a>
### Успешное получение временного `authorization_code`

В случае разрешения прав, в редиректе будет указан
временный `authorization_code`:

```http
HTTP/1.1 302 Found
Location: {redirect_uri}?code={authorization_code}
```

Если пользователь аутентифицирован на сайте и доступ данному приложению однажды
ранее выдан, ответом будет сразу вышеописанный редирект с `authorization_code`
(без показа формы логина и выдачи прав).


<a name="oauth_tokens"></a>
### Получение access и refresh токенов

После получения `authorization_code` приложению необходимо сделать сервер-сервер
POST-запрос на `https://huntflow.ru/oauth/token` для обмена полученного
`authorization_code` на `access_token`.

В запросе необходимо передать:

```
grant_type=authorization_code&client_id={client_id}&client_secret={client_secret}&code={authorization_code}&redirect_uri={redirect_uri}
```

Если при получении `authorization_code` был указан `redirect_uri`, то в запросе
необходимо обязательно передать это значение (происходит сравнение строк),
иначе этот параметр необязателен. Если же не указать `redirect_uri` при запросе
на `/oauth/authorize`, то при указании его во втором запросе (`/oauth/token`)
сервер вернёт ошибку.

Тело запроса необходимо передавать в стандартном
`application/x-www-form-urlencoded` с указанием соответствующего заголовка
`Content-Type`.

В ответе вернётся JSON:

```json
{
  "access_token": "{access_token}",
  "token_type": "bearer",
  "expires_in": 259200,
  "refresh_token": "{refresh_token}",
}
```

`authorization_code` имеет довольно короткий срок жизни, при его истечении
необходимо запросить новый.

Если обмен `authorization_code` произвести не удалось, то вернётся ответ `400 Bad Request` с телом:

```json
{
    "error": "...",
    "error_description": "..."
}
```

* `error` будет иметь одно из значений,
  [описанных в стандарте RFC 6749](http://tools.ietf.org/html/rfc6749#section-5.2).
  Например, `invalid_request`, если какой либо из обязательных параметров не был передан.
* `error_description` будет содержать дополнительное описание ошибки.


<a name="oauth_refresh_token"></a>
## Обновление пары access и refresh токенов
`access_token` также имеет срок жизни (ключ `expires_in`, в секундах), при его
истечении приложение должно сделать запрос с `refresh_token` для получения
нового.

Запрос необходимо делать в `application/x-www-form-urlencoded`.

```
POST https://huntflow.ru/oauth/token
grant_type=refresh_token&refresh_token={refresh_token}
```

Ответ будет идентичен ответу на получения токенов в первый раз:

```json
{
  "access_token": "{access_token}",
  "token_type": "bearer",
  "expires_in": 259200,
  "refresh_token": "{refresh_token}",
}
```

`refresh_token` можно использовать только один раз и только по истечению
срока действия `access_token`.

После получения новой пары `access_token` и `refresh_token`, их необходимо использовать
в дальнейших запросах в API и запросах на продление токена.


<a name="oauth_check_access_token"></a>
### Использование и проверка access_token

Приложение должно использовать полученный `access_token` для авторизации, 
передавая его в заголовке в формате:

```Authorization: Bearer <access_token>```

Для тестирования токена, удобно использовать метод [/me](user.md#me).

```http
GET /me HTTP/1.1
User-Agent: App/1.0 (incaseoffire@example.com)
Host: api.huntflow.ru
Accept: */*
Authorization: Bearer <access_token>
```

[Описание ошибок авторизации](errors.md#oauth).
