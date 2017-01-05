Projects
========

Get projects
------------

* `GET /projects.json` will return opened projects.
* `GET /projects/closed.json` will return closed projects.

* The first request returns up to 100 records and you can check the next page for more results by adding the `page` parameter to your request.  For example, `page=2`, then `page=3`, and so on until you get an empty response.

```json
[
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
  },
  {
    "id":4,
    "name":"Clean Up Alderaan",
    "budget":50.0,
    "date_closed":null,
    "notifications":false,
    "billable":true,
    "recurring":false,
    "client_id":3,
    "owner_id":3,
    "url":"https://www.tickspot.com/api/v2/123/projects/4.json",
    "created_at":"2014-09-09T13:36:19.000-04:00",
    "updated_at":"2014-09-09T13:36:19.000-04:00"
  }
]
```

Get project
-----------

* `GET /projects/16.json` will return the specified project
* Additionally the total hours that have been entered as well as a summary of task information is included.

```json
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
  "updated_at":"2014-09-09T13:36:20.000-04:00",
  "total_hours":22.0,
  "tasks":
    {
      "count":1,
      "url":"https://www.tickspot.com/api/v2/123/projects/16/tasks.json",
      "updated_at":null
    },
  "client":
    {
      "id":12,
      "name":"Empire",
      "archive":false,
      "url":"https://www.tickspot.com/api/v2/123/clients/12.json",
      "updated_at":"2014-09-15T10:32:46.000-04:00"
    }
}
```

Restricted Methods
----
The following methods **(create, update, and destroy)** are limited strictly to administrators on the subscription.  If a non-administrator calls these methods a `403 Forbidden` will be returned.

Create project
--------------

* `POST /projects.json` will create a new project from the parameters passed.
* No tasks will be created
* Time entries will not be allowed until at least one task is created.

```json
{
  "project":
    {
      "name":"Prepare Star Destroyer",
			"budget":50.0,
			"notifications":false,
      "billable":true,
      "recurring":false,
      "client_id":12,
      "owner_id":3
		}
}
```

This will return `201 Created`, with the location of the new project in the `Location` header along with the current JSON representation of the project if the creation was a success.


Update project
---------------

* `PUT /projects/16.json` will update the project from the parameters passed.

```json
{
  "project":
    {
      "budget":300,
      "billable":true
		}
}
```

This will return `200 OK` if the update was a success along with the current JSON representation of the project.

Delete project
-------------

* `DELETE /projects/16.json` will delete the project
* **WARNING:** The project **and all time entries** will be immediately deleted
* If successful `204 No Content` will be returned.
