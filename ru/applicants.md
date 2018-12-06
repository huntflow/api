# Работа с кандидатами

* [Добавление кандидата в базу](#add)
* [Добавление кандидата на вакансию](#vacancy_applicant)
* [Список кандидатов](#applicants)
* [Логи кандидата](#applicant_logs)

<a name="add"></a>
## Добавление кандидата в базу

`POST /account/{account_id}/applicants`

В теле запроса необходимо передать JSON вида:

```json
{
    "last_name": "Глибин",
    "first_name": "Виталий",
    "middle_name": "Николаевич",
    "phone": "89260731778",
    "email": "glibin.v@gmail.com",
    "position": "Фронтендер",
    "company": "ХХ",
    "money": "100000 руб",
    "birthday_day": 20,
    "birthday_month": 7,
    "birthday_year": 1984,
    "photo": 12341,
    "externals": [
        {
            "data": {
                "body": "Текст резюме\nТакой текст"
            },
            "auth_type": "NATIVE",
            "files": [
                {
                    "id": 12430
                }
            ],
            "account_source": 208
        }
    ]
}
```

### Поля запроса

* `[]` обозначает, что значение данного ключа является массивом
* `a.b` обозначает объект `a` с ключом `b`

Путь | Тип | Обязательный | Описание
---- | -------- | ------------ | --------
last_name | string | Да | Фамилия
first_name | string | Да | Имя
middle_name | string | Нет | Отчество
phone | string | Нет | Телефон
email | string | Нет | Электронная почта
position | string | Нет | Кем работает
company | string | Нет | Где работает
money | string | Нет | Зарплатные ожидания
birthday_day | number | Нет | День рождения
birthday_month | number | Нет | Месяц рождения
birthday_year | number | Нет | Год рождения
photo | number | Нет | Фото кандидата (идентификатор загруженного файла), информация о загрузке файлов доступна [здесь](upload.md)
externals[].data.body | string | Нет | Текст резюме
externals[].files.id | number | Нет | Идентификатор файла загруженного резюме
externals[].account_source | number | Нет | Идентификатор источника резюме ([справочник](dicts.md#applicant_sources))

### Поля ответа

Дополнительно к полям запроса также будут добавлены поля:

```json
{
    "id": 123321,
    "created": "2017-04-10T21:34:01+03:00",
    "doubles": [
        {
            "double": 123320
        }
    ]
}
```

Имя | Тип | Описание
--- | --- | ---
id | number | Идентификатор кандидата
created | datetime | Дата и время создания кандидата
doubles[].double | number | Идентификатор кандидата, определенного как дубликат


<a name="vacancy_applicant"></a>
## Добавление кандидата на вакансию

`POST /account/{account_id}/applicants/{applicant_id}/vacancy`

В теле запроса необходимо передать JSON вида:

```json
{
    "vacancy": 988,
    "status": 1230,
    "comment": "Привет",
    "files": [
        {
            "id": 1382810
        }
    ],
    "rejection_reason": null
}
```

### Поля запроса

Путь | Тип | Обязательный | Описание
---- | -------- | ------------ | --------
vacancy | number | Да | Идентификатор [вакансии](vacancies.md)
status | number | Да | [Этап подбора](dicts.md#vacancy_statuses)
comment | string | Нет | Произвольный комментарий
files | array | Нет | Массив прикрепленных файлов к записи в истории (не больше 10)
files[].id | number | Да | Идентификатор файла
rejection_reason | number | Нет | Причина отказа (если статус `Отказ`)

### Поля ответа

Дополнительно к полям запроса также будут добавлены поля:

```json
{
    "id": 279284,
    "changed": "2017-04-10T21:34:01+03:00"
}
```

Имя | Тип | Описание
--- | --- | ---
id | numeric | Идентификатор привязки
changed | datetime | Дата и время изменения статуса

<a name="applicants"></a>
## Получение списка кандидатов

`GET /account/{account_id}/applicants` вернёт список кандидатов компании.

Принимаемые параметры:

* `status` — этап подбора. Если передается, то вернутся только кандидаты, которые сейчас находятся на вакансиях на указанном этапе

* `vacancy` — вакансия. Используется дополнительно к `status` для фильтрации кандидатов по вакансии на указанном этапе

* `count`, `page` — [параметры постраничного вывода](general.md#pagination).

Пример ответа:

```json
{
  "count": 2,
  "items": [
    {
      "last_name": "Иванов",
      "links": [
        {
          "status": 2,
          "updated": "2017-12-19T21:48:12+03:00",
          "changed": "2017-12-19T21:48:12+03:00",
          "vacancy": 1,
          "id": 172
        }
      ],
      "money": "",
      "id": 290,
      "first_name": "Александр",
      "middle_name": null,
      "photo": 385,
      "photo_url": "https://store.huntflow.ru/uploads/5/7/0/5700c66c07a23f2e80a0acc0bc3ef6c8.jpeg",
      "email": "hello@mail.ru",
      "tags": [],
      "company": "Save Factory",
      "phone": "+79511234567",
      "birthday": "1979-06-20",
      "external": [
        {
          "auth_type": "HH",
          "account_source": 2,
          "updated": "2017-12-19T21:48:12+03:00",
          "applicant": 290,
          "id": 290
        }
      ],
      "doubles": [],
      "created": "2017-12-19T21:48:12+03:00",
      "position": "Artist"
    },
    {
      "last_name": "Петров",
      "links": [
        {
          "status": 2,
          "updated": "2017-12-19T21:35:09+03:00",
          "changed": "2017-12-19T21:35:09+03:00",
          "vacancy": 1,
          "id": 171
        }
      ],
      "money": "30000 RUR",
      "id": 289,
      "first_name": "Виталий",
      "middle_name": "Иванович",
      "photo": 384,
      "photo_url": "https://store.huntflow.ru/uploads/5/7/0/5700c66c07a23f2e80a0acc0bc3ef6c8.jpeg",
      "email": "test@mail.ru",
      "tags": [
          {
              "tag": 1,
              "id": 1
          }
      ],
      "company": "Краснодеревщик,ЗАО",
      "phone": "+79639801000",
      "birthday": "1975-01-26",
      "external": [
        {
          "auth_type": "HH",
          "account_source": 2,
          "updated": "2017-12-19T21:35:09+03:00",
          "applicant": 289,
          "id": 289
        }
      ],
      "doubles": [],
      "created": "2017-12-19T21:35:09+03:00",
      "position": "Фотограф"
    }
  ],
  "total": 120,
  "page": 1,
  "total_items": 240
}

```

Путь | Тип | Описание
---- | --- | --------
last_name | string | Фамилия
first_name | string | Имя
middle_name | string | Отчество
phone | string | Телефон
email | string | Электронная почта
position | string | Кем работает
company | string | Где работает
money | string | Зарплатные ожидания
birthday | date | Дата рождения
photo | number | Фото кандидата (идентификатор загруженного файла), информация о загрузке файлов доступна [здесь](upload.md)
photo_url | string | Ссылка на фото кандидата
external | array | Резюме кандидатов
external[].id | number | Идентификатор резюме <a name="external_id"></a>
external[].auth_type | string | Тип резюме
external[].account_source | number | Источник резюме
external[].updated | datetime | Дата и время обновления резюме
links | array | Вакансии, по которым проходит кандидат
links[].status | number | Этап подбора кандидата
links[].vacancy | number | Вакансия
links[].updated | datetime | Дата и время обновления по кандидату на вакансии
links[].changed | datetime | Дата и время последнего изменения этапа подбора
tags[].id | number | Идентификатор метки кандидата
tags[].tag | number | Идентификатор метки

<a name="applicant_log"></a>

## Получение логов кандидата

`GET /account/{account_id}/applicants/{applicant_id}/log` вернёт список логов кандидата.


Принимаемые параметры:

* `count`, `page` — [параметры постраничного вывода](general.md#pagination).

Пример ответа:

```
{
    "count": 30,
    "items": [
        {
            "comment": "Комментарий",
            "files": [
                {
                    "url": "http://huntflow.ru/uploads/named/e/x/a/example.docx",
                    "id": 2844,
                    "content_type": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
                    "name": "example.docx"
                }
            ],
            "vacancy": 3,
            "account_info": {
                "name": "Пример",
                "id": 1
            },
            "rejection_reason": null,
            "calendar_event": {
                "event_type": "interview",
                "creator": null,
                "vacancy": 3,
                "meta": {
                    "hide_salary": true,
                    "external": "64231",
                    "NATIVE": {
                        "url": "http://huntflow.ru/my/example#vacancy/3/filter/workon/id/34276",
                        "name": "Ссылка на кандидата: "
                    }
                },
                "timezone": "Europe/Saratov",
                "calendar": 1,
                "id": 18,
                "state_reason": null,
                "end": "2018-11-28 17:00:00",
                "interview_type": 49,
                "sms": null,
                "recurrence": null,
                "start": "2018-11-28 16:00:00",
                "state": "QUEUED",
                "etag": null,
                "location": "Саратов",
                "status": "confirmed",
                "description": "Описание",
                "sms_reminder_template": null,
                "recurrence_link": null,
                "attendees": [
                    {
                        "displayName": "Пример",
                        "email": "example@example.ru",
                        "responseStatus": "needsAction"
                    }
                ],
                "removed": null,
                "data": null,
                "name": "Интервью в центральном офисе: Пример – Тест",
                "created": "2018-11-28 15:46:13",
                "applicant": 34276,
                "reminders": [
                    {
                        "minutes": 15,
                        "method": "popup",
                        "value": 15,
                        "multiplier": "minutes"
                    }
                ],
                "all_day": false,
                "foreign": null,
                "transparency": "busy",
                "action": "ADD"
            },
            "id": 180368,
            "employment_date": null,
            "status": 2,
            "account": 1,
            "created": "2018-11-28T15:46:13+03:00",
            "type": "COMMENT"
        }
    ],
    "total": 1,
    "page": 1
}
```

Имя | Тип | Описание |
--- | --- | -------- |
account | number | Идентификатор пользователя от имени которого создан лог |
account_info | string | Информация о пользователе от имени которого создан лог |
account_info.id | number | Идентификатор пользователя |
account_info.name | string | Имя пользователя |
calendar_event | object | Событие календаря прикреплённое к логу |
calendar_event.all_day | bool | Признак того что событие запланировано на весь день |
calendar_event.attendees[] | list | Список приглашенных на событие |
calendar_event.attendees[].displayName | string | Отображаемое имя приглашенного |
calendar_event.attendees[].email | string | Электронная почта приглашенного |
calendar_event.attendees[].responseStatus | string | Ответ приглашенного на приглашение |
calendar_event.created | datetime | Дата создания события |
calendar_event.creator | object | Информация о создателе события |
calendar_event.creator.self | bool | ??? |
calendar_event.creator.displayName | string | Отображаемое имя создателя события |
calendar_event.creator.email | string | Электронная почта создателя события |
calendar_event.description | string | Описание события |
calendar_event.end | datetime | Дата и время окончания события |
calendar_event.etag | string | ETag события |
calendar_event.event_type | string | Тип события календаря |
calendar_event.foreign | string | Внешний идентификатор события |
calendar_event.location | string | Место проведения события |
calendar_event.name | string | Название события |
calendar_event.reminders[] | list | Список напоминаний о событии |
calendar_event.reminders[].method | string | Метод напоминания |
calendar_event.reminders[].minutes | number | За какое количество минут напомнить о событии |
calendar_event.start | datetime | Дата и время начала события |
calendar_event.status | string | Статус события |
calendar_event.timezone | string | Часовой пояс в котором заданы дата начала и конца события |
calendar_event.transparency |string | Занятость во время события |
comment | string |Текст комментария |
created | datetime | Дата создания лога |
employment_date | datetime | Дата принятия кандидата на работу |
files | list | Файлы прикрепленные к логу |
files[].content_type | string | Content Type файла |
files[].id | number | Идентификатор файла |
files[].name | string | Исходное имя файла |
files[].url | string | Ссылка для скачивания файла |
id | number | Идентификатор лога |
rejection_reason | number | Идентификатор причины отказа |
status | number | Идентификатор статуса |
type | string | Тип лога |
vacancy | number | Идентификатор вакансии к которой относится лог (если null, значит лог относится к кандидату в целом) |
