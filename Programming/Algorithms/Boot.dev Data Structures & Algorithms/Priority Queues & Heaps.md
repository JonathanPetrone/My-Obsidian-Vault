A priority queue is a data structure similar to a regular queue but where each element additionally has a _priority_ associated with it. In our priority queue, an element with low priority is served before an element with high priority, though this can go either way.

In some implementations, if two elements have the same priority, they are served according to the order in which they were enqueued, while in other implementations, the ordering of elements with the same priority is undefined.

Priority queues _can_ have `insert` and `pop` methods that are `Big O(log(n))`. These kinds of fast priority queues can speed up algorithms like Dijkstra's.

## What do we use heaps and priority queues for?

Heaps and priority queues show up everywhere you need to always grab “the next most important” item quickly. Here are the most common places you’ll see them:

---
## 1. Algorithmic Building Blocks
- **Shortest‑path & graph algorithms**
    - **Dijkstra’s algorithm** uses a min‑heap to always explore the closest unvisited node next.
    - **A*** (and other best‑first searches) use a priority queue to pick the node with the lowest estimated total cost.
- **Minimum spanning trees**
    - **Prim’s algorithm** picks the cheapest edge that connects the growing tree to a new vertex. A min‑heap of edges makes that O(log E) per edge.
- **Huffman coding (compression)**
    - Repeatedly merge the two least‑frequent symbols. A min‑heap of symbol‑frequencies gives you the two smallest in O(log n) each time.
- **K‑way merge (e.g., merging sorted files)**
    - If you have K sorted lists and want to merge them into one, push each list’s current head into a min‑heap. Pop the smallest, advance in that list, and push again—O(n log K) overall.

---
## 2. Real‑World & Systems Use
- **Task scheduling**
    - Operating systems often maintain a priority queue of “ready” processes or threads, picking the highest‑priority one next.
    - Print servers, job queues, or any work‑dispatcher will similarly use a PQ.
- **Event‑driven simulation**
    - In discrete‑event simulators (network, traffic, games), future events (with timestamps) live in a min‑heap so the next‑soonest event is always dequeued efficiently.
- **Timers & timeouts**
    - Frameworks that schedule timeouts or alarms keep events keyed by their expiration time—again, a min‑heap over timestamps.

---

## 3. Data‑stream & Online Problems

- **Running median**
    - Maintain two heaps (a max‑heap for the lower half, a min‑heap for the upper half) so you can insert numbers and get the median in O(log n).
- **Top‑K queries**
    - If you want the K largest (or smallest) items from a stream, keep a size‑K heap: push new items and pop when size>K. At the end, the heap holds the top K.

---

## 4. Why They’re So Popular

- **Speed**: Insert and remove‑min (or remove‑max) both cost O(log n), peek is O(1).
- **Simplicity**: Very easy to implement on top of an array.
- **Flexibility**: You can wrap a heap in a clean “priority queue” API and treat the details as an internal implementation.

---

**Bottom line:**  
Wherever you need to repeatedly pick—and then remove—“the best” (smallest, largest, soonest, most urgent, etc.) from a changing collection, a heap‑backed priority queue is your go‑to tool.