# Работа с вакансиями Кадрового Агенства

* [Добавление вакансии](#vacancy-add)

<a name="vacancy-add"></a>
## Добавление вакансии для Кадрового Агенства

`POST /account/{account_id}/clients/{client_id}/vacancies`

В теле запроса необходимо передать JSON вида:

```json
{
    "client_vacancy_request": 11,
    "position": "Manufacturing Engineer",
    "company": "Tesla",
    "money": "$100k",
    "deadline": "2017-09-03",
    "applicants_to_hire": 2,
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
client_vacancy_request | number | Да | Идентификатор [заявки на вакансию для Кадрового Агенства](agency_vacancy_requests.md)
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
created | datetime | Дата и время создания вакансии
