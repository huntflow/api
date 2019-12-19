# Работа с контактами Кадрового Агенства

* [Добавление контакта](#contact-add)
* [Получение списка контактов](#contact-list)
* [Удаление контакта](#contact-delete)

<a name="contact-add"></a>
## Добавление контакта

Контакт добавляется при [добавлении клиента](agency_clients.md#client-add)

Или методом `POST /account/{account_id}/clients/{client_id}/contacts`

В теле запроса необходимо передать JSON вида:

```json
{
    "last_name": "Петров",
    "first_name": "Петр",
    "middle_name": "Петрович",
    "birthday": "1975-01-26",
    "cell_phone": "79111111111",
    "work_phone": "79111111111",
    "email": "test@example.com",
    "skype": "test",
    "comment": "Комментарий",
    "position": "Глава отдела кадров"
}
```

### Поля запроса

* `[]` обозначает, что значение данного ключа является массивом
* `a.b` обозначает объект `a` с ключом `b`

Путь | Тип | Обязательный | Описание
---- | -------- | ------------ | --------
last_name | string | Да | Фамилия
first_name | string | Да | Имя
middle_name | string | Нет | Отчество
birthday | date | Нет | День рождения
cell_phone | string | Нет | Мобильный телефон
work_phone | string | Нет | Рабочий телефон
email | string | Нет | Эл. почта
skype | string | Нет | Скайп
comment | string | Нет | Комментарий
position | string | Нет | Должность


### Поля ответа

Дополнительно к полям запроса также будут добавлены поля:

```json
{
    "id": 1,
    "created": "2017-04-10T21:34:01+03:00"
}
```

Имя | Тип | Описание
--- | --- | ---
id | number | Идентификатор контакта
created | datetime | Дата и время создания контакта


<a name="contact-list"></a>
## Получение списка контактов

`GET /account/{account_id}/clients/{client_id}/contacts/` вернёт список контактов.

Принимаемые параметры:
* `count`, `page` — [параметры постраничного вывода](general.md#pagination).

```json
{
    "items": [
        {
            "id": 1,
            "created": "2017-04-10T21:34:01+03:00",
            "last_name": "Петров",
            "first_name": "Петр",
            "middle_name": "Петрович",
            "birthday": "1975-01-26",
            "cell_phone": "79111111111",
            "work_phone": "79111111111",
            "email": "test@example.com",
            "skype": "test",
            "comment": "Комментарий",
            "position": "Глава отдела кадров"
        }
    ],
    "total": 1,
    "page": 1,
    "count": 30
}
```

### Поля ответа

Путь | Тип | Описание
---- | -------- | --------
id | number | Идентификатор контакта
created | datetime | Дата и время создания контакта
last_name | string | Фамилия
first_name | string | Имя
middle_name | string | Отчество
birthday | date | День рождения
cell_phone | string | Мобильный телефон
work_phone | string | Рабочий телефон
email | string | Эл. почта
skype | string | Скайп
comment | string | Комментарий
position | string | Должность

<a name="contact-delete"></a>
## Удаление контакта

`DELETE /account/{account_id}/clients/{client_id}/contacts/{contact_id}`
