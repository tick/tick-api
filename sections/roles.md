Roles
========
Access to the Tick API requires the use of an API Token.  Users can be granted access to multiple subscriptions and these are called **roles**.
Unlike other areas of the API, a username and password must be provided using basic authentication.

```shell
curl -u "email_address:password" \
     -H 'User-Agent: MyCoolApp (me@example.com)' \
  https://www.tickspot.com/api/v2/roles.json
```

API Token
------------

* Each role that a user is granted will automatically be assigned a token.
* `GET /roles.json` will return all roles for the authenticated user.

```json
[
 {
   "subscription_id":15,
   "company":"Empire",
   "api_token":"f67158e7bf3d7a0fcaf9d258ace8b468"
 },
 {
   "subscription_id":16,
   "company":"Republic",
   "api_token":"f67158e7bf3d7a0fcaf9d258ace8b468"
 }
]
```