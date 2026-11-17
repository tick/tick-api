Timers
======

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
