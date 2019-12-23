# Работа с вакансиями Кадрового Агенства

* [Добавление вакансии](#vacancy-add)
* [Редактирование вакансии](#vacancy-edit)
* [Список вакансий](#vacancy-list)
* [Получение вакансии](#vacancy-view)
* [Удаление вакансии](#vacancy-delete)

<a name="vacancy-add"></a>
## Добавление вакансии

Если организация --- **не** Кадровое Агенство, то используется [другой набор полей](vacancies.md#add).

`POST /account/{account_id}/vacancies`

В теле запроса необходимо передать JSON вида:

```json
{
    "client_vacancy_request": 11,
    "deadline": "2030-09-03",
    "applicants_to_hire": 2,
    "priority": 1,
    "body": "<p>Some text</p>",
    "requirements": "<p>Another text</p>",
    "conditions": "<p>Different text</p>",
    "files": [1, 2, 3]
}
```

### Поля запроса

Имя | Тип | Обязательный | Описание
--- | --- | --- | ---
client_vacancy_request | number | Да | Идентификатор [заявки на вакансию для Кадрового Агенства](agency_vacancy_requests.md)
deadline | date | Нет | Дата дедлайна по вакансии
priority | number | Нет | Приоритет вакансии (может быть или 0 (обычный), или 1 (высокий))
body | string | Нет | Обязанности в формате HTML. Доступные теги: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**, **b**, **i**
requirements | string | Нет | Требования в формате HTML. Доступные теги: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**, **b**, **i**
conditions | string | Нет | Условия работы в формате HTML. Доступные теги: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**,**b**, **i**
files | array | Нет | Список файлов, прикрепленных к вакансии ([загрузка файлов](upload.md))

### Поля ответа

Дополнительно к полям запроса также будут добавлены поля:

```json
{
    "id": 1,
    "created": "2017-04-10T21:34:01+03:00",
    "position": "Моя должность"
}
```

Имя | Тип | Описание
--- | --- | ---
id | number | Идентификатор вакансии
created | datetime | Дата и время создания вакансии
position | string | Название вакансии (должности). Определяется по полю `position` из заявки на вакансию для Кадрового Агенства

<a name="vacancy-edit"></a>
## Редактирование вакансии

Если организация --- **не** Кадровое Агенство, то используется [другой набор полей](vacancies.md#edit).

`PUT /account/{account_id}/vacancies/{vacancy_id}`

### Поля запроса
Тело запроса аналогично телу в запросе на создание вакансии.

### Поля ответа
Ответ аналогичен ответу на запрос на создание вакансии.

<a name="vacancy-delete"></a>
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

<a name="vacancy-list"></a>
## Получение списка вакансий

Если организация --- **не** Кадровое Агенство, то возвращается [другой набор полей](vacancies.md#vacancies).

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
            "deadline": "2017-04-27",
            "applicants_to_hire": 1,
            "created": "2017-03-22T18:16:27+03:00",
            "priority": 0,
            "state": "OPEN"
        },
        {
            "id": 4530,
            "position": "Программист Python",
            "deadline": null,
            "applicants_to_hire": 1,
            "created": "2017-03-22T18:16:27+03:00",
            "priority": 0,
            "state": "CLOSED"
        }
    ],
    "total": 2,
    "page": 1,
    "count": 30
}
```

<a name="vacancies-result"></a>

Имя | Тип | Описание
 --- | --- | ---
 id |  number | Идентификатор вакансии
 position | string | Название вакансии (должности)
 deadline | date | Дата дедлайна по вакансии
 applicants_to_hire | number | Количество кандидатов к найму
 created | datetime | Дата и время создания вакансии
 priority | number | Приоритет вакансии (может быть или 0 (обычный), или 1 (высокий))
 state | string | Состояние вакансии


<a name="vacancy-view"></a>
## Получение вакансии

Если организация --- **не** Кадровое Агенство, то возвращается [другой набор полей](vacancies.md#vacancy).

`GET /account/{account_id}/vacancies/{vacancy_id}` вернёт вакансию с идентификатором `{vacancy_id}`

```json
{
    "id": 4531,
    "position": "Менеджер по продажам",
    "deadline": "2017-04-27",
    "applicants_to_hire": 1,
    "created": "2017-03-22T18:16:27+03:00",
    "priority": 0,
    "state": "OPEN",
    "body": "Обязанности",
    "requirements": "Требования",
    "conditions": "Условия работы",
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

Поля с результатом аналогичны данным из [списка вакансий](#vacancy-list) плюс поля:

Имя | Тип | Описание
 --- | --- | ---
 body | string | Обязанности в формате HTML
 requirements | string | Требования в формате HTML
 conditions | string | Условия работы в формате HTML
 files | array | Список файлов, прикрепленных к вакансии
