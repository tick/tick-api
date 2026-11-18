Tasks
========

Get tasks
------------

* `GET /projects/16/tasks.json` will return all opened tasks for the project.
* `GET /tasks.json` returns opened tasks across projects visible to you.

Add `user_id=4` to select tasks whose projects are assigned to user 4. The filter follows the same [access rules as project filtering](projects.md#get-projects) and never expands the caller's access. It applies to open and closed task lists, including the nested project URLs.

Add `include_budget=true` to include `total_hours` and `budget_remaining` on each returned task. Totals include all recorded entries for that task across users and dates, even when `user_id` selects the tasks. Values are rounded to three decimal places. Remaining hours equal the task's budget minus its total hours, can be negative, and are null for a null budget. An empty task has zero hours. Unsaved timers do not count.

```http
GET /projects/16/tasks.json?user_id=4&include_budget=true
```

Omit `include_budget` or use `false` to keep the standard response. Invalid boolean values or malformed user IDs return `400 Bad Request`. Unknown or inaccessible users return `404 Not Found`. Collection pages contain at most 100 tasks.

The standard response follows.

```json
[
  {
    "id":"25",
    "name":"Install exhaust port",
    "budget":14.0,
    "position":1,
    "project_id":16,
    "date_closed":null,
    "billable":true,
    "url":"https://www.tickspot.com/123/api/v2/tasks/25.json",
    "created_at":"2014-09-18T15:03:18.000-04:00",
    "updated_at":"2014-09-18T15:03:18.000-04:00"
  },
  {
    "id":"26",
    "name":"Shield exhaust port",
    "budget":1.0,
    "position":2,
    "project_id":16,
    "date_closed":null,
    "billable":true,
    "url":"https://www.tickspot.com/123/api/v2/tasks/26.json",
    "created_at":"2014-09-18T15:03:18.000-04:00",
    "updated_at":"2014-09-18T15:03:18.000-04:00"
  }
]
```

Get recent tasks
----------------

`GET /tasks/recent.json` returns up to 15 tasks. Tick starts with the authenticated user's recently used, assigned open tasks and fills remaining places with other visible open tasks. The result is sorted for display, not by recency, and does not paginate.

Each task has the standard fields except `url`, plus `project_name` and `client_name`. You can add `include_budget=true` for budget summaries. The `user_id` filter narrows the tasks by project assignment; it does not switch the usage history to that user's history.

Get closed tasks
------------

* `GET /projects/16/tasks/closed.json` will return all closed tasks for the project.
* `GET /tasks/closed.json` will return all closed tasks across all projects.

```json
[
  {
    "id":"25",
    "name":"Install exhaust port",
    "budget":14.0,
    "position":1,
    "project_id":16,
    "date_closed":"2014-09-18",
    "billable":true,
    "url":"https://www.tickspot.com/123/api/v2/tasks/25.json",
    "created_at":"2014-09-18T15:03:18.000-04:00",
    "updated_at":"2014-09-18T15:03:18.000-04:00"
  },
  {
    "id":"26",
    "name":"Shield exhaust port",
    "budget":1.0,
    "position":2,
    "project_id":16,
    "date_closed":"2014-09-18",
    "billable":true,
    "url":"https://www.tickspot.com/123/api/v2/tasks/26.json",
    "created_at":"2014-09-18T15:03:18.000-04:00",
    "updated_at":"2014-09-18T15:03:18.000-04:00"
  }
]
```

Get task
-----------

* `GET /tasks/25.json` will return the specified task
* Additionally the total hours that have been entered as well as a summary of entry information is included.

```json
{
  "id":"25",
  "name":"Install exhaust port",
  "budget":14.0,
  "position":1,
  "project_id":16,
  "date_closed":null,
  "billable":true,
  "created_at":"2014-09-18T15:03:18.000-04:00",
  "updated_at":"2014-09-18T15:03:18.000-04:00",
  "total_hours":12.388,
  "entries":
    {
      "count":5,
      "url":"https://www.tickspot.com/123/api/v2/tasks/25/entries.json",
      "updated_at":"2014-09-18T15:03:21.000-04:00"
    },
  "project":
    {
      "id":16,
      "name":"Build Death Star",
      "budget":150.0,
      "date_closed":null,
      "notifications":false,
      "billable":true,
      "recurring":false,
      "client_id":12,
      "owner_id":3,
      "url":"https://www.tickspot.com/123/api/v2/projects/16.json",
      "created_at":"2014-09-09T13:36:20.000-04:00",
      "updated_at":"2014-09-09T13:36:20.000-04:00"
    }
}
```

Restricted Methods
----
Only subscription administrators can create, update, and delete tasks. Other users receive `401 Unauthorized`.

Create task
--------------

* `POST /tasks.json` will create a new task from the parameters passed.

```json
{
  "name":"Seriously shield exhaust port",
  "budget":1.0,
  "project_id":16,
  "billable":true
}
```

This will return `201 Created`, with the location of the new task in the `Location` header along with the current JSON representation of the task, if the creation was a success.


Update task
---------------

* `PUT /tasks/25.json` will update the task from the parameters passed.

```json
{
  "budget":2,
  "billable":false
}
```

This will return `200 OK` if the update was a success along with the current JSON representation of the task.

Delete task
-------------

* `DELETE /tasks/25.json` will delete the task
* Only tasks **without any entries** can be deleted
* If successful `204 No Content` will be returned.
* If the task still has associated entries `406 Not Acceptable` will be returned.
