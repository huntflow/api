## Получение информации о лицензии пользователя

`GET /account/{account_id}/license`

### Пример запроса

`GET /account/3/delayed_task/b5174006-b46f-49fd-a16b-6fc0baf69d5f`

### Пример ответа

```json
{
  "begin_at": "2020-11-02T16:00:54.939767+03:00",
  "created_at": "2020-11-02T16:00:49.703283+03:00",
  "end_at": null,
  "id": 28,
  "scheduled_begin_at": "2020-11-01T00:00:00+03:00",
  "scheduled_end_at": "2021-02-03T23:59:59+03:00",
  "tariff_description": "Все самое необходимое для построения процесса подбора и автоматизации работы рекрутеров",
  "tariff_name": "Базовый (20201123)",
  "workplace_limit": 5,
  "services": [
    {
      "code": "survey_type_a",
      "limits": [
        {
          "limit_type_code": "active_service_count",
          "value": 1.0
        }
      ],
      "name": "Формы обратной связи"
    },
    {
      "code": "survey_type_r",
      "name": "Оценка рекрутмента"
    },
    {
      "code": "read_email_tracking",
      "name": "Трекинг открытия писем"
    },
    {
      "code": "followups",
      "name": "Фоллоу-аппы"
    },
    {
      "code": "schedule_email",
      "name": "Отложенная отправка писем"
    },
    {
      "code": "sms",
      "name": "SMS"
    },
    {
      "code": "ip_telephony",
      "name": "IP-телефония"
    },
    {
      "code": "time_on_state_limit",
      "name": "Ограничение времени кандидатов на этапах"
    },
    {
      "code": "time_on_state_report",
      "name": "Отчет по среднему времени нахождения кандидатов на этапах"
    },
    {
      "code": "email_conversion_report",
      "name": "Отчет по конверсии писем"
    },
    {
      "code": "themes",
      "name": "Темы оформления"
    },
    {
      "code": "api",
      "name": "API"
    },
    {
      "code": "view_applicants_in_reports",
      "name": "Просмотр списка кандидатов в отчетах"
    },
    {
      "code": "calendar_scheduler",
      "name": "Планировщик календаря"
    },
    {
      "code": "watchers",
      "limits": [
        {
          "limit_type_code": "active_service_count",
          "value": 5.0
        }
      ],
      "name": "Ограничение на число заказчиков"
    }
  ]
}
```


Поле | Тип | Описание | Обязательность
---- | --- | -------- | --------------
begin_at | unixtimestamp | Фактическая дата начала действия лицензии | Да
end_at | unixtimestamp | Фактическая дата окончания действия лицензии | Нет 
scheduled_begin_at | unixtimestamp | Планируемая дата начала действия лицензии | Да
scheduled_end_at | unixtimestamp | Планируемая дата окончания действия лицензии | Нет
created_at | unixtimestamp | Дата создания лицензии | Да
id | integer | Идентификатор лицензии | Да
tariff_description | string | Описание тарифа | Да
tariff_name | string | Название тарифа | Да
workplace_limit | integer | Лимит количества рекрутеров | Да
services | array | Список услуг, входящих в лицензию | Да
services[].code | string | Идентификатор услуги | Да
services[].name | string | Название услуги | Да
services[].limits[] | array | Список лимитов услуги | Нет
services[].limits[].limit_type_code | string | Идентификатор лимита (`active_service_count`) | Нет
services[].limits[].value | float | Значение лимита | Нет

