# Работа с заявками

* [Получние списка схем заявки на вакансию](#account-vacancy-request-list)
* [Получение схемы заявки на вакансию](#account-vacancy-request-view)
* [Получение списка заявок на вакансию](#vacancy-request-list)
* [Получение заявки на вакансию](#vacancy-request-view)
* [Создание заявки на вакансию](#vacancy-request-new)
* [Взятие заявки в работу](#vacancy-request-start)

<a name="account-vacancy-request-list"></a>
## Получние списка схем заявки на вакансию

`GET /account/{account_id}/account_vacancy_request`

Пример ответа:

```
{
    "items": [
        {
            "schema": {
                "position": {
                    "type": "string",
                    "title": "Должность",
                    "required": true,
                    "order": 1,
                    "id": 1,
                    "value": null,
                    "key": "position"
                },
                "company": {
                    "type": "string",
                    "title": "Отдел, подразделение",
                    "required": false,
                    "order": 2,
                    "id": 2,
                    "value": null,
                    "key": "company"
                },
                "money": {
                    "type": "string",
                    "title": "Зарплата",
                    "required": false,
                    "order": 3,
                    "id": 3,
                    "value": null,
                    "key": "money",
                    "delimiter": true
                },
                "hard_skills": {
                    "type": "text",
                    "title": "Профессиональные качества кандидата",
                    "required": true,
                    "order": 4,
                    "id": 4,
                    "value": null,
                    "key": null
                },
                "soft_skills": {
                    "type": "text",
                    "title": "Личностные качества кандидата",
                    "required": true,
                    "order": 5,
                    "id": 5,
                    "value": null,
                    "key": null,
                    "delimiter": true
                },
                "comment": {
                    "type": "text",
                    "title": "Комментарии",
                    "required": false,
                    "order": 6,
                    "id": 6,
                    "value": null,
                    "key": null
                }
            },
            "id": 1,
            "name": "Тестовая",
            "attendee_hint": "",
            "attendee_required": null,
            "active": true
        }
    ]
}
```

Путь |  Описание
---- | --------
id | Идентификатор схемы заявки
name | Название схемы
attendee_required | Флаг наличия поля "Отправить на согласование" при создании заявки (`null` –– поле отсутствует, `false` —— поле необязательное, `true` —— поле обязательное)
attendee_hint | Подсказка под полем  "Отправить на согласование"
active | Флаг активности схемы
schema | [Описание полей схемы](schema.md)

<a name="account-vacancy-request-view"></a>
## Получение схемы заявки на вакансию

`GET /account/{account_id}/account_vacancy_request/{account_vacancy_request_id}`

<a name="vacancy-request-list"></a>
## Получение списка заявок на вакансию

`GET /account/{account_id}/vacancy_request`

Также, можно передать параметр `?vacancy_id={vacancy_id}` с идентификатором вакансии для получения заявок, которые были взяты в работу по вакансии.

Пример ответа:

```
{
    "items": [
        {
            "status": "approved",
            "account": 1,
            "account_vacancy_request": 1,
            "created": "2018-07-10T14:50:21+03:00",
            "states": [],
            "account_info": {
                "id": 1,
                "name": "Main",
                "email": "demo@org.com"
            },
            "position": "position",
            "id": 10
        },
        {
            "status": "approved",
            "account": 1,
            "account_vacancy_request": 1,
            "created": "2018-07-10T14:50:03+03:00",
            "states": [],
            "account_info": {
                "id": 1,
                "name": "Main",
                "email": "demo@org.com"
            },
            "position": "position",
            "id": 9
        },
        {
            "status": "approved",
            "account": 3,
            "account_vacancy_request": 1,
            "created": "2018-07-10T12:26:00+03:00",
            "states": [],
            "account_info": {
                "id": 3,
                "name": "Recruiter Pavel",
                "email": "rec@org.com"
            },
            "position": "Менеджер",
            "id": 7
        },
        {
            "status": "rejected",
            "account": 1,
            "account_vacancy_request": 1,
            "created": "2018-07-10T12:25:42+03:00",
            "states": [
                {
                    "status": "PENDING",
                    "changed": null,
                    "email": "first@mail.com",
                    "reason": null,
                    "order": 1,
                    "id": 1
                },
                {
                    "status": "APPROVED",
                    "changed": "2018-07-11 12:56:30",
                    "email": "second@mail.com",
                    "reason": null,
                    "order": 2,
                    "id": 2
                },
                {
                    "status": "REJECTED",
                    "changed": "2018-07-11 13:00:09",
                    "vacancy_request": 13,
                    "email": "third@mail.com",
                    "reason": "Позиция закрыта",
                    "order": 3,
                    "id": 3
                },
            ],
            "account_info": {
                "id": 1,
                "name": "Main",
                "email": "demo@org.com"
            },
            "position": "Директор",
            "id": 6
        }
    ]
}
```

Путь |  Описание
---- | --------
status | [Статус заявки](#vacancy-request-statuses)
account | Идентификатор пользователя, создавшего заявку
account_vacancy_request | Идентификатор схемы заявки
created | Дата и время создания заявки
states[].status | [Статус](#vacancy-request-statuses) согласования
states[].changed | Дата и время последнего изменения согласования
states[].email | Email, по которому была отправлена заявка на согласование
states[].reason | Причина отказа
states[].order | Порядковый номер согласования
states[].id | Идентификатор согласования
account_info.id | Идентификатор пользователя, создавшего заявку
account_info.name | Имя пользователя, создавшего заявку
account_info.email | Email пользователя, создавшего заявку
position | Название позиции
id | Идентификатор заявки

<a name="vacancy-request-statuses"></a>
## Статусы заявки
Название |  Описание
-------- | --------
pending | Ожидание
approved | Согласовано
rejected | Отказано


<a name="vacancy-request-view"></a>
## Получение заявки на вакансию

`GET /account/{account_id}/vacancy_request/{vacancy_request_id}`


<a name="vacancy-request-new"></a>
## Создание заявки на вакансию

`POST /account/{account_id}/vacancy_request`


Пример запроса:

```
{
    "account_vacancy_request": 1
    "position": "Директор",
    "money": "25000",
    "hard_skills": "Опыт руководства производством не менее 80 лет",
    "soft_skills": "Коммуникабельность, целеустремленность, стрессоустойчивость",
    "comment": "ASAP"
}
```

Пример запроса с согласованием заявки:

```
{
    "account_vacancy_request": 1,    
    "position": "Директор",
    "money": "25000",
    "hard_skills": "Опыт руководства производством не менее 80 лет",
    "soft_skills": "Коммуникабельность, целеустремленность, стрессоустойчивость",
    "comment": "ASAP",
    "attendees": [
        {
            "email": "test@example.com",
            "displayName": "Ivanov Ivan"
        },
        {
            "email": "sendnext@example.com",
            "displayName": "Petrov Petr"
        }
    ]
}
```

Поле `attendees[].displayName` опционально. Заявка будет отправлена сначала первому согласующему, а после подтверждения – второму.

`account_vacancy_request` – идентификатор формы заявки на вакансию, которую можно получить [здесь](#account-vacancy-request-list).

Допускается два варианта передачи полей справочников:

-  [Получить справочник](dictionaries.md) и использовать id поля, например:

```
{
...
    "dictionary_field": 42,
...
}
``` 

- Использовать значение `foreign`, указанное при создании справочника, например:

```
{
...
    "dictionary_field": {"foreign": "field_foreign_value"},
...
}
``` 

В данном случае, для корректной работы необходимо, чтобы в рамках одного словаря не было полей с повторяющимся значением `foreign`.

<a name="vacancy-request-start"></a>
## Взятие заявки в работу
Заявка берется в работу путем [создания вакансии по этой заявке](vacancies.md#add) с указанием идентификатора заявки в поле `vacancy_request`.
