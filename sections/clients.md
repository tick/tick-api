Clients
========

Get clients
------------

* `GET /clients.json` will return all the clients that have opened projects.
* `GET /clients/all.json` will return all the clients.
```json
[
  {
    "id":12,
    "name":"The Republic",
    "archive":false,
    "url":"https://www.tickspot.com/api/v2/1/clients/12.json",
    "updated_at":"2014-09-09T13:36:20.000-04:00"
  },
  {
    "id":3,
    "name":"The Empire",
    "archive":false,
    "url":"https://www.tickspot.com/api/v2/1/clients/3.json",
    "updated_at":"2014-09-09T13:36:19.000-04:00"
  }
]


```

Get client
----------
* `GET /clients/12.json` will return the specified client along with a summary of project information.

```json
{
  "id":12,
  "name":"The Republic",
  "archive":false,
  "url":"https://www.tickspot.com/api/v2/1/clients/12.json",
  "updated_at":"2014-09-09T13:36:20.000-04:00",
  "projects":{
      "count":1,
      "url":"https://www.tickspot.com/api/v2/123/clients/12/projects.json",
      "updated_at":"2014-09-09T13:36:20.000-04:00"
}
```

Restricted Methods
----
The following methods **(create, update, and destroy)** are limited strictly to administrators on the subscription.  If a non-administrator calls these methods a `403 Forbidden` will be returned.

Create client
-------------
* `POST /clients.json` will create a new client with the included parameters.

```json
{
  "name":"The Hutts",
  "archive":false
}
```

This will return `201 Created`, with the location of the new client in the `Location` header along with the current JSON representation of the client if the creation was a success.

Update client
-------------
* `PUT /clients/12.json` will update an existing client with the included parameters.

```json
{
  "name":"The New Republic",
  "archive":false
}
```

This will return `200 OK` along with the current JSON representation of the updated client if the creation was a success.

Delete client
-------------

* `DELETE /clients/12.json` will delete the client
* Only clients **without any projects** can be deleted
* If successful `204 No Content` will be returned.
* If the client still has associated projects `406 Not Acceptable` will be returned.
