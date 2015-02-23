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