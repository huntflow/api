# Работа с вакансиями

* [Adding a vacancy to the base](#add)
* [Editing a vacancy](#edit)
* [The list of vacancies](#vacancies)
* [Getting a vacancy](#vacancy)
* [Deleting a vacancy](#delete)
* [The state of a vacancy](#vacancy-states)

<a name="add"></a>
## Adding a vacancy to the base

`POST /account/{account_id}/vacancies`

Add a JSON to the body of the request:

```json
{
    "position": "Manufacturing Engineer",
    "company": "Tesla",
    "money": "$100k",
    "deadline": "2017-09-03",
    "applicants_to_hire": 2,
    "priority": 1,
    "account_division": 6,
    "coworkers": [1],
    "body": "<p>Some text</p>",
    "requirements": "<p>Another text</p>",
    "conditions": "<p>Different text</p>",
    "hidden": false,
    "state": "OPEN",
    "files": [1, 2, 3]
}
```

### Request fields

Name | Type | Required | Description
 --- | --- | --- | ---
 position | string | Yes | The name of the vacancy (occupation)
 company | string | No | Department (igored if the DEPARTMENTS are enabled)
 money | string | No | Salary
 applicants_to_hire | number | No | How many people to hire (>= 1)
 deadline | date | No | Due date for a vacancy
 priority | number | No | The priority of a vacancy (0 for usual or 1 for high)
 account_division | number | No | Department identifier (if the DEPARTMENTS are enabled)
 coworkers | array | No | The list of recruiters working with a vacancy
 body | string | No | The responsibilities of a vacancy in HTML format. Enabled tags: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**, **b**, **i**
 requirements | string | No | The requiremetns for a vacancy in HTML format. Enabled tags: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**, **b**, **i**
 conditions | string | No | The conditions for a vacancy in HTML format. Enabled tags: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**,**b**, **i**
 hidden | bool | No | Is the vacancy hidden from the colleagues?
 state | string | No | [The state of a vacancy](#vacancy-states). `OPEN`by default
 files | array | No | The list of files attached to a vacancy ([file upload](upload.md))

### Response fields

In addition to the request fields the following fields are added:

```json
{
    "id": 1,
    "created": "2017-04-10T21:34:01+03:00"

}
```

Name | Type | Description
--- | --- | ---
id | number | Vacancy ID 
created | string | Date and time of creating a vacancy

<a name="edit"></a>
## Editing a vacancy

`PUT /account/{account_id}/vacancies/{vacancy_id}`

### Request fields
The body of the request is similar to the one of creating a vacancy.
### Response fields
The response is similar to the one returning for the request of creating a vacancy. 
<a name="delete"></a>
# Deleting a vacancy

`DELETE /account/{account_id}/vacancies/{vacancy_id}`

### Request fields
The body of the request shall be empty

# Response fields
```json
{
    "status": true
}
```

Name | Type | Description
--- | --- | ---
status | bool | Indicator of a successful operation

<a name="vacancies"></a>
## Getting the list of vacancies

`GET /account/{account_id}/vacancies` returns the list of vancancies of a company. 

Accepeted parameters:

* `mine` — logical field.
If passed, only the vacancies the current user is working with will return. 

* `opened` — logical field.
If passed, only the open vacancies will return.

* `count`, `page` — [parameters of page by page output](general.md#pagination).

```json
{
    "items": [
        {
            "id": 4531,
            "position": "Sales manager",
            "company": "Sales department",
            "money": "$65K",
            "deadline": "2017-04-27",
            "applicants_to_hire": 1,
            "created": "2017-03-22T18:16:27+03:00",
            "vacancy_request": null,
            "priority": 0,
            "hidden": false,
            "state": "OPEN"
        },
        {
            "id": 4530,
            "position": "Python developer",
            "company": "DEV department",
            "money": "$112K",
            "deadline": null,
            "applicants_to_hire": 1,
            "created": "2017-03-22T18:16:27+03:00",
            "vacancy_request": null,
            "priority": 0,
            "hidden": false,
            "state": "CLOSED"
        }
    ],
    "total": 1,
    "page": 1,
    "count": 30
}
```

<a name="vacancies-result"></a>

Name | Type | Description
 --- | --- | ---
 id |  number | Vacancy ID
 position | string | The name of the vacancy (occupation)
 company | string | Department
 money | string | Salary
 deadline | date | Due date for a vacancy
 applicants_to_hire | number | How many people to hire
 created | date+time | Date and time of creating a vacancy
 vacancy_request | number | The ID of a request for a vacancy that the vacancy was created from.
 priority | number | The priority of a vacancy (0 for usual or 1 for high)
 hidden | bool | Is the vacancy hidden from the colleagues?
 state | string | [The state of a vacancy](#vacancy-states)


<a name="vacancy"></a>
## Getting a vacancy

`GET /account/{account_id}/vacancies/{vacancy_id}` returns a vacancy with an ID `{vacancy_id}`

```json
{
    "id": 4531,
    "position": "Sales manager",
    "company": "Sales department",
    "money": "$65K",
    "deadline": "2017-04-27",
    "applicants_to_hire": 1,
    "created": "2017-03-22T18:16:27+03:00",
    "vacancy_request": null,
    "priority": 0,
    "hidden": false,
    "state": "OPEN",
    "body": "<p>Some important text</p>",
    "requirements": "<p>And some more</p>",
    "conditions": "<p>Sand some more</strong></p>",
    "files": [
        {
            "id": 15808,
            "name": "Screenshot 2017-04-10 at 11.00.13.png",
            "content_type": "image/png",
            "url": "https://store.huntflow.ru/uploads/f/f/h/ffhov94xuqytbl16u8b9l3oeewdjpyoc.png"
        }
    ]
}
```

The returned fields are similar to data from [the list of vancancies](#vacancies-result) with added fields:

Name | Type | Description
 --- | --- | ---
 body | string | The responsibilities of a vacancy in HTML format
 requirements | string | The requiremetns for a vacancy in HTML format
 conditions | string | The conditions for a vacancy in HTML format
 files | array | The list of files attached to a vacancy


<a name="vacancy-states"></a>
## The state of a vacancy

Identifier | Description
 --- | ---
 OPEN | The vacancy is open
 CLOSED | The vacancy is closed
 HOLD | The work on a vacancy is paused
