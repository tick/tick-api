Tasks
========

Get tasks
------------

* `GET /projects/16/tasks.json` will return all opened tasks for the project.
* `GET /tasks.json` will return all opened tasks across all projects.

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
    "url":"https://www.tickspot.com/api/v2/123/tasks/28.json",
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
    "url":"https://www.tickspot.com/api/v2/123/tasks/28.json",
    "created_at":"2014-09-18T15:03:18.000-04:00",
    "updated_at":"2014-09-18T15:03:18.000-04:00"
  }
]
```

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
    "url":"https://www.tickspot.com/api/v2/123/tasks/28.json",
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
    "url":"https://www.tickspot.com/api/v2/123/tasks/28.json",
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
  "url":"https://www.tickspot.com/api/v2/123/tasks/28.json",
  "created_at":"2014-09-18T15:03:18.000-04:00",
  "updated_at":"2014-09-18T15:03:18.000-04:00",
  "total_hours":12.388,
  "entries":
    {
      "count":5,
      "url":"https://www.tickspot.com/api/v2/123/tasks/16/entries.json",
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
      "url":"https://www.tickspot.com/api/v2/123/projects/16.json",
      "created_at":"2014-09-09T13:36:20.000-04:00",
      "updated_at":"2014-09-09T13:36:20.000-04:00"
    }
}
```

Restricted Methods
----
The following methods **(create, update, and destroy)** are limited strictly to administrators on the subscription.  If a non-administrator calls these methods a `403 Forbidden` will be returned.

Create task
--------------

* `POST /tasks.json` will create a new task from the parameters passed.

```json
{
  "name":"Seriously shield exhaust port",
  "budget":1.0,
  "project_id":16,
  "billable":true,
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
