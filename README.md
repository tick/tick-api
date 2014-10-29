Tick API
====================

This is version 2 of the Tick API.  This API has been designed around RESTful concepts with JSON for serialization.

URL
---
All URLs start with `https://www.tickspot.com/api/v2/9999`. **SSL only**. The path is prefixed with the subscription id and the API version. If we change the API in backward-incompatible ways, we'll bump the version marker and maintain stable support for the old URLs.


Authentication
--------------

Tick uses a simple token-based HTTP Authentication scheme. For clients to authenticate, the token key should be included in the Authorization HTTP header. The key should be prefixed by the string literal "Token", with whitespace separating the two strings. For example:

```shell
Authorization: Token ApV99yzvwApV99yzvwApV99yzvwApV99yzvw
```

To retrieve your token check out the [roles](http://www.tickspot.com/help) section.

User Agent
----------
A user agent that includes an email address is required to help us contact you should a need arise.

```shell
User-Agent: MyCoolApp (me@example.com)
```
Body Format
----------
All data is serialized with JSON and UTF-8 encoded.  This means that you have to send `Content-Type: application/json; charset=utf-8` when you're POSTing or PUTing data into Tick. All API URLs end in .json to indicate that they accept and return JSON.

You'll receive a `415 Unsupported Media Type` response code if you attempt to use a different URL suffix or leave out the `Content-Type` header.

Sample GET
-------
To make a request for all the projects on your subscription, you'd append the projects index path to the base url to form something like https://www.tickspot.com/999999999/api/v2/projects.json. In cURL, that looks like:

```shell
curl -H "Authorization: Token token=ApV99yzvwApV99yzvwApV99yzvwApV99yzvw" \
     -H 'User-Agent: MyCoolApp (me@example.com)' \
  https://www.tickspot.com/999999999/api/v2/projects.json
```
Sample POST
------------
To create something, it's the same deal except you also have to include the `Content-Type` header and the JSON data:

```shell
curl -H "Authorization: Token token=ApV99yzvwApV99yzvwApV99yzvwApV99yzvw" \
     -H 'Content-Type: application/json' \
     -H 'User-Agent: MyCoolApp (me@example.com)' \
     -d '{ "name": "My new project!", "client_id": "999" }' \
  https://www.tickspot.com/999999999/api/v2/projects.json
```

Pagination
----------

Most collection APIs paginate their results. The first request returns up to
50 records. Check the next page for more results by adding `&page=2`, then
`&page=3`, and so on until you get an empty response.

Use HTTP caching
----------------

You must make use of the HTTP freshness headers to lessen the load on our servers (and increase the speed of your application!). Most requests we return will include an `ETag` or `Last-Modified` header. When you first request a resource, store this value, and then submit them back to us on subsequent requests as `If-None-Match` and `If-Modified-Since`. If the resource hasn't changed, you'll see a `304 Not Modified` response, which saves you the time and bandwidth of sending something you already have.


Handling errors
---------------

If Tick is having trouble, you might see a 5xx error. `500` means that the app is entirely down, but you might also see `502 Bad Gateway`, `503 Service Unavailable`, or `504 Gateway Timeout`. It's your responsibility in all of these cases to retry your request later.


Thanks!
----------------------

Thank you for checking out Tick's API and please don't hesitate to reach out and let us know how you are using the API to help people track their time and hit their budgets.  Feel free to use GitHub issues to post any issues or feature requests.

Please tell us how we can make the API better. If you have a specific feature request or if you found a bug, please use GitHub issues. Fork these docs and send a pull request with improvements.

To talk with us and other developers about the API, [post a question on StackOverflow](http://stackoverflow.com/questions/ask) tagged `tick` or [check out our support section](http://www.tickspot.com/help).
