# Production calendar service

* [General calendar info](#production_calendar)
* [Info about non-working days in given period](#days)
* [Bulk request for non-working days](#days-bulk)
* [Deadline date evaluation taking into account the non-working days](#deadline)
* [Bulk request for deadline date evaluation](#deadline-bulk)
* [Start date evaluation taking into account the non-working days](#start)
* [Bulk request for start date evaluation](#start-bulk)

<a name="production_calendar"></a>
## General calendar info

`GET /production_calendar` returns list of production calendars for different countries

```json
{
    "items": [
        {
            "id": 1,
            "name": "Russian Federation"
        }
    ]
}
```

`GET /production_calendar/{calendar_id}` returns production calendar with the `{calendar_id}` identifier
```json
{
    "id": 1,
    "name": "Russian Federation"
}
```
### Response fields
Name | Type | Description
--- | --- | ---
id | number | Unique calendar identifier
name | str | Calendar title

<a name="days"></a>
## Info about non-working days in given period

`GET /production_calendar/{calendar_id}/days/{deadline}` returns info about non-working day till the `{deadline}` date in the `YYYY-MM-DD` format, according to the production calendar with `{calendar_id}`.

Request fields:

* `start` — date in the `YYYY-MM-DD` format.
A date to start counting of non-working days. Optional. Default is today.

* `verbose` — boolean field.
Extends the response with the `items` field — list of dates, weekends and holidays within given range; in `YYYY-MM-DD` format.

### Response fields

In addition to the request fields the following fields are added:

```json
{
    "total_days": 36,
    "not_working_days": 28,
    "items": ["2019-01-01"]
}
```
Name | Type | Description
--- | --- | ---
total_days | number | Total amount of days within the range
not_working_days | number | Amount of non-working days within the range
items | list | List of dates, weekends and holidays within the range

<a name="days-bulk"></a>
## Bulk request for non-working days

`POST /production_calendar/{calendar_id}/days` bulk version of the previous request.

### Request fields:
* `verbose` — boolean field.
Extends the response with the `items` field — list of dates, weekends and holidays within given range; in `YYYY-MM-DD` format.

Request body in the JSON format:
```json
[
    {"deadline": "2019-04-20"},
    {"deadline": "2019-05-20", "start": "2018-05-20"}
]
```

### Request fields

Name | Type | Required | Description
 --- | --- | --- | ---
 deadline | date | Yes | End of the range
 start | date | No | Begin of the range; Optional, default is today

```json
[
    {
        "total_days": 36,
        "not_working_days": 28,
        "items": ["2019-01-01"]
    }
]
```
Every element in the response list has the same fields as in the single request.

<a name="deadline"></a> 
## Deadline date evaluation taking into account the non-working days

`GET /production_calendar/{calendar_id}/deadline/{days}` - returns a deadline after `{days}` working days, according to the calendar with `{calendar_id}`.

### Request fields:

* `start` — date in `YYYY-MM-DD` format.
A date to start counting. Optional. Default is today.


###  Response example
```
"2019-01-25"
```

<a name="deadline-bulk"></a>
## Bulk request for deadline date evaluation

`POST /production_calendar/{calendar_id}/deadline` bulk version of the previous request.

Request body in the JSON format:
```json
[
    {"days": 10},
    {"days": 20},
    {"days": 30, "start": "2007-09-01"}
]
```
### Request fields

Name | Type | Required | Description
 --- | --- | --- | ---
 days | number | Yes | Amount of working days
 start | date | No | A date to start counting. Optional. Default is today.

 ###  Response example
```
[
    "2019-01-25",
    "2019-03-01"
]
```

<a name="start"></a> 
## Start date evaluation taking into account the non-working days

`GET /production_calendar/{calendar_id}/start/{days}` - returns a date in `{days}` working days ahead, according to `{calendar_id}` production calendar.

Request fields:

* `deadline` — date in `YYYY-MM-DD` format.
A date to start reverse counting. Optional. Default is today.


###  Response example
```
"2018-09-25"
```

## Bulk request for start date evaluation
<a name="start-bulk"></a> 

`POST /production_calendar/{calendar_id}/start` bulk version of the previous request.

Request body in the JSON format:
```json
[
    {"days": 10},
    {"days": 20},
    {"days": 30, "deadline": "2017-09-01"}
]
```
### Request fields

Name | Type | Required | Description
 --- | --- | --- | ---
 days | number | Yes | Amount of working days to count
 deadline | date | No | A date to finish the reverse counting. Optional. Default is today.

 ###  Response example
```
[
    "2018-07-25",
    "2018-03-01"
]
```



