# Работа с клиентами Кадрового Агенства

* [Добавление клиента в базу](#add)
* [Редактирование клиента](#edit)
* [Список клиентов](#clients)
* [Получение клиента](#client)

<a name="add"></a>
## Добавление клиента в базу

`POST /account/{account_id}/clients`

В теле запроса необходимо передать JSON вида:

```json
{
    "name": "Мой клиент",
    "condition": "Некоторый текст",
    "address": "Москва",
    "phone": "79111111111",
    "email": "hello@mail.ru",
    "contacts": [{
        "last_name": "Петров",
        "first_name": "Виталий",
        "middle_name": "Иванович",
        "birthday": "1975-01-26",
        "cell_phone": "79111111111",
        "work_phone": "79222222222",
        "email": "petrov@mail.ru",
        "skype": "petrov",
        "comment": "Другой текст",
        "position": "CEO"
    }]
}
```

### Поля запроса

* `[]` обозначает, что значение данного ключа является массивом
* `a.b` обозначает объект `a` с ключом `b`

Путь | Тип | Обязательный | Описание
---- | -------- | ------------ | --------
name | string | Да | Название клиента
condition | string | Нет | Условия работы
address | string | Нет | Адрес офиса
phone | string | Нет | Телефон
email | string | Нет | Эл. почта
contacts[].last_name | string | Нет | Фамилия
contacts[].first_name | string | Нет | Имя
contacts[].middle_name | string | Нет | Отчество
contacts[].birthday | date | Нет | Дата рождения
contacts[].cell_phone | string | Нет | Мобильный телефон
contacts[].work_phone | string | Нет | Рабочий телефон
contacts[].email | string | Нет | Эл. почта
contacts[].skype | string | Нет | Скайп
contacts[].comment | string | Нет | Комментарий
contacts[].position | string | Нет | Должность

### Поля ответа

Дополнительно к полям запроса также будут добавлены поля:

```json
{
    "id": 1,
    "created": "2017-04-10T21:34:01+03:00",
    "contacts": [{
        "id": 1,
        "created": "2017-04-10T21:34:01+03:00"
    }]
}
```

Имя | Тип | Описание
--- | --- | ---
id | number | Идентификатор клиента
created | datetime | Дата и время создания клиента
contacts[].position | ? | ?


<a name="clients"></a>
## Получение списка клиентов

`GET /account/{account_id}/clients` вернёт список клиентов.

Принимаемые параметры:
* `count`, `page` — [параметры постраничного вывода](general.md#pagination).

```json
{
    "items": [
        {
            "id": 2,
            "created": "2017-12-19T21:35:09+03:00",
            "name": "Мой клиент 2",
            "condition": null,
            "address": null,
            "phone": null,
            "email": "",
            "contacts": [
                {
                    "id": 2,
                    "created": "2017-12-19T21:35:09+03:00",
                    "last_name": "Иванов",
                    "first_name": null,
                    "middle_name": null,
                    "birthday": null,
                    "cell_phone": null,
                    "work_phone": null,
                    "email": "",
                    "skype": null,
                    "comment": null,
                    "position": null
                },
                {
                    "id": 3,
                    "created": "2017-12-19T21:35:09+03:00",
                    "last_name": "Сидоров",
                    "first_name": null,
                    "middle_name": null,
                    "birthday": null,
                    "cell_phone": null,
                    "work_phone": null,
                    "email": "",
                    "skype": null,
                    "comment": null,
                    "position": null
                }
            ]
        },
        {
            "id": 1,
            "created": "2017-04-10T21:34:01+03:00",
            "name": "Мой клиент",
            "condition": "Некоторый текст",
            "address": "Москва",
            "phone": "79111111111",
            "email": "hello@mail.ru",
            "contacts": [{
                "id": 1,
                "created": "2017-04-10T21:34:01+03:00",
                "last_name": "Петров",
                "first_name": "Виталий",
                "middle_name": "Иванович",
                "birthday": "1975-01-26",
                "cell_phone": "79111111111",
                "work_phone": "79222222222",
                "email": "petrov@mail.ru",
                "skype": "petrov",
                "comment": "Другой текст",
                "position": "CEO"
            }]
        }
    ],
    "total": 2,
    "page": 1,
    "count": 30
}
```

<a name="clients-result"></a>

Имя | Тип | Описание
 --- | --- | ---
 id | number | Идентификатор клиента
 created | datetime | Дата и время создания клиента
 name | string | Название клиента
 condition | string | Условия работы
 address | string | Адрес офиса
 phone | string | Телефон
 email | string | Эл. почта
 contacts[].id | number | Идентификатор контакта
 contacts[].created | datetime | Дата и время создания контакта
 contacts[].last_name | string | Фамилия
 contacts[].first_name | string | Имя
 contacts[].middle_name | string | Отчество
 contacts[].birthday | date | Дата рождения
 contacts[].cell_phone | string | Мобильный телефон
 contacts[].work_phone | string | Рабочий телефон
 contacts[].email | string | Эл. почта
 contacts[].skype | string | Скайп
 contacts[].comment | string | Комментарий
 contacts[].position | string | Должность


<a name="client"></a>
## Получение клиента

`GET /account/{account_id}/clients/{client_id}` вернёт клиента с идентификатором `{client_id}`

```json
{
    "id": 1,
    "created": "2017-04-10T21:34:01+03:00",
    "name": "Мой клиент",
    "condition": "Некоторый текст",
    "address": "Москва",
    "phone": "79111111111",
    "email": "hello@mail.ru",
    "contacts": [{
        "id": 1,
        "created": "2017-04-10T21:34:01+03:00",
        "last_name": "Петров",
        "first_name": "Виталий",
        "middle_name": "Иванович",
        "birthday": "1975-01-26",
        "cell_phone": "79111111111",
        "work_phone": "79222222222",
        "email": "petrov@mail.ru",
        "skype": "petrov",
        "comment": "Другой текст",
        "position": "CEO"
    }]
}
```

Поля с результатом аналогичны данным из [списка клиентов](#clients-result) плюс поля:
