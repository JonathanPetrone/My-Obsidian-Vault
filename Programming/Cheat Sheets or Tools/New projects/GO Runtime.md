git init
go mod init github.com/jonathanpetrone/ {something}

### Air (for live reload)
**NOTES:** This is in my GOPATH so don't need to download again for every project, but to update version I should:
```bash
go install github.com/air-verse/air@latest
```
#### First time in a project:
```bash
air init    # Creates .air.toml config (optional but recommended)
```
#### Every time you develop:
```bash
air         # Start live reload
```

### HTTP router: 
stdlib `net/http` ServeMux
