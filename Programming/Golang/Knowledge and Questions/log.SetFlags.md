The `log.SetFlags(log.LstdFlags | log.Lshortfile)` line configures what information gets included in your log output.

**`log.LstdFlags`** is the default flag combination that includes:

- Date (2009/01/23)
- Time (01:23:23)

**`log.Lshortfile`** adds:

- Filename and line number where the log was called (e.g., `main.go:15`)

The `|` operator combines these flags using bitwise OR.

So your log output will look like:

```
2025/06/06 14:30:45 main.go:15 ❌ Failed to load .env file
```

Instead of just:

```
❌ Failed to load .env file
```