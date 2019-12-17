# Работа с заявками Кадрового Агенства

* [Получение списка схем заявки на вакансию для Кадрового Агенства](#account-client-vacancy-request-list)
* [Получение схемы заявки на вакансию для Кадрового Агенства](#account-client-vacancy-request-view)
* [Получение заявки на вакансию для Кадрового Агенства](#client-vacancy-request-view)
* [Создание заявки на вакансию для Кадрового Агенства](#client-vacancy-request-new)
* [Взятие заявки на вакансию для Кадрового Агенства в работу](#client-vacancy-request-start)

<a name="account-client-vacancy-request-list"></a>
## Получение списка схем заявки на вакансию для Кадрового Агенства

`GET /account/{account_id}/account_client_vacancy_requests`

Пример ответа:

```
{
    "items": [{
        "account": 1862,
        "name": "",
        "active": true,
        "attendee_hint": null,
        "id": 4,
        "schema": {
            "position": {
                "title": "Должность",
                "required": true,
                "type": "string",
                "order": 1,
                "value": null,
                "id": 37
            },
            "duties": {
                "title": "Обязанности",
                "required": true,
                "type": "text",
                "order": 2,
                "value": null,
                "id": 38
            },
            "skills": {
                "title": "Профессиональные навыки",
                "required": true,
                "type": "text",
                "order": 3,
                "value": null,
                "id": 39
            },
            "soft_skills": {
                "title": "Личностные качества",
                "required": false,
                "type": "text",
                "order": 4,
                "value": null,
                "id": 40
            },
            "langs": {
                "delimiter": true,
                "title": "Иностранные языки",
                "required": false,
                "type": "text",
                "order": 5,
                "value": null,
                "id": 41
            },
            "money": {
                "title": "Оклад",
                "required": false,
                "type": "string",
                "order": 6,
                "value": null,
                "id": 42
            },
            "bonus": {
                "title": "Бонус",
                "required": false,
                "type": "string",
                "order": 7,
                "value": null,
                "id": 43
            },
            "fringe_benefits": {
                "delimiter": true,
                "title": "Социальный пакет",
                "required": false,
                "type": "string",
                "order": 8,
                "value": null,
                "id": 44
            },
            "reason": {
                "values": [
                    "Новая вакансия",
                    "Замена текущего сотрудника",
                    "Повышение текущего сотрудника"
                ],
                "title": "Причины открытия вакансии",
                "required": true,
                "type": "select",
                "order": 9,
                "value": null,
                "id": 45
            },
            "sources": {
                "title": "Источники поиска (компании, конкуренты)",
                "required": false,
                "type": "text",
                "order": 10,
                "value": null,
                "id": 46
            },
            "start_work": {
                "title": "Ожидаемая дата начала работы кандидата",
                "required": true,
                "type": "date",
                "order": 11,
                "value": null,
                "id": 47
            },
            "comment": {
                "title": "Комментарии",
                "required": false,
                "type": "text",
                "order": 12,
                "value": null,
                "id": 48
            }
        }
    }]
}
```

Путь |  Описание
---- | --------
id | Идентификатор схемы заявки
name | Название схемы
active | Флаг активности схемы
schema | [Описание полей схемы](schema.md)

<a name="account-client-vacancy-request-view"></a>
## Получение схемы заявки на вакансию для Кадрового Агенства

`GET /account/{account_id}/account_client_vacancy_requests/{account_client_vacancy_requests}`


<a name="client-vacancy-request-view"></a>
## Получение заявки на вакансию для Кадрового Агенства

`GET /account/{account_id}/clients/{client_id}/client_vacancy_requests/{client_vacancy_requests_id}`

Пример ответа:

```
{
    "id": 1,
    "created": "2019-12-12T07:22:39+03:00",
    "contacts": [{
        "id": 1
    }],
    "account_client_vacancy_request": 1,
    "values": {
        "comment": null,
        "start_work": "07.12.2019",
        "sources": null,
        "skills": "test",
        "money": "111",
        "fringe_benefits": "111",
        "bonus": "111",
        "reason": "Новая вакансия",
        "duties": "test",
        "position": "test",
        "langs": "test",
        "soft_skills": "test"
    },
    "account_info": {
        "email": "manager@example.com",
        "id": 1,
        "name": "Manager 1"
    }
}
```

Путь |  Описание
---- | --------
id | Идентификатор заявки
created | Дата и время создания заявки
account_info.id | Идентификатор пользователя, создавшего заявку
account_info.name | Имя пользователя, создавшего заявку
account_info.email | Email пользователя, создавшего заявку
account_client_vacancy_request | Идентификатор схемы заявки
contacts[].id | Идентификатор контакта, привязанного к заявке
values | Значение заполненных полей заявки


<a name="client-vacancy-request-new"></a>
## Создание заявки на вакансию для Кадрового Агенства

`POST /account/{account_id}/clients/{client_id}/client_vacancy_requests`


Пример запроса:

```
{
    "contacts": [{
        "id": 1
    }],
    "position": "Моя должность",
    "duties": "Мои обязанности",
    "skills": "Мои профессиональные навыки",
    "soft_skills": "Мои личностные качества",
    "langs": "Мои иностранные языки",
    "money": "10000",
    "bonus": "0",
    "fringe_benefits": "Мой социальный пакет",
    "reason": "Новая вакансия",
    "sources": "Мой источник",
    "start_work": "07.12.2019",
    "comment": "Мои комментарии",
    "account_client_vacancy_request": 1
}
```

Необязательный параметр `account_client_vacancy_request` --- идентификатор формы заявки на вакансию, которую можно получить [здесь](#account-client-vacancy-request-list). Если не указано, то используется последняя схема заявки.


<a name="client-vacancy-request-start"></a>
## Взятие заявки на вакансию для Кадрового Агенства в работу
Заявка берется в работу путем [создания вакансии для Кадрового Агентсва](agency_vacancies.md#add) по этой заявке с указанием идентификатора заявки в поле `client_vacancy_request`.
