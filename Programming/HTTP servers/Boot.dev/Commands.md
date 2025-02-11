go build -o out && ./out

go install github.com/pressly/goose/v3/cmd/goose@latest
protocol://username:password@host:port/database
psql "postgres://wagslane:@localhost:5432/chirpy"
goose postgres <connection_string> up
(psql chirpy
\dt)
go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest
go get github.com/google/uuid
go get github.com/lib/pq
import _ "github.com/lib/pq"