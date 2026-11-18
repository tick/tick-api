Timers
======

Get timers
----------

* `GET /timers.json` returns all timers for the authenticated user on this subscription in one array. It does not paginate.
* `GET /timers/7.json` returns one of those timers. A timer outside that scope returns `404 Not Found`, including when the caller is an administrator.

```json
{
  "id": 7,
  "task_id": 24,
  "last_started_at": "2026-11-18T09:30:00-05:00",
  "accumulated_seconds": 600,
  "notes": "Inspect the exhaust port",
  "role_id": 4,
  "updated_at": "2026-11-18T09:30:00-05:00"
}
```

`accumulated_seconds` contains time accumulated before the current run. For a running timer, add the elapsed seconds since `last_started_at` to display the elapsed time. A paused timer has a null `last_started_at`. Tick caps a timer at 24 hours when calculating its elapsed time.

Update a timer
--------------

`PUT /timers/7.json` updates the task, notes, or both for one of your timers on this subscription. The task must belong to the subscription. The response is the updated timer with `200 OK`.

```json
{
  "timer": {
    "task_id": 24,
    "notes": "Inspect the repaired exhaust port"
  }
}
```

This endpoint accepts only `task_id` and `notes`. Use the start and pause endpoints to change the timer's running state.

Create a running timer
----------------------

`POST /timers.json` starts a timer for the given task. Include the task ID and optional notes in the `timer` object.

```json
{
  "timer": {
    "task_id": 24,
    "notes": "Inspect the exhaust port"
  }
}
```

The response is `201 Created`. Use the timer's `id` in subsequent requests. Starting a timer pauses any other running timer for the same user on this subscription.

Pause or resume a timer
-----------------------

* `PUT /timers/7/pause.json` pauses timer 7 and preserves the accumulated time.
* `PUT /timers/7/start.json` resumes a paused timer.

Each request returns the timer with `200 OK`. A paused timer has a null `last_started_at`.

Save a timer as a time entry
--------------------------

`POST /entries/create_from_timer.json` records the elapsed time for a running or paused timer. Send the timer ID at the top level of the request body.

```json
{ "timer_id": 7 }
```

Tick returns the new [entry](entries.md#get-entry) with `201 Created` and removes the timer. The entry uses the timer's task and notes and the current date in the user's time zone. To discard a timer without recording time, use `DELETE /timers/7.json`.
