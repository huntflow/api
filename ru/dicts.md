# Справочники

* [Этапы подбора](#vacancy_statuses)
* [Источники резюме](#applicant_sources)


<a name="vacancy_statuses"></a>
## Этапы подбора

`GET /account/{account_id}/vacancy/statuses` вернёт список этапов подбора компании.

```json
{
    "items": [
        {
            "id": 206,
            "type": "user",
            "order": 1,
            "name": "New Lead",
            "removed": null
        },
        {
            "id": 207,
            "type": "user",
            "order": 2,
            "name": "Hired",
            "removed": null
        },
        {
            "id": 215,
            "type": "trash",
            "order": 9999,
            "name": "Отказ",
            "removed": null
        }
    ]
}
```

Имя | Тип | Описание
 --- | --- | ---
 id | number | Идентификатор этапа
 type | string | Тип статуса (`user` – созданный управляющим, `trash` – системный этап `Отказ`)
 order | number | Порядок сортировки
 name | string | Название этапа
 removed | datetime | Если установлена, то это означет, что этап был удален. Для сохранения истории работы по кандидату этап остается для отображения. Однако, перевести кандидата на этот этап невозможно.


<a name="applicant_sources"></a>
## Источники резюме

`GET /account/{account_id}/applicant/sources` вернёт список источников резюме компании.

```json
{
    "items": [
        {

            "id": 209,
            "name": "Агентство",
            "type": "user",
            "foreign": null
        },
        {
            "id": 199,
            "name": "HeadHunter",
            "type": "system",
            "foreign": "HH"
        }
    ]
}
```

Имя | Тип | Описание
 --- | --- | ---
 id | number | Идентификатор источника
 name | string | Название источника
 type | string | Тип источника (`user` – созданный пользователем, `system` – системный источник)
 foreign | string | Связь источника (используется только для системных источников)
