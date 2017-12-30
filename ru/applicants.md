# Работа с кандидатами

* [Добавление кандидата в базу](#add)
* [Добавление кандидата на вакансию](#vacancy_applicant)
* [Список кандидатов](#applicants)

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
created | string | Дата+время создания кандидата
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
changed | string | Дата изменения статуса

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

Путь |  Описание
---- | --------
last_name | Фамилия
first_name | Имя
middle_name | Отчество
phone | Телефон
email | Электронная почта
position | Кем работает
company | Где работает
money | Зарплатные ожидания
birthday | Дата рождения
photo | Фото кандидата (идентификатор загруженного файла), информация о загрузке файлов доступна [здесь](upload.md)
photo_url | Ссылка на фото кандидата
external | Резюме кандидатов
external[].auth_type | Формат резюме
external[].account_source | Источник резюме
external[].updated | Дата и время обновления резюме
links | Вакансии, по которым проходит кандидат
links[].status | Этап подбора кандидата
links[].vacancy | Вакансия
links[].updated | Дата обновления по кандидату на вакансии
links[].changed | Дата последнего изменения этапа подбора
