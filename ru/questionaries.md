  # Работа с анкетами кандидатов

* [Получение схемы анкеты кандидата](#scheme)
* [Получение анкеты кандидата](#view)
* [Создание/редактирование анкеты кандидата резюме](#edit)

<a name="scheme"></a>
## Получение схемы анкеты кандидата

`GET /account/{account_id}/applicants/questionary`

Пример ответа:

```json
{
    "resource": {
        "type": "dictionary",
        "title": "Ресурс-менеджер",
        "required": true,
        "account": 38,
        "order": 1,
        "id": 49,
        "value": null,
        "dictionary": "resource-manager"
    },
    "position": {
        "type": "dictionary",
        "title": "Позиция",
        "required": true,
        "account": 38,
        "order": 2,
        "id": 50,
        "value": null,
        "delimiter": true,
        "dictionary": "position"
    },
    "expected_trial": {
        "type": "income",
        "title": null,
        "required": false,
        "account": 38,
        "order": 3,
        "id": 51,
        "value": null,
        "fields": {
            "income": {
                "type": "string",
                "title": "Ожидаемый доход на ИС, руб",
                "required": false,
                "account": 38,
                "order": 4,
                "id": 52,
                "value": null
            },
            "type": {
                "type": "select",
                "title": " ",
                "required": false,
                "account": 38,
                "order": 5,
                "id": 53,
                "value": null,
                "values": [
                    "Gross",
                    "Net"
                ]
            }
        }
    },
    "expected_main": {
        "type": "income",
        "title": null,
        "required": false,
        "account": 38,
        "order": 6,
        "id": 54,
        "value": null,
        "delimiter": true,
        "fields": {
            "income": {
                "type": "string",
                "title": "Ожидаемый доход на ОС, руб",
                "required": false,
                "account": 38,
                "order": 7,
                "id": 55,
                "value": null
            },
            "type": {
                "type": "select",
                "title": " ",
                "required": false,
                "account": 38,
                "order": 8,
                "id": 56,
                "value": null,
                "values": [
                    "Gross",
                    "Net"
                ]
            }
        }
    },
    "current_salary": {
        "type": "income",
        "title": null,
        "required": false,
        "account": 38,
        "order": 9,
        "id": 57,
        "value": null,
        "fields": {
            "income": {
                "type": "string",
                "title": "Текущий оклад, руб",
                "required": false,
                "account": 38,
                "order": 10,
                "id": 58,
                "value": null
            },
            "type": {
                "type": "select",
                "title": " ",
                "required": false,
                "account": 38,
                "order": 11,
                "id": 59,
                "value": null,
                "values": [
                    "Gross",
                    "Net"
                ]
            }
        }
    },
    "current_total": {
        "type": "income",
        "title": null,
        "required": false,
        "account": 38,
        "order": 12,
        "id": 60,
        "value": null,
        "fields": {
            "income": {
                "type": "string",
                "title": "Текущий совокупный доход, руб",
                "required": false,
                "account": 38,
                "order": 13,
                "id": 61,
                "value": null
            },
            "type": {
                "type": "select",
                "title": " ",
                "required": false,
                "account": 38,
                "order": 14,
                "id": 62,
                "value": null,
                "values": [
                    "Gross",
                    "Net"
                ]
            }
        }
    }
}
```

## Описание полей
Поле |  Описание
---- | ---------
*поле верхнего уровня* | название поля анкеты
id | идентификатор поля
type | тип поля анкеты
title | заголовок поля анкеты
required | флаг обязательности поля анкеты
order | порядок расположения поля анкеты на форме
values | сипсок возможных значений (для полей с типом select)
fields | дочерние поля

## Типы полей
Тип поля |  Описание
-------- | ---------
*поле верхнего уровня* | название поля анкеты
string | Строковое поле
integer | Числовое поле
text | Текстовое поле
date | Дата
select | Элемент списка
complex | Составное поле
contract | Тип договора
reason | Причина открытия вакансии
stoplist | Стоп-лист
compensation | Зарплатная вилка
dictionary | Справочник
income | Доход с типом
position_status | Статус позиции
division | Подразделение
region | Регион
url | Ссылка
hidden | Скрытое поле
html | HTML-редактор

<a name="view"></a>

## Получение анкеты кандидата

`GET /account/{account_id}/applicants/{applicant_id}/questionary`

Пример ответа:

```json
{
    "resource": 667,
    "position": 33255,
    "expected_trial": {
        "income": "9",
        "type": "Gross"
    },
    "expected_main": {
        "income": "9",
        "type": "Gross"
    },
    "current_salary": {
        "income": "9",
        "type": "Gross"
    },
    "current_total": {
        "income": "9",
        "type": "Gross"
    }
}
```

Чтобы нормализовать значения полей с типом справочник, добавьте к запросу параметр `normalize=true`:

`GET /account/{account_id}/applicants/{applicant_id}/questionary?normalize=true`

Пример ответа:

```json
{
    "resource": {
        "id": 667,
        "name": "Тестирование 2017",
        "foreign": null,
        "meta": null
    },
    "position": {
        "id": 33255,
        "name": "Стажер",
        "foreign": null,
        "meta": null
    }
}
```

Поле |  Описание
---- | --------
id | идентификатор значения
name | значение (для полей с типом dictionary, region, division)
foreign | внешний идентификатор значения (для полей с типом dictionary, region, division)
meta | дополнительная информация о значении (для полей с типом dictionary, region, division)

<a name="edit"></a>

## Создание/редактирование анкеты кандидата резюме

`POST /account/{account_id}/applicants/{applicant_id}/questionary`

В теле запроса необходимо передать JSON вида:

```json
{
    "current_salary": {"income": "9", "type": "Gross"},
    "current_total": {"income": "9", "type": "Gross"},
    "expected_main": {"income": "9", "type": "Gross"},
    "expected_trial": {"income": "9", "type": "Gross"},
    "position": 33255,
    "resource": 667
}
```

Пример ответа:

```
{
    "resource": 667,
    "position": 33255,
    "expected_trial": {
        "income": "9",
        "type": "Gross"
    },
    "expected_main": {
        "income": "9",
        "type": "Gross"
    },
    "current_salary": {
        "income": "9",
        "type": "Gross"
    },
    "current_total": {
        "income": "9",
        "type": "Gross"
    }
}
```
