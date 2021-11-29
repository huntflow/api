# Work with vacancies

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
    "deadline": "2017-09-03", # DEPRECATED (moved to fill_quotas)
    "applicants_to_hire": 2,  # DEPRECATED (moved to fill_quotas)
    "priority": 1,
    "account_division": 6,
    "coworkers": [1],
    "body": "<p>Some text</p>",
    "requirements": "<p>Another text</p>",
    "conditions": "<p>Different text</p>",
    "hidden": false,
    "state": "OPEN",
    "files": [1, 2, 3],
    "vacancy_request": 11,    # DEPRECATED (moved to fill_quotas)
    "fill_quotas": [
        {
            "deadline": "2017-09-03",
            "applicants_to_hire": 2,
            "vacancy_request": 11,
        }
    ]
}
```

### Request fields

Name | Type | Required | Description
 --- | --- | --- | ---
 position | string | Yes | The name of the vacancy (occupation)
 company | string | No | Department (ignored if the DEPARTMENTS are enabled)
 money | string | No | Salary
 applicants_to_hire | number | No | How many people to hire (>= 1)
 deadline | date | No | Due date for a vacancy
 priority | number | No | The priority of a vacancy (0 for usual or 1 for high)
 account_division | number | No | Department identifier (if the DEPARTMENTS are enabled)
 coworkers | array | No | The list of recruiters working with a vacancy
 body | string | No | The responsibilities for a vacancy in HTML format. Enabled tags: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**, **b**, **i**
 requirements | string | No | The requirements for a vacancy in HTML format. Enabled tags: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**, **b**, **i**
 conditions | string | No | The conditions for a vacancy in HTML format. Enabled tags: **ul**, **ol**, **li**, **p**, **br**, **a**, **strong**, **em**, **u**,**b**, **i**
 hidden | bool | No | Is the vacancy hidden from the colleagues?
 state | string | No | [The state of a vacancy](#vacancy-states). `OPEN`by default
 files | array | No | The list of files attached to a vacancy ([file upload](upload.md))
 ~vacancy_request~ | number | No | DEPRECATED Identifier of a request for a vacancy] (moved to fill_quotas structure)
 fill_quotas | array | No | List of fill quotas, see [Description of fill quotas](#fill-quotas)


 *Notice*

During vacancies creation it is acceptable not more than one item in `fill_quotas` list.

Until *2020-06-01* there is enabled backward compatibility (not full) with old API:
* If `fill_quotas` field is missed in the request structure, then will be processed `applicants_to_hire`,
`deadline`, `vacancy_request` fields from root-level of the request body (except vacancy edit case,
see below).

On vacancy creation these fields will be used to compose a fill quota structure.
On vacancy edit these fields may be used to change existing fill quota attached to the vacancy, but
only in case of single fill quota on the vacancy.
If there are more than one fill quota on the vacancy, then will be returned an error (http status
400), because it's impossible to guess which fill quota must be edited.

An item of a fill quotas list is a structure. If the structure contains a fill quota's identifier,
then will be changed this existing quota. If the structure does not contain a fill quota's
identifier, then will be added a new fill quota to the vacancy. There is no way to delete a fill
quota from a vacancy.

Limitations:
* On adding a new vacancy it's forbidden to specify more than one fill quota.
* On adding a new vacancy it's forbidden to specify an ID in fill quota structure (because a new
  fill quota should be created)
* The same vacancy request can not be attached more than one time (to one or several vacancies).

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

`GET /account/{account_id}/vacancies` returns the list of vacancies of a company. 

Accepted parameters:

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
 requirements | string | The requirements for a vacancy in HTML format
 conditions | string | The conditions for a vacancy in HTML format
 files | array | The list of files attached to a vacancy


<a name="vacancy-states"></a>
## The state of a vacancy

Identifier | Description
 --- | ---
 OPEN | The vacancy is open
 CLOSED | The vacancy is closed
 HOLD | The work on a vacancy is paused

<a name="fill-quotas"></a>
## Fill quotas

A vacancy can have one or more fill quotas. Fill quotas bind vacancies, vacancy requests (optional),
a number of applicants to hire and deadlines to close vacancies.
Selectable number of fill quotas for one vacancy allows to precise control times of vacancy
requests closing. And at the same time there may be several vacancy requests on one vacancy, also
requests may be attached to the vacancy at any moment.
For a vacancy it's allowed to have one fill quota without a vacancy request.


Fill quota structure looks like:

```json
{
    "id": 123,
    "deadline": "2020-03-12",
    "applicants_to_hire": 3,
    "vacancy_request": 234,
}
```

Name | Type | Description
 --- | --- | ---
 id | number | Fill quota identifier
 deadline | string | Date when the quota should be filled (YYYY-MM-DD)
 applicants_to_hire | number | Number of applicants should be hired on the fill quota
 vacancy_request | number | Vacancy request identifier


On vacancy edit requests it may be specified fill quotas list to add or change in `fill_quotas` field.
If an item of `fill_quotas` list doesn't contain ID of a quota, then it means a new quota should be
created. If the ID is specified, then will be edited already existing fill quota.
It's forbidden to hire an applicant on a fill quota if the quota is filled (number of
applicants_to_hire <= already hired on the quota).


Retrieving a list of fill quotas


`GET /account/{account_id}/vacancy/{vacancy_id}/quotas` returns fill quotas bound to a vacancy with identifier `{vacancy_id}`


Parameters:

* `count`, `page` — [pagination parameters](general.md#pagination).

Returned structure looks like:

```json
{
    "Vacancy ID": {
        "count": 30,
        "page": 1,
        "total": 1,
        "items": [
            {
              "already_hired": 1,
              "applicants_to_hire": 2,
              "closed": null,
              "created": "2020-02-03 19:47:59",
              "deadline": "2020-03-04",
              "id": 64,
              "vacancy_request": null,
              "account_info": {
                  "email": "email of a user who had opened the fill quota",
                  "id": 135,
                  "name": "name of a user who had opened the fill quota"
              }
            },
            ...
        ]
    }
}
```

Name | Type | Description
 --- | --- | ---
 id | number | Fill quota identifier
 deadline | string | Fill quota deadline (YYYY-MM-DD)
 applicants_to_hire | number | Number of people should be hired on the quota
 vacancy_request | number | Vacancy request ID
 created | string | Fill quota creation date
 closed | string | Fill quota closing date
 already_hired | number | How many people already hired on the quota at the moment


## Splitting an applicant to a child vacancy
`PUT /account/{account_id}/vacancy/{vacancy_id}/split`

Add a JSON to the body of the request:
```json
{"applicant": 1, "status": 1}
```

### Request fields
Name | Type | Required | Description
 --- | --- | --- | ---
 applicant | number | Yes | Applicant ID
 status    | number | Yes | [The stage of headhunting](dicts.md#vacancy_statuses) 
 
### Response fields
```json
{
    "applicant": 1,
    "id": 8,
    "status": 1,
    "vacancy": 4,
    "vacancy_parent": 3
}
```

Name | Type | Description
--- | --- | ---
applicant | number | Applicant ID
id | number | Applicant log ID
status | number | [The stage of headhunting](dicts.md#vacancy_statuses) 
vacancy | number | Child vacancy ID
vacancy_parent | number | Parent vacancy ID