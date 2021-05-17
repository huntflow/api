# Work with vacancy requests

* [List of the vacancy requests schemas](#account-vacancy-request-list)
* [Vacancy request schema](#account-vacancy-request-view)
* [List of the vacancy requests](#vacancy-request-list)
* [Vacancy request](#vacancy-request-view)
* [Vacancy request creation](#vacancy-request-new)
* [Take vacancy request into work](#vacancy-request-start)

<a name="account-vacancy-request-list"></a>
## List of the vacancy requests schemas

`GET /account/{account_id}/account_vacancy_request`

Response example:

```
{
    "items": [
        {
            "schema": {
                "position": {
                    "type": "string",
                    "title": "Position",
                    "required": true,
                    "order": 1,
                    "id": 1,
                    "value": null,
                    "key": "position"
                },
                "company": {
                    "type": "string",
                    "title": "Division",
                    "required": false,
                    "order": 2,
                    "id": 2,
                    "value": null,
                    "key": "company"
                },
                "money": {
                    "type": "string",
                    "title": "Salary",
                    "required": false,
                    "order": 3,
                    "id": 3,
                    "value": null,
                    "key": "money",
                    "delimiter": true
                },
                "hard_skills": {
                    "type": "text",
                    "title": "Professional skills",
                    "required": true,
                    "order": 4,
                    "id": 4,
                    "value": null,
                    "key": null
                },
                "soft_skills": {
                    "type": "text",
                    "title": "Soft skills",
                    "required": true,
                    "order": 5,
                    "id": 5,
                    "value": null,
                    "key": null,
                    "delimiter": true
                },
                "comment": {
                    "type": "text",
                    "title": "Comments",
                    "required": false,
                    "order": 6,
                    "id": 6,
                    "value": null,
                    "key": null
                }
            },
            "id": 1,
            "name": "Test request schema",
            "attendee_hint": "",
            "attendee_required": null,
            "active": true
        }
    ]
}
```

Name |  Description
---- | --------
id | Vacancy request schema identifier
name | Schema name
attendee_required | Flag that controls "Send for approval" field (`attendees` object) on vacancy request creation (`null` – field disabled, `false` – field value is not required, `true` – field value is required)
attendee_hint | Hint for the "Send for approval" field
active | Is schema enabled for vacancy request creation
schema | [Fields schema description](schema.md)

<a name="account-vacancy-request-view"></a>
## Vacancy request schema

`GET /account/{account_id}/account_vacancy_request/{account_vacancy_request_id}`

<a name="vacancy-request-list"></a>
## List of the vacancy requests

`GET /account/{account_id}/vacancy_request`

It's also possible to pass `?vacancy_id={vacancy_id}` parameter with vacancy identifier to retrieve vacancy requests that were taken into work by the passed vacancy.

Response example:

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
            "id": 10,
            "files": [
                {
                    "id": 42,
                    "name": "Image 2021-02-01 в 00.00.00.png",
                    "content_type": "image/png",
                    "url": "https://store.huntflow.ru/uploads/f/f/h/ffhov94xuqytbl16u8b9l3oeewdjpyoc.png"
                }
            ]
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
            "position": "Manager",
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
                    "reason": "Position closed",
                    "order": 3,
                    "id": 3
                },
            ],
            "account_info": {
                "id": 1,
                "name": "Main",
                "email": "demo@org.com"
            },
            "position": "Director",
            "id": 6
        }
    ]
}
```

Name |  Description
---- | --------
status | [Vacancy request status](#vacancy-request-statuses)
account | Vacancy request author (user identifier)
account_vacancy_request | Vacancy request schema identifier
created | Vacancy request creation date and time
states[].status | [Vacancy request status](#vacancy-request-statuses)
states[].changed | Vacancy request last approval change date and time
states[].email | Approval email
states[].reason | Rejection reason
states[].order | Order number of approval process
states[].id | Approval identifier
account_info.id | Vacancy request author user identifier
account_info.name | Vacancy request author name
account_info.email | Vacancy request author nemail
position | Position name
id | Vacancy request identifier
files[].id | Attached file identifier
files[].name | File name
files[].content_type | File type
files[].url | File url

<a name="vacancy-request-statuses"></a>
## Vacancy request states
Name |  Description
-------- | --------
pending | Pending
approved | Approved
rejected | Rejected


<a name="vacancy-request-view"></a>
## Vacancy request

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
    "comment": "ASAP",
    "files": [1, 2, 3]
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

`files` - список файлов, прикрепленных к вакансии ([загрузка файлов](upload.md)) 

Допускается два варианта передачи полей справочников (в том числе подразделений):

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
