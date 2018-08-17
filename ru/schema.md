# Пользовательская схема данных

Пример схемы:

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
*Поле верхнего уровня* | Название поля
id | Идентификатор поля
type | [Тип поля](#types-list)
title | Заголовок поля
required | Флаг обязательности поля
order | Порядок расположения поля на форме
values | Список возможных значений (для полей с  `type = select`)
fields | Дочерние поля

<a name="types-list"></a>
## Типы полей
Тип поля |  Описание
-------- | ---------
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
