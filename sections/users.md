Users
========

Get users
------------

* `GET /users.json` will return information about the users on the subscription
* Administrators see every current user. Other users see only themselves.
* This list returns all visible users in one response and ignores `page`.

```json
[
  {
    "id":4,
    "first_name":"Anakin",
    "last_name":"Skywalker",
    "email":"user@tickspot.com",
    "timezone":"Hawaii",
    "billable_rate":null,
    "updated_at":"2014-11-19T12:53:46.000-05:00"
  },
  {
    "id":1,
    "first_name":"Luke",
    "last_name":"Skywalker",
    "email":"owner@tickspot.com",
    "timezone":"Hawaii",
    "billable_rate":null,
    "updated_at":"2015-01-30T15:13:44.000-05:00"
  }
]
```

Get a user
----------

`GET /users/4.json` returns one user object with the same fields as an item in `GET /users.json`, including `billable_rate`. Administrators can look up any current user on the subscription. Other users can look up their own user ID. A request for a user outside that scope returns `404 Not Found`.

Get deleted users
------------

* `GET /users/deleted.json` will return users who have been deleted from the subscription and have time entries
* This admin-only list returns all matching users in one response and ignores `page`.

```json
[
  {
    "id":5,
    "first_name":"Darth",
    "last_name":"Vader",
    "email":"dv@tickspot.com",
    "timezone":"Death Star",
    "updated_at":"2014-11-19T12:53:46.000-05:00"
  }
]
```

Create user
------------

* `POST /users.json` will create a user on the subscription
* Only administrators will be able to create users

Send the user attributes inside a `user` object. Use a JSON boolean for `admin` and a number for `billable_rate`. Tick also accepts `"true"` or `"false"` for `admin` and a numeric string for `billable_rate`.

```json
{
  "user": {
    "first_name": "Anakin",
    "last_name": "Skywalker",
    "email": "admin@tickspot.com",
    "admin": true,
    "billable_rate": 100.0
  }
}
```
