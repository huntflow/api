# Работа с метками

* [Получение меток организации](#account-tags-list)
* [Информация о метки организации](#account-tags-view)
* [Создание меток организации](#account-tags-create)
* [Редактирование меток организации](#account-tags-edit)
* [Удаление меток организации](#account-tags-delete)
* [Получение меток кандидата](#applicant-tags-list)
* [Информация о метки кандидата](#applicant-tags-view)
* [Назначение меток кандидату](#applicant-tags-edit)
* [Удаление меток кандидата](#applicant-tags-delete)

<a name="account-tags-list"></a>
## Получение меток организации

`GET /account/{account_id}/tag` вернёт список меток компании.

```json
{
    "items": [
        {
            "color": "000000",
            "id": 185,
            "name": "Черный список"
        },
        {
            "color": "d7b100",
            "id": 186,
            "name": "Резерв"
        },
        {
            "color": "00ad3b",
            "id": 187,
            "name": "Рекомендация"
        }
    ]
}
```

Имя | Тип | Описание
 --- | --- | ---
 id |  number | Идентификатор метки
 color | string | Цвет метки в формате hex
 name | string | Название метки
 
<a name="account-tags-view"></a>
## Информация о метки организации

`GET /account/{account_id}/tag/{tag_id}` вернёт метку компании.

```json
{
    "color": "000000",
    "id": 185,
    "name": "Черный список"
}
```

<a name="account-tags-create"></a>
## Создание метки организации

`POST /account/{account_id}/tag`
В теле запроса необходимо передать JSON вида:

```
{
    "color": "d7b100",
    "name": "Резерв"
}
```

## Поля запроса
Имя | Тип | Обязательный | Описание
 --- | --- | --- | ---
 color | string | Да | Цвет метки в формате hex
 name | string | Да | Название метки

## Поля ответа
Дополнительно к полям запроса также будут добавлены поля:

```
{
    "id": 1
}
```
Имя | Тип | Описание
 --- | --- | ---
 id |  number | Идентификатор метки
 
<a name="account-tags-edit"></a>
## Редактирование метки организации

`PUT /account/{account_id}/tag/{tag_id}`
В теле запроса необходимо передать JSON вида:

```
{
    "color": "d7b500",
    "name": "Измененный резерв"
}
```

## Поля запроса
См. описание полей в [запросе создания метки организации](#account-tags-create)
 
<a name="account-tags-delete"></a>
## Удаление метки организации

`DELETE /account/{account_id}/tag/{tag_id}`
 
<a name="applicant-tags-list"></a>
## Получение меток кандидата

`GET /account/{account_id}/applicants/{applicant_id}/tag` вернёт список меток кандидата.

```
{
    "items": [
        {
            "tag": 4,
            "id": 16
        },
        {
            "tag": 5,
            "id": 17
        }
    ]
}
```

Имя | Тип | Описание
 --- | --- | ---
 id |  number | Идентификатор метки кандидата
 tag | number | Идентификатор [метки](#account-tags-list)
 
 <a name="applicant-tags-view"></a>
## Информация о метки кандидата

`GET /account/{account_id}/applicants/{applicant_id}/tag/{tag_id}` вернёт информацию о метки кандидата.

```
{
    "tag": 4,
    "id": 16
}
```

 
<a name="applicant-tags-edit"></a>
## Назначение меток кандидату

`POST /account/{account_id}/applicants/{applicant_id}/tag`
В теле запроса необходимо передать JSON вида:

```
{
    "tag": 11
}
```
## Поля запроса
Имя | Тип | Обязательный | Описание
 --- | --- | --- | ---
 tag | number | Да | Идентификатор [метки](#account-tags-list)
 
## Поля ответа
Поля ответа идентичны полям запроса.

<a name="applicant-tags-delete"></a>
## Удаление меток организации

`DELETE /account/{account_id}/applicants/{applicant_id}/tag/{tag_id}`
