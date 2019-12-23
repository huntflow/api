# Работа с заявками Кадрового Агенства

* [Добавление заявки на вакансию](#client-vacancy-request-new)
* [Получение заявки на вакансию](#client-vacancy-request-view)
* [Взятие заявки на вакансию в работу](#client-vacancy-request-start)
* [Удаление заявки на вакансию](#client-vacancy-request-delete)
* [Описание схемы заявки на вакансию](#account-client-vacancy-request-list)


<a name="client-vacancy-request-new"></a>
## Добавление заявки на вакансию

`POST /account/{account_id}/clients/{client_id}/client_vacancy_requests`

В теле запроса необходимо передать JSON вида:

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
    "comment": "Мои комментарии"
}
```

### Поля запроса

* `[]` обозначает, что значение данного ключа является массивом
* `a.b` обозначает объект `a` с ключом `b`

| Путь | Тип | Обязательный | Описание
| ---- | -------- | ------------ | --------
| contacts[].id | number | Да | Идентификатор контакта
| position | string | Да | Должность
| * | * | * | Поля согласно [схеме заявки на вакансию](#account-client-vacancy-request-list)



<a name="client-vacancy-request-view"></a>
## Получение заявки на вакансию

`GET /account/{account_id}/clients/{client_id}/client_vacancy_requests/{client_vacancy_requests_id}`  вернёт клиента с идентификатором `{client_vacancy_requests_id}`

```
{
    "id": 1,
    "created": "2019-12-12T07:22:39+03:00",
    "position": "Моя должность",
    "contacts": [{
        "id": 1
    }],
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

Имя | Тип | Описание
--- | --- | ---
id | number | Идентификатор заявки
created | datetime | Дата и время создания заявки
position | string | Должность
contacts[].id | number | Идентификатор контакта, привязанного к заявке
values | object | Значение заполненных полей заявки
account_info.id | number | Идентификатор пользователя, создавшего заявку
account_info.name | string | Имя пользователя, создавшего заявку
account_info.email | string | Email пользователя, создавшего заявку


<a name="client-vacancy-request-start"></a>
## Взятие заявки на вакансию в работу
Заявка берется в работу путем [создания вакансии](agency_vacancies.md#vacancy-add) с указанием идентификатора заявки в поле `client_vacancy_request`.

<a name="client-vacancy-request-delete"></a>
## Удаление заявки на вакансию

`DELETE /account/{account_id}/clients/{client_id}/client_vacancy_requests/{client_vacancy_requests_id}`

<a name="account-client-vacancy-request-list"></a>
## Описание схемы заявки на вакансию

`GET /account/{account_id}/account_client_vacancy_requests` вернёт список схем заявки на вакансию.

```
{
    "items": [{
        "account": 1862,
        "name": "",
        "active": true,
        "attendee_hint": null,
        "id": 4,
        "schema": {
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

Имя | Тип | Описание
 --- | --- | ---
id | number | Идентификатор схемы заявки
name | string | Название схемы
active | bool | Флаг активности схемы
schema | object | [Описание полей схемы](schema.md)
