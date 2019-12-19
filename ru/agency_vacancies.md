# Работа с вакансиями Кадрового Агенства

* [Добавление вакансии](#vacancy-add)
* [Методы работы с вакансиями](vacancies.md)

<a name="vacancy-add"></a>
## Добавление вакансии для Кадрового Агенства

`POST /account/{account_id}/clients/{client_id}/vacancies`

В теле запроса необходимо передать JSON вида:

```json
{
    "client_vacancy_request": 11,
    "position": "Manufacturing Engineer",
    "deadline": "2020-09-03",
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
position | string | Да | Название вакансии (должности)
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
    "created": "2017-04-10T21:34:01+03:00"
}
```

Имя | Тип | Описание
--- | --- | ---
id | number | Идентификатор вакансии
created | datetime | Дата и время создания вакансии
