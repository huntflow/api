# Работа с резюме

* [Получение резюме кандидата](#view)
* [Типы резюме](#auth_types)

<a name="view"></a>
## Получение резюме кандидата

`GET /account/{account_id}/applicants/{applicant_id}/external/{external_id}`, где external_id — [идентификатор резюме.](applicants.md#external_id)

Пример ответа:

```json
{
    "auth_type": "HH",
    "files": [],
    "updated": "2018-05-15T12:49:03+03:00",
    "created": "2018-05-15T12:49:03+03:00",
    "account_source": 2,
    "foreign": "580ab9e3ff02cd5e710039ed1f74593273674e",
    "portfolio": [],
    "data": {
        "last_name": "Александрова",
        "citizenship": [
            {
                "url": "https://api.hh.ru/areas/113",
                "id": "113",
                "name": "Россия"
            }
        ],
        "resume_locale": {
            "id": "RU",
            "name": "Русский"
        },
        "photo": {
            "40": "http://hh.ru/photo/454047540.jpeg?t=1448523580&h=GUxmvLbNy4iynV2F0Tmjug",
            "100": "http://hh.ru/photo/454047541.jpeg?t=1448523580&h=8IZ6jezosevP-EG1uJ-1_A",
            "500": "http://hh.ru/photo/454047542.jpeg?t=1448523580&h=TUuVh4M24U_svUG1VbiF-w",
            "small": "http://hh.ru/photo/454047541.jpeg?t=1448523580&h=8IZ6jezosevP-EG1uJ-1_A",
            "medium": "https://huntflow.ru/static/i/template/appl2.jpeg"
        },
        "business_trip_readiness": {
            "id": "never",
            "name": "Не готов к командировкам"
        },
        "new_views": null,
        "updated_at": "2015-11-16T18:38:16+0300",
        "next_publish_at": null,
        "travel_time": {
            "id": "any",
            "name": "Не имеет значения"
        },
        "recommendation": [],
        "owner": {
            "comments": {
                "url": "https://api.hh.ru/applicant_comments/1276707"
            }
        },
        "portfolio": [],
        "education": {
            "elementary": [],
            "attestation": [],
            "level": {
                "id": "higher",
                "name": "Высшее"
            },
            "primary": [
                {
                    "name": "Московский государственный университет им. М.В. Ломоносова, Москва",
                    "organization_id": null,
                    "name_id": "39144",
                    "result": null,
                    "year": 2010,
                    "organization": "Химический факультет",
                    "result_id": null
                }
            ],
            "additional": []
        },
        "employment": {
            "id": "full",
            "name": "Полная занятость"
        },
        "id": "580ab9e3ff02cd5e710039ed1f74593273674e",
        "blocked": null,
        "first_name": "Анна",
        "middle_name": "Сергеевна",
        "specialization": [
            {
                "profarea_name": "Начало карьеры, студенты",
                "id": "15.389",
                "name": "Продажи",
                "profarea_id": "15"
            }
        ],
        "certificate": [],
        "area": {
            "url": "https://api.hh.ru/areas/2",
            "id": "2",
            "name": "Санкт-Петербург"
        },
        "relocation": {
            "type": {
                "id": "no_relocation",
                "name": "не готов к переезду"
            },
            "area": []
        },
        "work_ticket": [
            {
                "url": "https://api.hh.ru/areas/113",
                "id": "113",
                "name": "Россия"
            }
        ],
        "employments": [
            {
                "id": "full",
                "name": "Полная занятость"
            }
        ],
        "total_experience": {
            "months": 71
        },
        "status": null,
        "_progress": null,
        "schedule": {
            "id": "fullDay",
            "name": "Полный день"
        },
        "total_views": null,
        "site": [],
        "finished": null,
        "skill_set": [],
        "schedules": [
            {
                "id": "fullDay",
                "name": "Полный день"
            }
        ],
        "salary": {
            "currency": "RUR",
            "amount": 50000
        },
        "can_view_full_info": true,
        "language": [
            {
                "id": "rus",
                "name": "Русский",
                "level": {
                    "id": "native",
                    "name": "родной"
                }
            }
        ],
        "metro": null,
        "skills": "Прямые продажи, продажи по телефону, (выявление потребностей клиента, формирование, и продажа), телемаркетинг, взаимодействовать с клиентами, повышение лояльности, консультирование, решение конфликтных ситуаций, уверенное знание ПК, умение находить общий язык с каждым клиентом",
        "gender": {
            "id": "male",
            "name": "Мужской"
        },
        "age": 24,
        "title": "Менеджер по продажам",
        "experience": [
            {
                "industries": [
                    {
                        "id": "41.517",
                        "name": "Розничная сеть (продуктовая)"
                    },
                    {
                        "id": "41.512",
                        "name": "Розничная сеть (drogerie, товары повседневного спроса)"
                    }
                ],
                "end": null,
                "description": "Продажа услуг компании как новым так и действующим клиентом, кросс-продажи, клиентоориентированный подход, устранение технических неполадок.",
                "area": {
                    "url": "https://api.hh.ru/areas/113",
                    "id": "113",
                    "name": "Россия"
                },
                "company_url": "http://www.X5.ru",
                "industry": null,
                "company_id": "4233",
                "employer": {
                    "url": "https://api.hh.ru/employers/4233",
                    "alternate_url": "http://hh.ru/employer/4233",
                    "logo_urls": {
                        "90": "http://hh.ru/employer/logo/4233"
                    },
                    "id": "4233",
                    "name": "X5 RETAIL GROUP"
                },
                "start": "2010-01-01",
                "position": "Менеджер по продажам",
                "company": "X5 RETAIL GROUP"
            }
        ],
        "views_url": null,
        "contact": [
            {
                "comment": null,
                "type": {
                    "id": "cell",
                    "name": "Мобильный телефон"
                },
                "value": {
                    "country": "7",
                    "number": "212-85-06",
                    "city": "812"
                },
                "preferred": false
            },
            {
                "type": {
                    "id": "email",
                    "name": "Эл. почта"
                },
                "value": "hello@huntflow.ru",
                "preferred": true
            }
        ],
        "birth_date": "1991-04-08",
        "alternate_url": "http://hh.ru/resume/580ab9e3ff02cd5e710039ed1f74593273674e",
        "created_at": "2015-11-16T10:29:22+0300"
    },
    "id": 3
}
```

Путь | Тип | Описание
---- | --- | --------
id | number | Идентификатор резюме
auth_type | string | Тип резюме
files[].id | number | Идентификатор файла загруженного резюме
files[].name | string | Имя файла резюме
files[].url | string | URL файла резюме
files[].content_type | string | MIME-тип файла резюме
portfolio[].small | string | URL уменьшенного изображения портфолио
portfolio[].medium | string | URL изображения среднего размера
portfolio[].description | string | Описание изображения портфолио
updated | datetime | Дата и время последнего редактирования резюме
created | datetime | Дата в время создания резюме
account_source | number | Идентификатор [источника резюме](dicts.md#applicant_sources)
foreign | number | Внешний идентификатор резюме
data | object | Объект резюме (формат зависит от auth_type)
data.body | string | Текст резюме (для резюме с auth_type = NATIVE)


<a name="auth_types"></a>
## Типы резюме
auth_type | Формат резюме
--------- | -------------
NATIVE | Текстовый формат
SJ | [SuperJob](https://api.superjob.ru/client/#resume_descr)
HH | [HeadHunter](https://github.com/hhru/api/blob/master/docs/resumes.md#item)
LI | LinkedIn (формат аналогичен [HeadHunter](https://github.com/hhru/api/blob/master/docs/resumes.md#item))*
VK | VK
MK | [Moi Krug](https://moikrug.ru/info/api#q1.4)
ZPRU | [Zarplata.ru](http://api.zp.ru/v1/)
WORKUA | Work Ua (формат аналогичен [HeadHunter](https://github.com/hhru/api/blob/master/docs/resumes.md#item))*
RABOTAUA | Rabota Ua (формат аналогичен [HeadHunter](https://github.com/hhru/api/blob/master/docs/resumes.md#item))*
GITHUB | Github (формат аналогичен [HeadHunter](https://github.com/hhru/api/blob/master/docs/resumes.md#item))*
FACEBOOK | Facebook (формат аналогичен [HeadHunter](https://github.com/hhru/api/blob/master/docs/resumes.md#item))*
AVITO | Avito (формат аналогичен [HeadHunter](https://github.com/hhru/api/blob/master/docs/resumes.md#item))*

\* Форматы, аналогичные HeadHunter являются справочными и могут измениться в будущем
