## Проверка статуса выполнения отложенной задачи

`GET /account/{account_id}/delayed_task/{task_id}`

Идентификатор задачи (`task_id`) – идентификатор в поле `payload.task_id` в ответе на запрос, вызвавший отложенную задачу.

### Пример запроса

`GET /account/3/delayed_task/b5174006-b46f-49fd-a16b-6fc0baf69d5f`

### Пример ответа

```json
{
    "states_log": [
        {
            "state": "enqueued",
            "comment": null,
            "datetime": "2019-06-20T04:55:01+00:00",
            "timestamp": 1561006501.3160393
        },
        {
            "state": "inprogress",
            "comment": null,
            "datetime": "2019-06-20T04:55:01+00:00",
            "timestamp": 1561006501.318867
        },
        {
            "state": "success",
            "comment": null,
            "datetime": "2019-06-20T04:55:01+00:00",
            "timestamp": 1561006501.391373
        }
    ],
    "state": "success",
    "task_id": "11271792-f244-47c4-95a3-3e5973e7ffdc",
    "updated_datetime": "2019-06-20T04:55:01+00:00",
    "created": 1561006501.3160393,
    "created_datetime": "2019-06-20T04:55:01+00:00",
    "updated": 1561006501.391373
}
```


Путь | Тип | Описание
---- | --- | --------
state | string | Текущий статус задачи (`enqueued`/`inprogress`/`success`/`failed`) 
created | unixtimestamp | Unix timestamp создания задачи
updated | unixtimestamp | Unix timestamp последнего обновления задачи
created_datetime | datetime | Дата и время (в ISO8601) создания задачи
updated_datetime | datetime | Дата и время (в ISO8601) последнего обновления задачи
states_log | array | Лог изменений по задаче
