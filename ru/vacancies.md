# Работа с вакансиями

* [Список вакансий](#vacancies)
* [Получение вакансии](#vacancy)
* [Состояние вакансии](#vacancy-states)

<a name="vacancies"></a>
## Получение списка вакансий

`GET /account/{account_id}/vacancies` вернёт список вакансий компании.

Принимаемые параметры:

* `mine` — логическое поле.
Если передается, то вернутся только вакансии, над которыми работает текущий пользователь.

* `opened` — логическое поле.
Если передается, то вернутся только открытые вакансии

* `count`, `page` — [параметры постраничного вывода](general.md#pagination).

```json
{
    "items": [
        {
            "id": 4531,
            "position": "Менеджер по продажам",
            "company": "Отдел продаж",
            "money": "30 000 + 3% от продаж",
            "deadline": "2017-04-27",
            "created": "2017-03-22T18:16:27+03:00",
            "vacancy_request": null,
            "priority": 0,
            "hidden": false,
            "state": "OPEN"
        },
        {
            "id": 4530,
            "position": "Программист Python",
            "company": "Отдел разработки",
            "money": "80 000 руб",
            "deadline": null,
            "created": "2017-03-22T18:16:27+03:00",
            "vacancy_request": null,
            "priority": 0,
            "hidden": false,
            "state": "CLOSED"
        }
    ],
    "total": 1,
    "page": 1,
    "count": 30
}
```

<a name="vacancies-result"></a>

Имя | Тип | Описание
 --- | --- | ---
 id |  number | Идентификатор вакансии
 position | string | Название вакансии (должности)
 company | string | Отдел, подразделение
 money | string | Зарплата
 deadline | date | Дата дедлайна по вакансии
 created | date+time | Дата и время создания вакансии
 vacancy_request | число | Идентификатор заявки на вакансию, из которой вакансия была создана
 priority | number | Приоритет вакансии (может быть или 0 (обычный), или 1 (высокий))
 hidden | bool | Скрыта ли вакансия от коллег
 state | string | [состояние вакансии](#vacancy-states)


<a name="vacancy"></a>
## Получение вакансии

`GET /account/{account_id}/vacancies/{vacancy_id}` вернёт вакансию с идентификатором `{vacancy_id}`

```json
{
    "id": 4531,
    "position": "Менеджер по продажам",
    "company": "Отдел продаж",
    "money": "30 000 + 3% от продаж",
    "deadline": "2017-04-27",
    "created": "2017-03-22T18:16:27+03:00",
    "vacancy_request": null,
    "priority": 0,
    "hidden": false,
    "state": "OPEN",
    "body": "<p>Пишу я вам, чего же <strong>боле</strong></p>",
    "files": [
        {
            "id": 15808,
            "name": "Снимок экрана 2017-04-10 в 11.00.13.png",            
            "content_type": "image/png",
            "url": "https://store.huntflow.ru/uploads/f/f/h/ffhov94xuqytbl16u8b9l3oeewdjpyoc.png"
        }
    ]
}
```

Поля с результатом аналогичны данным из [списка вакансий](#vacancies-result) плюс поля:

Имя | Тип | Описание
 --- | --- | ---
 body | string | Чистовое описание вакансии в формате HTML
 files | array | Список файлов, прикрепленных к вакансии


<a name="vacancy-states"></a>
## Состояние вакансии

Идентификатор | Описание
 --- | ---
 OPEN | Вакансия открыта
 CLOSED | Вакансия закрыта
 HOLD | Работа над вакансией приостановлена
