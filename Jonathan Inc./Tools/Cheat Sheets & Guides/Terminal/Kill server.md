lsof -ti :PORT | xargs kill -9

Replace `PORT` with your port number, e.g. `:8080`.

- `lsof -ti :8080` — finds the PID of whatever's using that port
- `xargs kill -9` — force-kills it

### List processes port 8080
```bash
lsof -i :8080
```

## Kill process
```bash
kill 45672
```