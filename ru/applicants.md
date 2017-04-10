# Работа с кандидатами

* [Добавление кандидата в базу](#add)
* [Добавление кандидата на вакансию](#vacancy_applicant)


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

Путь | JSON тип | Обязательный | Описание
---- | -------- | ------------ | --------
last_name | string | Да | Фамилия
first_name | string | Да | Имя
middle_name | string | Нет | Отчество
phone | string | Нет | Телефон
email | string | Нет | Электронная почта
position | string | Нет | Кем работает
company | string | Нет | Где работает
money | string | Нет | Зарплатные ожидания
birthday_day | numeric | Нет | День рождения
birthday_month | numeric | Нет | Месяц рождения
birthday_year | numeric | Нет | Год рождения
photo | numeric | Нет | Фото кандидата (идентификатор загруженного файла), информация о загрузке файлов доступна [здесь](upload.md)
externals[].data.body | string | Нет | Текст резюме
externals[].files.id | numeric | Нет | Идентификатор файла загруженного резюме
externals[].account_source | numeric | Нет | Идентификатор источника резюме ([справочник](dicts.md#applicant_sources))

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

Имя | JSON тип | Описание
--- | --- | ---
id | numeric | Идентификатор кандидата
created | строка | Дата+время создания кандидата
doubles[].double | numeric | Идентификатор кандидата, определенного как дубликат 


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

Путь | JSON тип | Обязательный | Описание
---- | -------- | ------------ | --------
vacancy | numeric | Да | Идентификатор [вакансии](vacancies.md)
status | numeric | Да | [Этап подбора](dicts.md#vacancy_statuses) 
comment | string | Нет | Произвольный комментарий
files | array | Нет | Массив прикрепленных файлов к записи в истории (не больше 10)
files[].id | numeric | Да | Идентификатор файла
rejection_reason | numeric | Нет | Причина отказа (если статус `Отказ`)

### Поля ответа

Дополнительно к полям запроса также будут добавлены поля:

```json
{
    "id": 279284,
    "changed": "2017-04-10T21:34:01+03:00"
}
```

Имя | JSON тип | Описание
--- | --- | ---
id | numeric | Идентификатор привязки
changed | строка | Дата изменения статуса
