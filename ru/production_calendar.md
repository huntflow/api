# Сервис производственных календарей

* [Получение информации о календарях](#production_calendar)
* [Получение нерабочих дней за период](#days)
* [Массовое получение нерабочих дней за период](#days-bulk)
* [Расчет даты в будущем с учетом нерабочих дней](#deadline)
* [Массовый расчет даты в будущем с учетом нерабочих дней](#deadline-bulk)
* [Расчет даты в прошлом с учетом нерабочих дней](#start)
* [Массовый расчет даты в прошлом с учетом нерабочих дней](#start-bulk)

## Получение информации о календарях
<a name="production_calendar"></a>

`GET /production_calendar` вернет список производственных календарей для разных стран

```json
{
    "items": [
        {
            "id": 1,
            "name": "Russian Federation"
        }, 
        ... 
    ]
}
```

`GET /production_calendar/{calendar_id}` вернёт календарь с идентификатором `{calendar_id}`
```json
{
    "id": 1,
    "name": "Russian Federation"
}
```
### Поля ответа
Имя | Тип | Описание
--- | --- | ---
id | number | Уникальный идентификатор календаря
name | str | Название календаря

<a name="days"></a>
## Получение нерабочих дней за период

`GET /production_calendar/{calendar_id}/days/{deadline}` вернет информацию о нерабочих днях до даты `{deadline}` в формате `YYYY-MM-DD`, согласно календарю `{calendar_id}`.

Принимаемые параметры:

* `start` — дата в формате `YYYY-MM-DD`.
Дата с которой нужно начать отсчет нерабочих дней. Опционально. По-умолчанию — сегодня.

* `verbose` — логическое поле.
Добавляет в ответ поле `items` — список дат, выходных и праздничных дней за период в  формате `YYYY-MM-DD`.

### Поля ответа

Дополнительно к полям запроса также будут добавлены поля:

```json
{
    "total_days": 36,
    "not_working_days": 28,
    "items": ["2019-01-01", ... ]
}
```
Имя | Тип | Описание
--- | --- | ---
total_days | number | Общее количество дней за период
not_working_days | number | Количество нерабочих дней за период
items | list | список дат, выходных и праздничных дней за период

<a name="days-bulk"></a>
## Массовое получение нерабочих дней за период

`POST /production_calendar/{calendar_id}/days` массовая версия предыдущего запроса.

Принимаемые параметры:
* `verbose` — логическое поле.
Добавляет в ответ поле `items` — список дат, выходных и праздничных дней за период в  формате `YYYY-MM-DD`.

В теле запроса необходимо передать JSON вида:
```json
[
    {"deadline": "2019-04-20"},
    {"deadline": "2019-05-20" "start": "2018-05-20"}
]
```

### Поля запроса

Имя | Тип | Обязательный | Описание
 --- | --- | --- | ---
 deadline | date | Да | Конец периода
 start | date | Нет | Начало периода, по-умолчанию — сегодня

```json
[
    {
        "total_days": 36,
        "not_working_days": 28,
        "items": ["2019-01-01", ... ]
    },
    ...
]
```
Для каждого элемента в списке набор полей совпадает с одиночным заросом.

<a name="deadline"></a> 
## Расчет даты в будущем с учетом нерабочих дней

`GET /production_calendar/{calendar_id}/deadline/{days}` - вернет дату в будущем, через `{days}` рабочих дней, согласно календарю `{calendar_id}`.

Принимаемые параметры:

* `start` — дата в формате `YYYY-MM-DD`.
Дата, с которой нужно начать отсчет. Опционально. По-умолчанию — сегодня.


###  Пример ответа
```
"2019-01-25"
```

<a name="deadline-bulk"></a>
## Массовый расчет даты в будущем с учетом нерабочих дней

`POST /production_calendar/{calendar_id}/deadline` массовая версия предыдущего запроса.

В теле запроса необходимо передать JSON вида:
```json
[
    {"days": 10},
    {"days": 20},
    {"days": 30, "start": "2007-09-01"}
]
```
### Поля запроса

Имя | Тип | Обязательный | Описание
 --- | --- | --- | ---
 days | number | Да | количество рабочих дней
 start | date | Нет | Дата, с которой нужно начать отсчет. Опционально. По-умолчанию — сегодня.

 ###  Пример ответа
```
[
    "2019-01-25",
    "2019-03-01"
]
```

<a name="start"></a> 
## Расчет даты в прошлом с учетом нерабочих дней

`GET /production_calendar/{calendar_id}/start/{days}` - вернет дату в прошлом, через `{days}` рабочих дней, согласно календарю `{calendar_id}`.

Принимаемые параметры:

* `deadline` — дата в формате `YYYY-MM-DD`.
Дата, с которой нужно начать отсчет назад. Опционально. По-умолчанию — сегодня.


###  Пример ответа
```
"2018-09-25"
```

<a name="start-bulk"></a> 
## Массовый расчет даты в прошлом с учетом нерабочих дней

`POST /production_calendar/{calendar_id}/start` массовая версия предыдущего запроса.

В теле запроса необходимо передать JSON вида:
```json
[
    {"days": 10},
    {"days": 20},
    {"days": 30, "deadline": "2017-09-01"}
]
```
### Поля запроса

Имя | Тип | Обязательный | Описание
 --- | --- | --- | ---
 days | number | Да | количество рабочих дней
 deadline | date | Нет | Дата, на которой нужно окончить отсчет. Опционально. По-умолчанию — сегодня.

 ###  Пример ответа
```
[
    "2018-07-25",
    "2018-03-01"
]
```


