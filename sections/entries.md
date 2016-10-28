Entries
========

Get entries
------------

* `GET /entries.json` will return all time entries that meet the provided parameters
* `GET /users/4/entries.json`
* `GET /projects/16/entries.json`
* `GET /tasks/24/entries.json`
* Either a ```start_date``` and ```end_date``` have to be provided **or** an ```updated_at``` time
* Depending on what parameters are provided:
  * Entries will be between the ```start_date``` and ```end_date``` or
  * Entries created or modified after the ```updated_at``` time
* Each of the following **optional** parameters can be used to filter the response:
  * ```billable``` (true/false)
  * ```project_id```
  * ```task_id```
  * ```user_id```
  * ```billed``` (true/false)
* Example returns billable time entries between two dates
```shell
start_date='2014-09-01'&end_date='2014-09-02'&billable=true"
```
* Example returns a user's entries that have been updated after a specific time
```shell
updated_at='2014-09-18T15:03:20.000-04:00'&user_id=12"
```

```json

[
  {
    "id":"234",
    "date":"2014-09-17",
    "hours":2.88,
    "notes":"Stocking up on Bacta.",
    "task_id":24,
    "user_id":4,
    "url":"https://www.tickspot.com/api/v2/123/entries/234.json",
    "created_at":"2014-09-18T15:03:19.000-04:00",
    "updated_at":"2014-09-18T15:03:19.000-04:00"
  },

  {
    "id":"235",
    "date":"2014-09-17",
    "hours":12,
    "notes":"Making the Kessel run.",
    "task_id":24,
    "user_id":4,
    "url":"https://www.tickspot.com/api/v2/123/entries/235.json",
    "created_at":"2014-09-18T15:03:19.000-04:00",
    "updated_at":"2014-09-18T15:03:19.000-04:00"
  }

]
```


Get entry
-----------

* `GET /entries/235.json` will return the specified entry

```json
{
  "id":"235",
  "date":"2014-09-17",
  "hours":12,
  "notes":"Making the Kessel run.",
  "task_id":24,
  "user_id":4,
  "url":"https://www.tickspot.com/api/v2/123/entries/235.json",
  "created_at":"2014-09-18T15:03:19.000-04:00",
  "updated_at":"2014-09-18T15:03:19.000-04:00",
  "task":
    {
    "id":"24",
    "name":"Install exhaust port",
    "budget":14.0,
    "position":1,
    "project_id":16,
    "date_closed":null,
    "billable":true,
    "url":"https://www.tickspot.com/api/v2/123/tasks/24.json",
    "created_at":"2014-09-18T15:03:18.000-04:00",
    "updated_at":"2014-09-18T15:03:18.000-04:00"
    },
}
```

Create entry
--------------

* `POST /entries.json` will create a new entry from the parameters passed
* The ```user_id``` will be ignored if the user is not an administrator

```json
{
  "date":"2014-09-18",
  "hours":1.5,
  "notes":"Chasing Ewoks",
  "task_id":24,
  "user_id":4
}
```

This will return `201 Created`, with the location of the new entry in the `Location` header along with the current JSON representation of the entry if the creation was a success.


Update entry
---------------

* `PUT /entries/235.json` will update the entry from the parameters passed

```json
{
  "hours":12.5,
  "billed":true
}
```

This will return `200 OK` if the update was a success along with the current JSON representation of the entry.

Delete entry
-------------

* `DELETE /entries/235.json` will delete the entry
* If successful `204 No Content` will be returned
