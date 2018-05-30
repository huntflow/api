# Work with candidates

* [Adding a candidate to the base](#add)
* [Adding a candidate to the vacancy](#vacancy_applicant)
* [The list of candidates](#applicants)

<a name="add"></a>
## Adding a candidate to the base

Add a JSON to the body of the request:

`POST /account/{account_id}/applicants` 

Add a JSON to the body of the request::

```json
{
    "last_name": "Doe",
    "first_name": "John",
    "middle_name": "Michael",
    "phone": "89260731778",
    "email": "doe@gmail.com",
    "position": "Front-end developer",
    "company": "ХХ",
    "money": "$100000",
    "birthday_day": 20,
    "birthday_month": 7,
    "birthday_year": 1984,
    "photo": 12341,
    "externals": [
        {
            "data": {
                "body": "Resume text\nSome text"
            },
            "auth_type": "NATIVE",
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

### Request fields

* `[]` denotes that the value of the key is n array
* `a.b` denotes and object "a" with a "b" key

Path | Type | Required | Description
---- | -------- | ------------ | --------
last_name | string | Yes | Last name
first_name | string | Yes | First name
middle_name | string | No | Middle name
phone | string | No | Phone number
email | string | No | Email adress
position | string | No | Candidate’s occupation
company | string | No | Candidate’s place of work
money | string | No | Salary expectation
birthday_day | number | No | Day of birth
birthday_month | number | No | Month of birth
birthday_year | number | No | Year of birth
photo | number | No | Candidate’s photo (uploaded file identifier), the information about uploading files is [here](upload.md)
externals[].data.body | string | No | Resume text
externals[].files.id | number | No | Uploaded resume file identifier
externals[].account_source | number | No | Resume source identifier ([guide](dicts.md#applicant_sources))

### Response fields

In addition to the request fields the following fields are added:

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

Name | Type | Description
--- | --- | ---
id | number | Candidate’s ID
created | string | Date and time of adding a candidate
doubles[].double | number | The ID of a duplicated candidate 


<a name="vacancy_applicant"></a>
## Adding a candidate to the vacancy 

`POST /account/{account_id}/applicants/{applicant_id}/vacancy` 

Add a JSON to the body of the request::

```json
{
    "vacancy": 988,
    "status": 1230,
    "comment": "Hello",
    "files": [
        {
            "id": 1382810
        }
    ],
    "rejection_reason": null
}
```

### Request fields

Path | Type | Required | Description
---- | -------- | ------------ | --------
vacancy | number | Yes | [Vacancy] ID(vacancies.md)
status | number | Yes | [The stage of headhunting](dicts.md#vacancy_statuses) 
comment | string | No | Some comment
files | array | No | The array of files attached to the log (not more than 10)
files[].id | number | Yes | File ID
rejection_reason | number | No | The reason of the rejection (if the candidate’s status is 'rejected')

### Response fields

In addition to the request fields the following fields are added:

```json
{
    "id": 279284,
    "changed": "2017-04-10T21:34:01+03:00"
}
```

Name | Type | Description
--- | --- | ---
id | numeric | Binding ID
changed | string | The date of status change

<a name="applicants"></a>
## Getting a list of candidates

`GET /account/{account_id}/applicants` returns the list of companies's candidates.

Accepted parameters:

* `status` — the stage of headhunting. If passed only the candidated at a certain stage ofheadhunting will return.

* `vacancy` — vacancy. Used additionally to "status" for filtering the candidated at a certain stage of headhunting.

* `count`, `page` — [parameters of page by page output](general.md#pagination).

The example of the response:

```json
{
  "count": 2,
  "items": [
    {
      "last_name": "Doe",
      "links": [
        {
          "status": 2,
          "updated": "2017-12-19T21:48:12+03:00",
          "changed": "2017-12-19T21:48:12+03:00",
          "vacancy": 1,
          "id": 172
        }
      ],
      "money": "",
      "id": 290,
      "first_name": "John",
      "middle_name": null,
      "photo": 385,
      "photo_url": "https://store.huntflow.ru/uploads/5/7/0/5700c66c07a23f2e80a0acc0bc3ef6c8.jpeg",
      "email": "hello@mail.ru",
      "company": "Save Factory",
      "phone": "+79511234567",
      "birthday": "1979-06-20",
      "external": [
        {
          "auth_type": "HH",
          "account_source": 2,
          "updated": "2017-12-19T21:48:12+03:00",
          "applicant": 290,
          "id": 290
        }
      ],
      "doubles": [],
      "created": "2017-12-19T21:48:12+03:00",
      "position": "Artist"
    },
    {
      "last_name": "Smith",
      "links": [
        {
          "status": 2,
          "updated": "2017-12-19T21:35:09+03:00",
          "changed": "2017-12-19T21:35:09+03:00",
          "vacancy": 1,
          "id": 171
        }
      ],
      "money": "$30000",
      "id": 289,
      "first_name": "Michael",
      "middle_name": "David",
      "photo": 384,
      "photo_url": "https://store.huntflow.ru/uploads/5/7/0/5700c66c07a23f2e80a0acc0bc3ef6c8.jpeg",
      "email": "test@mail.ru",
      "company": "Doormaker ltd.",
      "phone": "+79639801000",
      "birthday": "1975-01-26",
      "external": [
        {
          "auth_type": "HH",
          "account_source": 2,
          "updated": "2017-12-19T21:35:09+03:00",
          "applicant": 289,
          "id": 289
        }
      ],
      "doubles": [],
      "created": "2017-12-19T21:35:09+03:00",
      "position": "Photographer"
    }
  ],
  "total": 120,
  "page": 1,
  "total_items": 240
}

```

Path |  Description
---- | --------
last_name | Last name
first_name | First name
middle_name | Middle name
phone | Phone number
email | Email address
position | Candidate’s occupation
company | Candidate’s place of work
money | Salary expectation
birthday | Date of birth
photo |Candidate’s photo (uploaded file identifier), the information about uploading files is [here](upload.md)
photo_url | A link to a candidate’s photo
external | Candidate’s resume
external[].auth_type | The format of resume
external[].account_source | The sourse of a resume
external[].updated | The date and time of resume update
links | Candidate’s vacancies
links[].status | The stage of headhunting
links[].vacancy | Vacancy
links[].updated | The date of the candidate’s update at a vacancy
links[].changed | The date of the latest changes at the current stage of headhunting
