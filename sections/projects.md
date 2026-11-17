Projects
========

Get projects
------------

* `GET /projects.json` will return opened projects.
* `GET /projects/closed.json` returns closed projects visible to the authenticated user. Administrators see all closed projects in the subscription. Other users see only their assigned closed projects.

Project lists ignore `user_id`. Administrators can look up the [users assigned to each project](#get-project-users).

The first request returns up to 100 records and you can check the next page for more results by adding the `page` parameter to your request.  For example, `page=2`, then `page=3`, and so on until you get an empty response.

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
Only subscription administrators can list project users or create, update, or delete projects. Other users receive `401 Unauthorized`.

Get project users
-----------------

`GET /projects/16/users.json` returns the users assigned to project 16 in a single array. Each object has the same fields as a [subscription user](users.md#get-a-user). An empty array means the project has no assigned users.

Create project
--------------

* `POST /projects.json` will create a new project from the parameters passed.
* By default, Tick assigns all current subscription users. To select users, include a top-level `assign_user_ids` array alongside `project`.
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

Replace project assignments
---------------------------

To replace the assigned users in `PUT /projects/16.json`, send the complete list of subscription user IDs alongside a nonempty `project` object. This request updates the budget and assigns users 3 and 4.

```json
{
  "project": { "budget": 300 },
  "assign_user_ids": [3, 4]
}
```

Tick removes assignments omitted from the list. An empty list clears all assignments. Omit `assign_user_ids` to keep the current assignments. If two clients replace the list concurrently, the later request can overwrite the earlier change.

Delete project
-------------

* `DELETE /projects/16.json` will delete the project
* **WARNING:** The project **and all time entries** will be immediately deleted
* If successful `204 No Content` will be returned.
