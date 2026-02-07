**Natural Next Steps (Still Minimal Philosophy):**

**Background Jobs:**
- Problem: Long-running tasks block HTTP handlers (emails, reports, image processing)
- Solution: `riverqueue` (github.com/riverqueue/river) - Postgres-backed job queue, or `asynq` (github.com/hibiken/asynq) - Redis-backed
- Pattern fit: Both are libraries, not frameworks. You control the workers.

**Structured Logging:**
- Problem: `fmt.Println` doesn't scale when debugging production issues
- Solution: `slog` (stdlib as of Go 1.21) - structured, leveled logging
- Pattern fit: Already in stdlib, zero dependencies

**Configuration Management:**
- Problem: Environment vars get messy beyond 5-6 settings
- Solution: `envconfig` (github.com/kelseyhightower/envconfig) - parse env vars into structs, or `viper` if you need file-based config
- Pattern fit: envconfig is dead simple; viper is heavier but standard

**API Documentation:**
- Problem: Frontend needs to know your endpoints/contracts
- Solution: `swag` (github.com/swaggo/swag) generates OpenAPI docs from comments, or just hand-write a simple OpenAPI YAML
- Pattern fit: You already document in code; swag extracts it

**Metrics/Observability:**
- Problem: "Why is this slow?" questions need data
- Solution: `prometheus` client (github.com/prometheus/client_golang) for metrics, stdlib `net/http/pprof` for profiling
- Pattern fit: Prometheus is industry standard, minimal integration

**File Uploads:**
- Problem: Handling multipart forms, validating file types, storing safely
- Solution: Stdlib `mime/multipart` + your storage layer (filesystem, S3 via `aws-sdk-go-v2`)
- Pattern fit: Build it yourself with stdlib primitives

**Rate Limiting:**
- Problem: Protect endpoints from abuse
- Solution: `tollbooth` (github.com/didip/tollbooth) or `rate` (golang.org/x/time/rate) for simple cases
- Pattern fit: Middleware-based, plugs into stdlib ServeMux

**Mental Model:** These solve problems you _will_ hit as apps grow beyond CRUD. Each maintains your pattern: learn the problem domain first (sessions, jobs, logging), then add minimal tooling when pain is concrete.