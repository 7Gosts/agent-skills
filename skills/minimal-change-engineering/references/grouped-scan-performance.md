# Grouped Scan Performance

Use this guidance when source data can contain consecutive items with the same key and each item must find a position in a target table or list.

## Avoid Repeated Full Scans

Scanning the target from the beginning for every source item can approach `O(m * n)` for `m` source items and `n` target rows.

Prefer one of these established shapes:

- build a `key -> positions` index when the target can be indexed once;
- maintain a `key -> scan_cursor` when matching order and incremental writes matter;
- batch reads or writes when the external API supports them.

After a successful match or write, advance the cursor for that key to the next useful position.

## Recover from a Stale Cursor

When target data can change concurrently, search in two segments before falling back:

1. from the saved cursor to the end;
2. from the beginning to the saved cursor.

Rebuild the index or perform a full scan only when the cursor cannot be trusted or neither segment finds a valid position.

## Report the Real Bottleneck

State the expected complexity before and after the change. An index or monotonic cursor can reduce local scanning toward `O(n + m)`, but remote reads and writes may still dominate wall-clock time. Report network round trips separately from in-memory complexity.

If headers or column definitions accompany row data, update both structures together and keep their column order aligned.
