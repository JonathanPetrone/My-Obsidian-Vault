# Concurrent Systems Design Reference

A practical guide to understanding hardware constraints, concurrency patterns, and system optimization.

---

## Table of Contents

1. [Hardware Fundamentals](https://claude.ai/chat/bf4e5764-ce37-4222-99d2-ef2f064de17b#hardware-fundamentals)
2. [Workload Characteristics](https://claude.ai/chat/bf4e5764-ce37-4222-99d2-ef2f064de17b#workload-characteristics)
3. [Core Concurrency Patterns](https://claude.ai/chat/bf4e5764-ce37-4222-99d2-ef2f064de17b#core-concurrency-patterns)
4. [Resource Optimization](https://claude.ai/chat/bf4e5764-ce37-4222-99d2-ef2f064de17b#resource-optimization)
5. [Pattern Selection Framework](https://claude.ai/chat/bf4e5764-ce37-4222-99d2-ef2f064de17b#pattern-selection-framework)
6. [Real-World Example: YouTube Transcript Summarizer](https://claude.ai/chat/bf4e5764-ce37-4222-99d2-ef2f064de17b#real-world-example-youtube-transcript-summarizer)

---

## Hardware Fundamentals

### CPU Cores

**What they are:**

- CPU core = processor core = physical computation unit
- Each core executes instructions independently
- Modern machines: 4-16 cores typical (Apple Silicon M1/M2/M3: 8-10 cores)

**Key insight:** Sequential code only uses one core regardless of how many are available. The system doesn't automatically parallelize your code.

**Check your cores:**

```bash
# macOS
sysctl -n hw.ncpu

# In Go
import "runtime"
cores := runtime.NumCPU()
```

**Performance impact:**

- Sequential: 1 core at 100%, others idle → 12% total CPU usage (on 8-core machine)
- Concurrent: All cores working → 80-100% total CPU usage
- Same hardware, 8× faster with proper concurrency

### RAM (Memory)

**How it constrains concurrency:**

Each worker (goroutine) consumes memory:

- **Stack space:** ~2KB initial per goroutine (grows dynamically)
- **Work data:** File contents, API responses, intermediate results

**Practical limits:**

- 1,000 goroutines ≈ 2MB (stack only)
- 100,000 goroutines ≈ 200MB+ (plus growth)
- Real constraint: work data, not goroutine overhead

**Example - Document processing:**

- Bad: Load all 10,000 × 500KB docs = 5GB RAM → might OOM
- Good: Worker pool of 20, each holds one doc = 10MB RAM

**Worker pool solves this:**

- Bounded workers = bounded memory
- Only N items in memory simultaneously
- Rest wait in queue (just pointers, negligible memory)

**Memory pressure effects:**

- System swaps to disk when RAM full
- Swapping extremely slow (milliseconds vs nanoseconds)
- Too many workers → RAM exhausted → swapping → slower than fewer workers

### CPU vs GPU

**CPU (Central Processing Unit):**

- Few powerful, flexible cores (4-16)
- Strength: Complex logic, branching, varied operations
- Use: General computation, web servers, most application code

**GPU (Graphics Processing Unit):**

- Thousands of simple cores (2,000-10,000+)
- Strength: Same simple operation on massive data simultaneously
- Use: Graphics, ML training, video processing, scientific computing

**Key difference:**

- CPU = Smart generalist (8 workers solving different complex problems)
- GPU = Army of specialists (5,000 workers doing identical simple tasks)

**Why ML uses GPUs:**

- Neural network training = billions of matrix multiplications
- GPU does this 100-1000× faster than CPU
- CPUs can do it, just takes days instead of hours

**Mac-specific:**

- M-series chips have **unified memory** (CPU and GPU share same RAM pool)
- No separate VRAM, no copying between CPU/GPU memory
- Faster for small tasks, but total memory shared

**For most application development:** You're using CPU only. GPU work requires specialized libraries/frameworks.

---

## Workload Characteristics

### CPU-Bound vs I/O-Bound

Understanding workload type determines optimal concurrency strategy.

**CPU-Bound Work:**

- Pure computation, no waiting
- Examples: Image processing, parsing, compression, matrix math
- Worker always computing, never idle
- **Optimal workers ≈ number of cores** (8 workers for 8 cores)
- More workers = context switching overhead, no benefit

**I/O-Bound Work:**

- Waiting for external resources
- Examples: File reads, network requests, database queries, API calls
- Worker spends most time **waiting**, core sits idle
- **Optimal workers >> number of cores** (50-200+ workers common)

**Example: API requests (I/O-bound)**

Each request: 2000ms total

- 10ms: CPU (prepare request, parse response)
- 1990ms: Waiting for network/API

With 8 workers (matching cores):

- Core usage: ~1% (idle 99% of time)
- Throughput: 4 requests/second

With 100 workers:

- Cores stay busy switching between ready workers
- While 99 wait on network, 1 ready worker uses core
- Core usage: 50-80%
- Throughput: 50 requests/second

**Mental model:** Cores are cooks, workers are orders. If every order requires waiting (oven timer), you want many more orders than cooks so cooks always have something ready.

### Practical Guidelines

|Work Type|Workers Formula|Example|
|---|---|---|
|Pure CPU|`cores` or `cores + 1-2`|Image processing: 8 workers for 8 cores|
|Mixed|`cores × 2-4`|Web server with DB calls: 16-32 workers|
|Heavy I/O|`cores × 10-50`|API client with rate limits: 80-400 workers|

**For document processing with API calls:** 20-50 workers typical even on 8 cores.

---

## Core Concurrency Patterns

### 1. Worker Pool

**Pattern:**

```go
jobs := make(chan Job, 100)
results := make(chan Result, 100)

// Spawn N workers
for i := 0; i < 5; i++ {
    go worker(jobs, results)
}

// Send jobs
for _, job := range allJobs {
    jobs <- job
}
close(jobs)
```

**Optimizes for:**

- Predictable resource usage (exactly N goroutines)
- Worker lifecycle management (initialization, cleanup per worker)
- Stateful workers (each maintains connection pool, cache)
- Metrics per worker

**Use when:**

- Workers need state (database connections, caches)
- Need to track which worker processed what
- Resource usage must be strictly bounded

**Trade-offs:**

- More complex setup
- Workers might sit idle with uneven job distribution

---

### 2. Semaphore (Bounded Concurrency)

**Pattern:**

```go
sem := make(chan struct{}, 5)

for _, job := range jobs {
    go func(j Job) {
        sem <- struct{}{}        // Acquire
        defer func() { <-sem }() // Release
        process(j)
    }(job)
}
```

**What it is:**

- Semaphore = fixed number of tokens/permits
- Workers acquire token before proceeding
- When done, release token
- If no tokens available, workers wait

**Buffered channel as semaphore:**

- Channel capacity = max concurrent workers
- Send = acquire token (blocks if full)
- Receive = release token
- Automatic coordination

**Optimizes for:**

- Simplicity (spawn and forget)
- Dynamic scaling (goroutines created/destroyed as needed)
- Independent, stateless jobs
- Maximum N concurrent regardless of total jobs

**Use when:**

- Jobs are independent and stateless
- Don't need worker lifecycle management
- Want simple bounded concurrency

**Trade-offs:**

- Goroutine creation overhead
- Harder to track individual workers

---

### 3. Pipeline (Staged Processing)

**Pattern:**

```go
stage1 := fetch(inputs)      // Returns channel
stage2 := transform(stage1)  // Consumes, returns channel
stage3 := store(stage2)      // Consumes, returns channel
```

**Optimizes for:**

- Different bottlenecks per stage (I/O vs CPU vs rate-limited)
- Backpressure (slow stage naturally throttles fast stage)
- Composability (add/remove stages)
- Streaming (start processing before all input arrives)

**Use when:**

- Multi-stage processing with different characteristics
- Stages have different optimal concurrency
- Want natural backpressure

**Example:**

```go
// Stage 1: I/O bound, 20 workers
transcripts := fetchPipeline(urls, 20)

// Stage 2: API rate limited, 5 workers
summaries := processPipeline(transcripts, 5)

// Stage 3: Sequential writes, 1 worker
results := storePipeline(summaries, 1)
```

**Trade-offs:**

- More complex coordination
- Buffer sizing decisions between stages

---

### 4. Rate Limiter

**Pattern:**

```go
limiter := time.NewTicker(100 * time.Millisecond)
defer limiter.Stop()

for job := range jobs {
    <-limiter.C  // Wait for tick
    go process(job)
}
```

**Optimizes for:**

- External API rate limits
- Preventing resource exhaustion
- Smooth traffic patterns
- Quota compliance

**Use when:**

- Calling rate-limited APIs
- Need to prevent overwhelming downstream services
- Compliance with quotas (requests/minute)

**Advanced: Semaphore + Rate Limiter:**

```go
type RateLimitedPool struct {
    maxConcurrent  int              // e.g., 5 simultaneous
    requestsPerMin int              // e.g., 50
    semaphore      chan struct{}
    rateLimiter    *time.Ticker
}

func (p *RateLimitedPool) Process(jobs <-chan Job) {
    for job := range jobs {
        p.semaphore <- struct{}{}  // Wait for worker slot
        <-p.rateLimiter.C          // Wait for rate limit
        
        go func(j Job) {
            defer func() { <-p.semaphore }()
            process(j)
        }(job)
    }
}
```

**Trade-offs:**

- Artificial slowdown even when capacity available
- Need to tune rate to match actual limits

---

### 5. Fan-Out / Fan-In

**Pattern:**

```go
// Fan-out: One source, many processors
results := make([]chan Result, 5)
for i := range results {
    results[i] = make(chan Result)
    go process(input, results[i])
}

// Fan-in: Merge many sources into one
merged := merge(results...)
```

**Optimizes for:**

- Parallel processing of same data
- Load distribution across heterogeneous workers
- Aggregating from multiple sources

**Use when:**

- Same data needs multiple transformations
- Combining results from independent workers
- Redundant processing for reliability

**Trade-offs:**

- Synchronization complexity
- Potential for unbalanced load

---

### 6. Errgroup (Coordinated Goroutines)

**Pattern:**

```go
import "golang.org/x/sync/errgroup"

g, ctx := errgroup.WithContext(context.Background())

for _, job := range jobs {
    g.Go(func() error {
        return process(ctx, job)
    })
}

if err := g.Wait(); err != nil {
    // First error cancels all others
}
```

**Optimizes for:**

- Error propagation (one failure stops all)
- Context cancellation
- Waiting for group completion
- Limiting concurrent goroutines (SetLimit)

**Use when:**

- All-or-nothing semantics (one failure invalidates all)
- Need coordinated cancellation
- Want simple error handling

**Trade-offs:**

- Might waste work if early failure
- All-or-nothing might be too strict

---

### 7. Select (Event Multiplexing)

**Pattern:**

```go
for {
    select {
    case job := <-jobs:
        process(job)
    case <-ctx.Done():
        return
    case <-ticker.C:
        reportStatus()
    }
}
```

**Optimizes for:**

- Reacting to multiple event sources
- Timeouts and cancellation
- Non-blocking operations
- Priority handling

**Use when:**

- Need to handle multiple channels
- Want timeout capability
- Need graceful shutdown

**Trade-offs:**

- Sequential processing (not concurrent by itself)
- Complexity increases with more cases

---

### 8. Timeout Pattern

**Pattern:**

```go
result := make(chan Result)
go func() {
    result <- slowOperation()
}()

select {
case r := <-result:
    return r
case <-time.After(5 * time.Second):
    return errors.New("timeout")
}
```

**Optimizes for:**

- Preventing indefinite blocking
- SLA enforcement
- Resource reclamation

**Use when:**

- Operations might hang
- Need to enforce time limits
- Preventing resource leaks

**Trade-offs:**

- Work continues after timeout (potential goroutine leak)
- Need cleanup mechanism

---

## Resource Optimization

### Token Optimization (for LLM APIs)

**The cost problem:**

- YouTube transcripts: 1,500-12,000 tokens per video
- 1,000 videos × 5,000 tokens average = 5M input tokens
- At $3/M tokens = $15 just for input

**Optimization strategies:**

#### 1. Preprocessing: Reduce before API call

```go
func optimizeTranscript(raw string) string {
    // Remove timestamps [00:01:23]
    cleaned := removeTimestamps(raw)
    
    // Remove filler words (um, uh, like)
    cleaned = removeFiller(cleaned)
    
    // Remove repetition
    cleaned = deduplicateSentences(cleaned)
    
    // Remove ads/sponsors
    cleaned = removeSponsors(cleaned)
    
    return cleaned
}
// Potential: 20-40% token reduction
```

#### 2. Chunking: Process long content in pieces

```go
func chunkTranscript(text string, maxTokens int) []string {
    chunks := splitIntoChunks(text, maxTokens)
    return chunks
}

// Process each chunk
summaries := make([]string, len(chunks))
for i, chunk := range chunks {
    summaries[i] = summarize(chunk)
}

// Combine chunk summaries
finalSummary := synthesize(summaries)
```

#### 3. Hierarchical summarization

```go
// Step 1: Extract key points (lossy compression)
keyPoints := extractKeyPoints(transcript)  // 12k → 2k tokens

// Step 2: Summarize key points
summary := summarize(keyPoints)  // 2k → 500 tokens

// Send 2k instead of 12k → 83% cost savings
```

#### 4. Prompt optimization

```go
// Bad: Verbose
prompt := `You are a helpful assistant. Please read the following 
transcript carefully and provide a comprehensive summary...`

// Good: Concise
prompt := `Summarize in 3 bullet points:`

// Saves ~50 tokens × 1,000 requests = 50k tokens
```

#### 5. Caching

```go
hash := sha256(transcript)

if cached := getCachedSummary(hash); cached != nil {
    return cached
}

summary := callAPI(transcript)
cacheSummary(hash, summary)
```

**Real-world impact:**

Without optimization:

- 1,000 videos × 8k tokens = 8M tokens
- Cost: $24

With optimization:

- Preprocessing: 8M → 5M
- Chunking: 5M → 3M
- Prompt optimization: -50k
- Final: ~3M tokens = $9
- **Savings: $15 (62% reduction)**

### Dynamic Resource Detection

**Runtime detection:**

```go
import "runtime"

func optimalWorkers(workType string) int {
    cores := runtime.NumCPU()
    
    switch workType {
    case "cpu-intensive":
        return cores
    case "io-intensive":
        return cores * 10
    case "mixed":
        return cores * 2
    default:
        return cores
    }
}
```

**Memory-aware bounds:**

```go
func safeWorkerCount(workSize int64) int {
    cores := runtime.NumCPU()
    idealWorkers := cores * 10
    
    availableRAM := getAvailableMemory()
    maxByMemory := availableRAM / workSize
    
    return min(idealWorkers, maxByMemory)
}
```

**Configuration with defaults:**

```go
type Config struct {
    MaxWorkers int  // 0 = auto-detect
}

func (c *Config) WorkerCount() int {
    if c.MaxWorkers > 0 {
        return c.MaxWorkers
    }
    return runtime.NumCPU() * 10  // Default for I/O
}
```

**Environment overrides:**

```bash
export MAX_WORKERS=50
export WORK_TYPE=io-intensive
```

```go
workers := getEnvInt("MAX_WORKERS", runtime.NumCPU() * 10)
```

---

## Pattern Selection Framework

### Decision Tree

**What's your primary constraint?**

| Constraint                          | Pattern        | Example                                            |
| ----------------------------------- | -------------- | -------------------------------------------------- |
| **Predictable memory usage**        | Worker Pool    | Database connection pool with max N connections    |
| **Simple stateless jobs**           | Semaphore      | Process independent files, max 20 concurrent       |
| **Different stage speeds**          | Pipeline       | Fetch (fast) → Process (slow) → Store (sequential) |
| **External rate limits**            | Rate Limiter   | API allows 50 requests/minute                      |
| **Error handling critical**         | Errgroup       | Batch job where one failure invalidates all        |
| **Multiple event sources**          | Select         | React to jobs, timeouts, cancellations             |
| **Same data, different processing** | Fan-Out/Fan-In | Process same input with multiple algorithms        |
| **Prevent indefinite wait**         | Timeout        | API calls must complete within 5 seconds           |

### Combining Patterns

Real systems often combine multiple patterns:

```go
// Pipeline with different concurrency per stage
func processVideos(urls []string) {
    // Stage 1: Fetch (I/O bound, semaphore)
    transcripts := make(chan Transcript, 100)
    go func() {
        sem := make(chan struct{}, 20)  // Max 20 concurrent fetches
        for _, url := range urls {
            sem <- struct{}{}
            go func(u string) {
                defer func() { <-sem }()
                t := fetchTranscript(u)
                transcripts <- t
            }(url)
        }
    }()
    
    // Stage 2: Process (rate limited, worker pool)
    summaries := make(chan Summary, 100)
    go func() {
        limiter := time.NewTicker(1200 * time.Millisecond)
        defer limiter.Stop()
        
        for t := range transcripts {
            <-limiter.C  // Rate limit
            go func(transcript Transcript) {
                s := summarize(transcript)
                summaries <- s
            }(t)
        }
    }()
    
    // Stage 3: Store (sequential)
    for s := range summaries {
        store(s)
    }
}
```

---

## Real-World Example: YouTube Transcript Summarizer

### Problem Analysis

**Requirements:**

- Process 1,000 YouTube videos
- Fetch transcripts
- Generate AI summaries
- Handle rate limits
- Optimize token usage

**Constraints:**

- YouTube API: Rate limited
- Transcript fetch: I/O bound (~300ms per video)
- AI summarization: Rate limited (50 requests/min)
- Cost: Token usage matters at scale

### Solution Architecture

**Three-stage pipeline with different concurrency:**

```
[Fetch Stage]           [Process Stage]        [Store Stage]
  20 workers     -->      5 workers      -->     1 worker
  I/O bound              Rate limited           Sequential
  Semaphore              Rate Limiter           Simple channel
```

### Implementation Sketch

```go
type TranscriptProcessor struct {
    fetchWorkers    int
    processWorkers  int
    rateLimit       time.Duration
}

func (tp *TranscriptProcessor) Process(urls []string) []Summary {
    // Stage 1: Fetch transcripts
    transcripts := tp.fetchStage(urls)
    
    // Stage 2: Generate summaries
    summaries := tp.processStage(transcripts)
    
    // Stage 3: Store results
    results := tp.storeStage(summaries)
    
    return results
}

func (tp *TranscriptProcessor) fetchStage(urls []string) <-chan Transcript {
    out := make(chan Transcript, 100)
    sem := make(chan struct{}, tp.fetchWorkers)
    
    go func() {
        defer close(out)
        var wg sync.WaitGroup
        
        for _, url := range urls {
            wg.Add(1)
            sem <- struct{}{}
            
            go func(u string) {
                defer wg.Done()
                defer func() { <-sem }()
                
                transcript := fetchTranscript(u)
                out <- transcript
            }(url)
        }
        
        wg.Wait()
    }()
    
    return out
}

func (tp *TranscriptProcessor) processStage(in <-chan Transcript) <-chan Summary {
    out := make(chan Summary, 100)
    limiter := time.NewTicker(tp.rateLimit)
    
    go func() {
        defer close(out)
        defer limiter.Stop()
        
        for transcript := range in {
            <-limiter.C  // Rate limit
            
            // Optimize tokens before API call
            optimized := optimizeTranscript(transcript.Text)
            
            summary := callAI(optimized)
            out <- summary
        }
    }()
    
    return out
}

func (tp *TranscriptProcessor) storeStage(in <-chan Summary) []Summary {
    var results []Summary
    
    for summary := range in {
        store(summary)
        results = append(results, summary)
    }
    
    return results
}
```

### Performance Characteristics

**Sequential approach:**

- Fetch: 1,000 × 300ms = 5 minutes
- Process: 1,000 ÷ 50/min = 20 minutes
- Total: 25 minutes

**Pipeline with concurrency:**

- Fetch: 1,000 ÷ 20 × 300ms = 15 seconds (starts immediately)
- Process: 1,000 ÷ 50/min = 20 minutes (overlaps with fetch)
- Total: ~20 minutes (25% improvement + better UX)

**Token optimization impact:**

- Without: 1,000 × 8k tokens = 8M tokens = $24
- With: 1,000 × 3k tokens = 3M tokens = $9
- Savings: $15 (62% reduction)

### Progressive Implementation

**V1: Basic fetch**

- Fetch 10 transcripts concurrently
- Print to console
- Learn semaphore pattern

**V2: Add summarization**

- Sequential summarization first
- Measure token usage
- Identify optimization opportunities

**V3: Concurrent summarization**

- Add rate limiter
- Test with small batch
- Verify API compliance

**V4: Token optimization**

- Add preprocessing
- Measure savings
- Tune for quality vs cost

**V5: Production features**

- Error handling and retries
- Progress tracking
- Resume capability
- Metrics and monitoring

---

## Key Takeaways

### Mental Models

1. **Cores are workers, not automatic parallelization**
    
    - Sequential code uses one core regardless of hardware
    - Concurrency requires explicit code (goroutines in Go)
2. **Worker count optimization depends on work type**
    
    - CPU-bound: workers ≈ cores
    - I/O-bound: workers >> cores
    - Measure and tune, don't guess
3. **Memory constrains concurrency**
    
    - Each worker consumes RAM
    - Bounded workers = bounded memory
    - Too many workers → swapping → slower than fewer
4. **Patterns solve specific constraints**
    
    - No universal "best" pattern
    - Choose based on bottleneck type
    - Combine patterns for complex systems
5. **Optimization requires measurement**
    
    - Profile to find actual bottlenecks
    - Tune based on data, not assumptions
    - Small optimizations compound at scale

### Anti-Patterns to Avoid

1. **Premature optimization**
    
    - Start simple (sequential or basic concurrency)
    - Measure before adding complexity
    - Optimize when you hit real limits
2. **Unbounded concurrency**
    
    - Don't spawn goroutine per item without limits
    - Will exhaust memory or overwhelm services
    - Always use semaphore or worker pool
3. **Ignoring resource constraints**
    
    - Consider CPU, RAM, network, API limits
    - Design for the actual bottleneck
    - Monitor resource usage in production
4. **Hardcoded worker counts**
    
    - Detect available resources at runtime
    - Allow configuration overrides
    - Adapt to different environments
5. **Missing error handling**
    
    - Concurrent errors harder to track
    - Need proper propagation and aggregation
    - Consider errgroup for coordinated cancellation

### What Tutorials Don't Teach

Most tutorials show mechanics (how to use channels, WaitGroups) but skip:

- **Why** worker counts matter
- **How** to decide the number for your workload
- **What** resource constraints affect performance
- **When** to use which pattern
- **How** real systems adapt to different environments

This gap between "how to write the code" and "how to design concurrent systems" is what this reference bridges.

---

## Additional Resources

**Go-specific:**

- `runtime.NumCPU()` - Detect available cores
- `golang.org/x/sync/errgroup` - Coordinated goroutines with error handling
- `golang.org/x/time/rate` - Rate limiting utilities

**Profiling and measurement:**

- `runtime.MemStats` - Memory usage statistics
- `pprof` - CPU and memory profiling
- `trace` - Execution tracing

**Common packages:**

- `sync.WaitGroup` - Wait for goroutine completion
- `context.Context` - Cancellation and timeouts
- `time.Ticker` - Rate limiting

**Design principles:**

- Start simple, add complexity when measured
- Choose patterns based on constraints
- Combine patterns for complex systems
- Always bound concurrency
- Measure, don't guess

---

_This reference was distilled from a conversation exploring concurrent systems design, hardware fundamentals, and practical optimization strategies. It represents a systematic framework for thinking about concurrency based on constraints and trade-offs rather than just coding patterns._