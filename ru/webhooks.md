# Вебхуки

## Что такое вебхуки
Вебхуки — это удобный способ оповещать ваши внутренние системы о действиях с кандидатами в Хантфлоу.

Хантфлоу может отправлять данные кандидата в ваш 1С или интранет при переводе кандидата на этап «Выход на работу». Или вы можете разработать чат-бота, который будет отправлять заказчику ссылку на кандидата в Слак или Телеграм.

## Технически
С технической точки зрения вебхук – это POST запрос, отправляемый нашей системой на ваш удаленный сервер. Вся информация о субъекте, объекте и характере изменения данных содержится в теле запроса и в его заголовках.

## Как создать вебхук
1. Перейдите в настройки организации
2. Убедитесь, что у вас подключен API

![API](img/screenshot_01.png)

3. Добавьте вебхук, указав URL, секретный ключ (опционально) и выбрав события,
на которые вы хотите подписаться. Обратите внимание, что при добавлении вебхука
система проверяет его на доступность. Для прохождения этой проверки, удаленный
сервер должен ответить кодом `200` или `204` на запрос с типом PING.

![API](img/screenshot_02.png)

4. В случае успешной проверки, вебхук будет добавлен (всего можно добавлять до 5 вебхуков).
Вы можете редактировать или удалять вебхуки после создания, нажав на интересующую
вас запись в списке.

![API](img/screenshot_03.png)


<a name="event-types"></a>

## Типы событий
 - APPLICANT — действия над кандидатом
 - VACANCY — действия по вакансиям
 - PING — проверка на доступность вебхука


