Users
========

Get users
------------

* `GET /users.json` will return information about the users on the subscription
* Non-administrators will only have visibility of themselves while administrators will see everyone

```json
[
  {
    "id":4,
    "first_name":"Anakin",
    "last_name":"Skywalker",
    "email":"user@tickspot.com",
    "timezone":"Hawaii",
    "updated_at":"2014-11-19T12:53:46.000-05:00"
  },
  {
    "id":1,
    "first_name":"Luke",
    "last_name":"Skywalker",
    "email":"owner@tickspot.com",
    "timezone":"Hawaii",
    "updated_at":"2015-01-30T15:13:44.000-05:00"
  }
]
```

Get deleted users
------------

* `GET /users/deleted.json` will return users who have been deleted from the subscription and have time entries
* Non-administrators will not have access

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

```json
{ 
"user":
   {
    "first_name":"Anakin",
    "last_name":"Skywalker",
    "email":"admin@tickspot.com",
    "admin":"true",
    "billable_rate": "100.0"
   }
 }
```
