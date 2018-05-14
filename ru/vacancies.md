# Работа с вакансиями

* [Добавление вакансии в базу](#add)
* [Редактирование вакансии](#edit)
* [Список вакансий](#vacancies)
* [Получение вакансии](#vacancy)
* [Удаление вакансии](#delete)
* [Состояние вакансии](#vacancy-states)

<a name="add"></a>
## Добавление вакансии в базу

`POST /account/{account_id}/vacancies`

В теле запроса необходимо передать JSON вида:

```json
{
    "position": "Manufacturing Engineer",
    "company": "Tesla",
    "money": "$100k",
    "deadline": "2017-09-03",
    "priority": 1,
    "account_division": 6,
    "coworkers": [1],
    "body": "<p>Some text</p>",
    "requirements": "<p>Another text</p>",
    "conditions": "<p>Different text</p>",
    "hidden": false,
    "state": "OPEN",
    "files": [1, 2, 3]
}
```

### Поля запроса

Имя | Тип | Обязательный | Описание
 --- | --- | --- | ---
 position | string | Да | Название вакансии (должности)
 company | string | Нет | Отдел, подразделение (игнорируется, если подключены подразделения)
 money | string | Нет | Зарплата
 applicants_to_hire | number | Нет | Сколько человек нужно нанять (>= 1)
 deadline | date | Нет | Дата дедлайна по вакансии
 priority | number | Нет | Приоритет вакансии (может быть или 0 (обычный), или 1 (высокий))
 account_division | number | Нет | Идентификатор подразделения (если подразделения подключены)
 coworkers | array | Нет | Список рекрутеров, работающих над вакансией
 body | string | Нет | Обязанности в формате HTML. Доступные теги: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**, **b**, **i**
 requirements | string | Нет | Требования в формате HTML. Доступные теги: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**, **b**, **i**
 conditions | string | Нет | Условия работы в формате HTML. Доступные теги: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**,**b**, **i**
 hidden | bool | Нет | Скрыта ли вакансия от коллег
 state | string | Нет | [Состояние вакансии](#vacancy-states). По умолчанию `OPEN`
 files | array | Нет | Список файлов, прикрепленных к вакансии ([загрузка файлов](upload.md))

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
id | number | Идентификатор вакансии
created | string | Дата+время создания вакансии

<a name="edit"></a>
## Редактирование вакансии

`PUT /account/{account_id}/vacancies/{vacancy_id}`

### Поля запроса
Тело запроса аналогично телу в запросе на создание вакансии.

### Поля ответа
Ответ аналогичен ответу на запрос на создание вакансии.

<a name="delete"></a>
# Удаление вакансии

`DELETE /account/{account_id}/vacancies/{vacancy_id}`

### Поля запроса
Тело запроса должно быть пустым

# Поля ответа
```json
{
    "status": true
}
```

Имя | Тип | Описание
--- | --- | ---
status | bool | Флаг успешной операции

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
            "applicants_to_hire": 1,
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
            "applicants_to_hire": 1,
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
 applicants_to_hire | number | Количество кандидатов к найму
 created | date+time | Дата и время создания вакансии
 vacancy_request | number | Идентификатор заявки на вакансию, из которой вакансия была создана
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
    "applicants_to_hire": 1,
    "created": "2017-03-22T18:16:27+03:00",
    "vacancy_request": null,
    "priority": 0,
    "hidden": false,
    "state": "OPEN",
    "body": "<p>Пишу я вам, чего же <strong>боле</strong></p>",
    "requirements": "<p>Что я могу ещё <strong>сказать</strong></p>",
    "conditions": "<p>Теперь я знаю в вашей воле <strong>воле</strong></p>",
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
 body | string | Обязанности в формате HTML
 requirements | string | Требования в формате HTML
 conditions | string | Условия работы в формате HTML
 files | array | Список файлов, прикрепленных к вакансии


<a name="vacancy-states"></a>
## Состояние вакансии

Идентификатор | Описание
 --- | ---
 OPEN | Вакансия открыта
 CLOSED | Вакансия закрыта
 HOLD | Работа над вакансией приостановлена
