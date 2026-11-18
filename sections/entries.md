Entries
========

Get entries
------------

* `GET /entries.json` will return all time entries that meet the provided parameters
* `GET /users/4/entries.json`
* `GET /projects/16/entries.json`
* `GET /tasks/24/entries.json`

Supply both `start_date` and `end_date` to select entries by date. The range includes both dates. Alternatively, supply `updated_at` to select entries created or modified after the given timestamp. Optional filters are `billable` and `billed` (true or false), `project_id`, `task_id`, and `user_id`.

Each page returns up to 100 entries. For a range spanning several months or years, repeat the same query with `page=2`, then `page=3`, until the API returns `[]`.

For example, to get the second page of billable entries for September 2014, use this path.

```http
GET /entries.json?start_date=2014-09-01&end_date=2014-09-30&billable=true&page=2
```

To get entries for user 12 that changed after a specific time, use this path.

```http
GET /entries.json?updated_at=2014-09-18T15:03:20-04:00&user_id=12
```

The response reports `hours` rounded to four decimal places.

```json

[
  {
    "id":"234",
    "date":"2014-09-17",
    "hours":2.88,
    "notes":"Stocking up on Bacta.",
    "task_id":24,
    "user_id":4,
    "url":"https://www.tickspot.com/123/api/v2/entries/234.json",
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
    "url":"https://www.tickspot.com/123/api/v2/entries/235.json",
    "created_at":"2014-09-18T15:03:19.000-04:00",
    "updated_at":"2014-09-18T15:03:19.000-04:00"
  }

]
```


Get this week's entries
-----------------------

`GET /entries/this_week.json` returns the authenticated user's entries for Monday through Sunday of the current week, using the subscription's time zone. It returns all matching entries in one array and ignores `page` and the supplied `user_id`, including for administrators.

The endpoint sets `start_date` and `end_date` for you. Omit `updated_at` when requesting this week: supplying it selects entries modified after that timestamp instead of limiting results to the week. The optional `project_id`, `task_id`, `billable`, and `billed` filters still apply.

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
  "url":"https://www.tickspot.com/123/api/v2/entries/235.json",
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
    "url":"https://www.tickspot.com/123/api/v2/tasks/24.json",
    "created_at":"2014-09-18T15:03:18.000-04:00",
    "updated_at":"2014-09-18T15:03:18.000-04:00"
    }
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
