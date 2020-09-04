# Работа со справочниками

* [Получение справочника](#get)
* [Создание справочника](#create)
* [Редактирование справочника](#edit)

<a name="get"></a>
## Получение справочника

`GET /account/{account_id}/dictionary/{dictionary_code}`

Пример ответа:

```json
{
    "id": 1,
    "name": "Grade Values",
    "code": "grade",
    "created": "2019-06-26 15:19:10",
    "foreign": "grade_v_12",
    "fields": [
        {
            "deep": 0,
            "foreign": "0001",
            "id": 1,
            "meta": null,
            "name": "Backend",
            "order": 0,
            "parent": null,
            "active": true
        },
        {
            "deep": 1,
            "foreign": "0002",
            "id": 2,
            "meta": {"kpi_coefficient": 5, "level": 3},
            "name": "Senior",
            "order": 1,
            "parent": 1,
            "active": true
        },
        {
            "deep": 1,
            "foreign": "0003",
            "id": 3,
            "meta": {"kpi_coefficient": 3, "level": 3},
            "name": "Middle",
            "order": 2,
            "parent": 1,
            "active": true
        },
        {
            "deep": 1,
            "foreign": "0004",
            "id": 4,
            "meta": {"kpi_coefficient": 1, "level": 3},
            "name": "Junior",
            "order": 3,
            "parent": 1,
            "active": true
        }
    ]
}
```

## Поля ответа
Имя |  Тип  |  Описание
---- | ----- | ---------
id | number | Идентификатор справочника
code | string | Код справочника
name | string | Наименование справочника
created | datetime | Дата-время создания справочника
foreign | string | Внений идентификатор справочника
fields | array | Список значений справочника
fields[].deep | number | Глубина вложенности значения
fields[].foreign | string | Внешний уникальный идентификатор значения справочника
fields[].id | number | Внутренний идентификатор значения справочника
fields[].meta | json object | Мета-информация о значении справочника. Может быть использована в отчетах, статистике и т.д.
fields[].name | string | Наименование значения справочника
fields[].order | number | Порядковый номер значения справочника. Учитывает иерархию справочника
fields[].parent | number | Внутренний идентификатор родительского значения. Через это поле достигается иерархичность значений справочника
fields[].active | boolean | Флаг активности значения. Неактивные значения не отображаются в селекторах


<a name="create"></a>

## Создание справочника

Доступно только для управляющего рекрутера

`POST /account/{account_id}/dictionary`
В теле запроса необходимо передать JSON вида:

```json
{
    "code": "grade",
    "name": "Grade Values",
    "foreign": "grade_v_12",
    "items": [
        {
            "foreign": "0001", 
            "name": "Backend",
            "items": [
                {"name": "Senior", "foreign": "0002", "meta": {"kpi_coefficient": 5, "level": 3}},
                {"name": "Middle", "foreign": "0003", "meta": {"kpi_coefficient": 3, "level": 3}},
                {"name": "Junior", "foreign": "0004", "meta": {"kpi_coefficient": 1, "level": 3}}
            ]
        }
    ]
}
```

## Поля запроса
Имя |  Тип  |  Описание | Обязательность
--- | ----- | --------- | --------------
code | string | Код справочника | Да
name | string | Наименование справочника | Да
foreign | string | Внешний идентификатор справочника | Нет
items | array | Список значений справочника | Да
items[].foreign | string | Внешний идентификатор значения справочника | Да
items[].name | string | Наименование значения справочника | Да
items[].meta | objects | Мета-информация значения справочника | Нет
items[].active | boolean | Флаг активности значения справочника | Нет
items[].items | array | Вложенные значения справчника (максимальная вложенность –– 5) | Нет

Создание справочника выполняется отложенно, поэтому в ответе вы получите информацию о созданной задаче:

```json
{
    "status": "ok",
    "payload": {
        "task_id": "a798029c-8353-4f5c-ae69-103d7b631172"
    },
    "meta": {
        "data": {
            "name": "control",
            "items": [
                {
                    "foreign": "0001", 
                    "name": "Backend",
                    "items": [
                        {"name": "Senior", "foreign": "0002", "meta": {"kpi_coefficient": 5, "level": 3}},
                        {"name": "Middle", "foreign": "0003", "meta": {"kpi_coefficient": 3, "level": 3}},
                        {"name": "Junior", "foreign": "0004", "meta": {"kpi_coefficient": 1, "level": 3}}
                    ]
                }
            ],
            "code": "control"
        },
        "account_id": 291
    }
}
```
где `payload.task_id` –– уникальный идентификатор задачи. [Как отслеживать статус задачи](delayed_tasks.md).


<a name="edit"></a>

## Редактирование справочника

Доступно только для управляющего рекрутера

`PUT /account/{account_id}/dictionary/{dictionary_code}`

Тела запроса и ответа аналогичны [методу создания](#create) справочника.


