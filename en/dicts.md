# Guides

* [Stages of headhunting](#vacancy_statuses)
* [Sources of resumes](#applicant_sources)


<a name="vacancy_statuses"></a>
## Stages of headhunting

`GET /account/{account_id}/vacancy/statuses` returns a list of stages of headhunting for a company.

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
            "name": "Declined",
            "removed": null
        }
    ]
}
```

Name | Type | Description
 --- | --- | ---
 id | number | Stage identifier
 type | string | Status type (user – made by a manager, trash – system stage 'declined')
 order | number | Sort order
 name | string | The name of a stage
 removed | date+time | If stated means that the stage was deleted. The stage is saved only as a history of candidate workflow. The candidate can’t be transfered to this stage.


<a name="applicant_sources"></a>
## The sources of resumes

`GET /account/{account_id}/applicant/sources` returns a list of resume sources of a company.

```json
{
    "items": [
        {
            
            "id": 209,
            "name": "Agency",
            "type": "user",
            "foreign": null
        },
        {
            "id": 199,
            "name": "HeadHunter",
            "type": "system",
            "foreign": "HH"
        }
