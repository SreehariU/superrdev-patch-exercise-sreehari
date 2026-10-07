# Patch Notes

## Summary

- Fixed the task search SQL so archived tasks remain excluded and the optional status filter applies to both title and description matches.
- Removed an artificial request-thread delay from the task search endpoint.
- Fixed the React task-loading state so failed requests clear `loading` and stale errors are reset when a new request starts.

## What I Did Not Change

- Did not add search debouncing or request cancellation because a stale-result race was not reproduced during testing and the timebox favored focused fixes.
- Did not change unrelated framework warnings, styling, or lower-impact task data issues.

## Biggest Remaining Risk

The backend currently retrieves all matching tasks and then paginates the results in memory. This is acceptable for the current dataset, but database-level pagination would scale better for a larger task table.

## Tools / AI

Used ChatGPT to inspect the code, identify candidate issues, and reason about the SQL operator-precedence bug and frontend loading-state bug. I reproduced the issues using curl and the browser, reviewed the proposed changes, and applied the changes manually.