## Заголовки вебхука
 ```json
 {
     "X-Huntflow-Delivery": "fcb5de9e-75a9-42c7-a7d2-3f070f2c5e00",
     "X-Huntflow-Event": "APPLICANT",
     "X-Huntflow-Signature": "298d0211223cf9b504c04674d3b7dbf9627803818098fdf3cec6f50cabb15b76"
 }
 ```

 | Заголовок | Описание |
 | --------- | -------- |
 | X-Huntflow-Delivery | Уникальный идентификатор вебхука |
 | X-Huntflow-Event | [Тип события](#event-types) |
 | X-Huntflow-Signature | hex digest sha256 hmac тела вебхука, сгенерированный с помощью секретного ключа (отсутствует, если в вебхуке не указан секретный ключ) |


## Тело вебхука

### APPLICANT

```json
{
    "event": {
        "id": 1,
        "type": "COMMENT",
        "applicant": {
                "id": 1,
                "first_name": "Иванов",
                "last_name": "Иван",
                "middle_name": "Инванович",
                "birthday": "1970-01-01",
                "photo": {
                    "id": 1307833,
                    "content_type": "image/png",
                    "name": "477233672.png",
                    "url": "https://store.huntflow.ru/uploads/named/4/8/5/485cc4914d214065784507b1275fc143.png/477233672.png?s=7hq2usgld1uqC9k5-AcwkA&e=1504005423"
                }
            },
        "vacancy": {
            "id": 1,
            "position": "Manufacturing Engineer",
            "company": "Tesla",
            "money": "$100k",
            "state": "OPEN",
            "hidden": false,
            "priority": 1,
            "deadline": null,
            "account_division": {
                "id": 1,
                "name": "name"
            },
            "created": "2017-06-22T18:16:27+03:00"
        },
        "status": {
            "id": 3,
            "name": "Declined"
        },
        "rejection_reason": {
            "id": 4,
            "name": "Does not meet the qualifications"
        },
        "comment": null,
        "calendar_event": {
            "status": "confirmed",
            "attendees": [
                {
                    "displayName": "Zach Braff",
                    "responseStatus": "needsAction",
                    "email": "za@za.za"
                }
            ],
            "end": "2018-06-29T12:00:00+03:00",
            "event_type": "interview",
            "created": "2018-06-29T10:31:57+03:00",
            "description": "Ссылка на кандидата: http://127.0.0.1:8400/my/zazzaza#vacancy/48594/filter/workon/id/8224\n\n***\n\n",
            "creator": {"self": true, "displayName": "Zach Braff", "email": "za@za.za"},
            "reminders": [{"minutes": 15, "method": "popup"}],
            "all_day": false,
            "foreign": "20180629T103157_HF_8224_48594_true_165",
            "recurrence": [],
            "start": "2018-06-29T11:00:00+03:00",
            "etag": "1530258908289",
            "location": null,
            "transparency": "busy",
            "timezone": "Europe/Moscow",
            "name": "Интервью: Кораллов Михаил – Менеджер по продажам"
        },
        "created": "2017-08-22T18:16:27+03:00"
    },
    "author": {
        "id": 4,
        "name": "Валентин Сергеев",
        "email": "sergeev@example.com"
    },
    "account": {
        "id": 6,
        "name": "San Carlos Recruitment"
    }
}
```

- a.b обозначает объект a с ключом b


|  Имя | Тип | Описание |
| --- | --- | -------- |
| event.id | number | Идентификатор действия |
| event.type | string | [Тип действия](#action-types) |
| event.applicant.id | number | Идентификатор кандидата |
| event.applicant.first_name | string | Имя кандидата |
| event.applicant.last_name | string | Фамилия кандидата |
| event.applicant.middle_name | string | Отчество кандидата |
| event.applicant.birthday | date | Дата рождения кандидата |
| event.applicant.photo.url | string | Сссылка на фотографию кандидата |
| event.vacancy.id | number | Идентификатор вакансии |
| event.vacancy.position | string | Название вакансии (должности) |
| event.vacancy.company | string | Отдел, подразделение (`null`, если подключены подразделения) |
| event.vacancy.money | string | Зарплата |
| event.vacancy.state | string | Статус вакансии |
| event.vacancy.hidden | bool | Скрыта ли вакансия от коллег |
| event.vacancy.priority | number | Приоритет вакансии (может быть или 0 (обычный), или 1 (высокий)) |
| event.vacancy.deadline | date | Дата дедлайна по вакансии |
| event.vacancy.account_division.id | number | Идентификатор подразделения (если подразделения подключены) |
| event.vacancy.account_division.name | string | Название подразделения (если подразделения подключены) |
| event.vacancy.created | datetime | Дата и время создания вакансии |
| event.status.id | number | Идентификатор этапа подбора |
| event.status.name | string | Название этапа подбора |
| event.rejection_reason.id | number | Идентификатор причины отказа |
| event.rejection_reason.name | string | Название причины отказа |
| event.comment | string | Текст комментария |
| event.calendar_event.name | string | Название события |
| event.calendar_event.description | string | Описание события |
| event.calendar_event.status | string | [Статус события](#event-status) |
| event.calendar_event.event_type | string | [Тип события](#event-type) |
| event.calendar_event.start | datetime | Дата и время начала события |
| event.calendar_event.end | datetime | Дата и время окончания события |
| event.calendar_event.timezone | string | Название часового пояса события |
| event.calendar_event.attendees | list | Участники события |
| event.calendar_event.attendees.displayName | string | Имя участника события |
| event.calendar_event.attendees.email | string | Email участника события |
| event.calendar_event.attendees.responseStatus | string | [Статус участника события](#event-status) |
| event.calendar_event.created | datetime | Дата и время создания события |
| event.calendar_event.creator.displayName | string | Имя создателя события |
| event.calendar_event.creator.email | string | Email создателя события |
| event.calendar_event.creator.self | boolean | Флаг указывающий на то, что вы создатель события |
| event.calendar_event.reminders | list | Список напоминаний |
| event.calendar_event.reminders.method | string | [Способ напоминания](#event-reminder-method) |
| event.calendar_event.reminders.minutes | number | За сколько минут до начала события сработает напоминание |
| event.calendar_event.all_day | boolean | Флаг указывающий на то, что событие запланировано на весь день |
| event.calendar_event.foreign | string | Внешний уникальный идентификатор события |
| event.calendar_event.recurrence | list | Список повторений [RFC 5545](https://tools.ietf.org/html/rfc5545) |
| event.calendar_event.etag | string | ETag события |
| event.calendar_event.location | string | Географическое местоположение события |
| event.calendar_event.transparency | string | [Доступность события](#event-transparency) |
| event.created | datetime	| Дата и время создания события |
| event.agreement.state | string | [Состояние согласия на хранение Персональных Данных](#pd-agreement-state). Возвращается, если включен модуль Персональных Данных |
| event.agreement.decision_date | datetime | Дата принятия решения по хранению Персональных Данных. Возвращается, если включен модуль Персональных Данных |
| author.id | number | Идентификатор автора действия |
| author.name | string | Имя автора действия |
| author.email | string | Email автора действия |
| account.id | number | Идентификатор организации |
| account.name | string | Название организации |

<a name="action-types"></a>

##### Типы действий над кандидатом

| Тип | Описание |
| --- | -------- |
| ADD | Добавление кандидата в базу |
| VACANCY-ADD | Добавление кандидата на вакансию |
| STATUS | Изменение этапа подбора кандидата |
| COMMENT | Комментарий по кандидату |
| REMOVED | Кандидат удален |
| DOUBLE | Объединение дубликатов |
| AGREEMENT | Действие с согласием на хранение Персональных Данных |

<a name="event-status"></a>

##### Статусы событий календаря

| Тип | Описание |
| --- | -------- |
| confirmed | Подтверждение
| tentative | Предварительное подтверждение
| cancelled | Отказ
| needsAction | Без ответа

<a name="event-type"></a>

##### Типы событий календаря

| Тип | Описание |
| --- | -------- |
| interview | Интервью
| other | Другое

<a name="event-reminder-method"></a>

##### Способы напоминаний

| Тип | Описание |
| --- | -------- |
| sms | Интервью
| other | Другое

<a name="event-transparency"></a>

##### Типы доступности

| Тип | Описание |
| --- | -------- |
| free | Свободен
| busy | Занят

<a name="pd-agreement-state"></a>

##### Состояния согласия на хранение Персональных Данных

| Тип | Описание |
| --- | -------- |
| not_sent | запрос не отправлялся
| sent | запрос отправлен, но ответ не получен
| accepted | получено согласие на хранение
| declined | получен отказ на хранение

### VACANCY

```json
{
    "event": {
        "vacancy": {
            "created": "2017-10-19",
            "money": null,
            "company": null,
            "priority": 0,
            "state": "OPEN",
            "deadline": null,
            "account_division": {
                "id": 1,
                "name": "name"
            },
            "grade": {
                "foreign": "202301",
                "id": 7,
                "name": "1.2"
            },
            "position": "Разработчик интерфейсов",
            "body": "<p>Обязанности</p>",
            "requirements": "<p>Требования</p>",
            "conditions": "<p>Условия</p>",
            "hidden": false,
            "id": 28
        },
        "type": "EDIT",
        "id": 972,
        "created": "2018-01-11T09:54:15+03:00"
    },
    "account": {
          "id": 2,
          "name": "Хантфлоу"
    }
}
```

- a.b обозначает объект a с ключом b


|  Имя | Тип | Описание |
| --- | --- | -------- |
| event.id | number | Идентификатор действия |
| event.type | string | [Тип действия](#vacancy-action-types) |
| event.applicant.id | number | Идентификатор кандидата |
| event.applicant.first_name | string | Имя кандидата |
| event.applicant.last_name | string | Фамилия кандидата |
| event.applicant.middle_name | string | Отчество кандидата |
| event.applicant.birthday | date | Дата рождения кандидата |
| event.applicant.photo.url | string | Сссылка на фотографию кандидата |
| event.vacancy.id | number | Идентификатор вакансии |
| event.vacancy.position | string | Название вакансии (должности) |
| event.vacancy.company | string | Отдел, подразделение (`null`, если подключены подразделения) |
| event.vacancy.money | string | Зарплата |
| event.vacancy.state | string | Статус вакансии |
| event.vacancy.hidden | bool | Скрыта ли вакансия от коллег |
| event.vacancy.priority | number | Приоритет вакансии (может быть или 0 (обычный), или 1 (высокий)) |
| event.vacancy.deadline | date | Дата дедлайна по вакансии |
| event.vacancy.account_division.id | number | Идентификатор подразделения (если подразделения подключены) |
| event.vacancy.account_division.name | string | Название подразделения (если подразделения подключены) |
| event.vacancy.body | string | Обязанности в формате HTML |
| event.vacancy.requirements | string | Требования в формате HTML |
| event.vacancy.conditions | string | Условия в формате HTML |
| event.vacancy.grade | object | Пример внедренного дополнительного поля вакансии типа элемент справочника
| event.vacancy.grade.id | number | Идентификатор значения из справочника |
| event.vacancy.grade.name | string | Название значения из справочника |
| event.vacancy.grade.foreign | string | Идентификатор значения во внешней системе (может быть `null`) |
| event.vacancy.created | datetime | Дата и время создания вакансии |
| event.created | datetime	| Дата и время создания события |
| account.id | number | Идентификатор организации |
| account.name | string | Название организации |

<a name="vacancy-action-types"></a>

##### Типы действий по вакансиям

| Тип | Описание |
| --- | -------- |
| CREATED | Вакансия создана |
| OPEN | Вакансия открыта / переоткрыта |
| CLOSED | Вакансия закрыта |
| HOLD | Работа по вакансии приостановлена |
| RESUME | Работа по вакансии возобновлена (после присотановки) |
| EDIT | Вакансия отредактирована |
| JOIN | Пользователь присоединился к работе по вакансии (к событию будет добавлено поле `user`) |
| LEAVE | Пользователь перестал работать по вакансии (к событию будет добавлено поле `user`) |
