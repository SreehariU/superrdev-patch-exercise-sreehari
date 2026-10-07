# Patch Notes

Repo: https://github.com/SreehariU/superrdev-patch-exercise-sreehari

## Summary

I fixed a SQL operator-precedence bug causing incorrect filtering, removed an artificial backend delay that blocked requests, and fixed the React error state so failed requests no longer leave the UI stuck in loading.

## Priorities

1. **SQL correctness:** `status=OPEN` returned other statuses and search returned archived tasks, so users saw wrong data.
2. **Backend performance:** a `Thread.sleep()` blocked request threads.
3. **Frontend error handling:** failed requests left the UI on "Loading tasks...".

## How I Debugged

- Tested the API with `curl` rather than assuming the frontend was at fault. Inspecting the repository query revealed missing parentheses around the `OR`.
- Confirmed the delay with `time curl`; the controller slept based on query length.
- In `useTasks.js`, `catch` set `error` but never reset `loading`.

## What Changed

- **SQL:** `archived = FALSE AND (title LIKE ... OR description LIKE ...) AND status = ...`. Previously `AND` bound tighter than `OR`.
- **Controller:** removed the complexity calculation, `Thread.sleep()`, and related logging.
- **Hook:** `setLoading(false)` in `catch`; `setError(null)` when a new request starts.

## Not Changed

- No debouncing or request cancellation: I did not reproduce a stale-response race, and the exercise was timeboxed.
- No unrelated UI or framework changes.

## Biggest Remaining Risk

The backend fetches all matching tasks and paginates in memory. Fine for this dataset; I would move to database-level pagination at scale.

## Tools / AI

Used ChatGPT to inspect code and reason about the SQL and loading-state bugs. I reproduced the issues with curl and the browser, reviewed the proposed changes, and applied them manually.
