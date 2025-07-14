# Breadth First Search (BFS)

## What is it?

Breadth First Search is like exploring a maze by checking all rooms at your current distance before moving further away. It's a graph traversal algorithm that visits nodes level by level, starting from a source node.

Think of it like ripples in a pond - you explore all nodes at distance 1, then all nodes at distance 2, and so on.

## Key Properties

- **Level-by-level exploration**: Visits all nodes at distance `k` before visiting any node at distance `k+1`
- **Uses a queue**: FIFO (First In, First Out) data structure to maintain order
- **Guarantees shortest path**: In unweighted graphs, BFS finds the shortest path to any reachable node
- **Complete**: Will find a solution if one exists
- **Optimal**: For unweighted graphs, always finds the shortest path

## Time/Space Complexity

- **Time Complexity**: O(V + E) where V = vertices, E = edges
- **Space Complexity**: O(V) for the queue and visited set

## Implementation Example

```go
package main

import (
    "fmt"
    "container/list"
)

type Graph struct {
    vertices int
    adjList  [][]int
}

func NewGraph(vertices int) *Graph {
    return &Graph{
        vertices: vertices,
        adjList:  make([][]int, vertices),
    }
}

func (g *Graph) AddEdge(src, dest int) {
    g.adjList[src] = append(g.adjList[src], dest)
    g.adjList[dest] = append(g.adjList[dest], src) // For undirected graph
}

func (g *Graph) BFS(startVertex int) []int {
    visited := make([]bool, g.vertices)
    queue := list.New()
    result := []int{}
    
    // Mark starting vertex as visited and enqueue it
    visited[startVertex] = true
    queue.PushBack(startVertex)
    
    for queue.Len() > 0 {
        // Dequeue a vertex
        vertex := queue.Remove(queue.Front()).(int)
        result = append(result, vertex)
        
        // Visit all adjacent vertices
        for _, neighbor := range g.adjList[vertex] {
            if !visited[neighbor] {
                visited[neighbor] = true
                queue.PushBack(neighbor)
            }
        }
    }
    
    return result
}

// Find shortest path between two nodes
func (g *Graph) ShortestPath(start, end int) []int {
    if start == end {
        return []int{start}
    }
    
    visited := make([]bool, g.vertices)
    parent := make([]int, g.vertices)
    queue := list.New()
    
    // Initialize parent array
    for i := range parent {
        parent[i] = -1
    }
    
    visited[start] = true
    queue.PushBack(start)
    
    for queue.Len() > 0 {
        vertex := queue.Remove(queue.Front()).(int)
        
        for _, neighbor := range g.adjList[vertex] {
            if !visited[neighbor] {
                visited[neighbor] = true
                parent[neighbor] = vertex
                queue.PushBack(neighbor)
                
                // If we reached the destination, reconstruct path
                if neighbor == end {
                    path := []int{}
                    current := end
                    for current != -1 {
                        path = append([]int{current}, path...)
                        current = parent[current]
                    }
                    return path
                }
            }
        }
    }
    
    return nil // No path found
}

func main() {
    // Example usage
    g := NewGraph(6)
    g.AddEdge(0, 1)
    g.AddEdge(0, 2)
    g.AddEdge(1, 3)
    g.AddEdge(1, 4)
    g.AddEdge(2, 5)
    
    fmt.Println("BFS traversal starting from vertex 0:")
    result := g.BFS(0)
    fmt.Println(result) // Output: [0 1 2 3 4 5]
    
    fmt.Println("Shortest path from 0 to 5:")
    path := g.ShortestPath(0, 5)
    fmt.Println(path) // Output: [0 2 5]
}
```

## When to Use

- **Finding shortest path** in unweighted graphs
- **Level-order traversal** of trees
- **Testing if graph is connected** (check if all nodes are reachable)
- **Finding all nodes at distance k** from a source
- **Web crawling** (explore pages level by level)
- **Social network analysis** (find degrees of separation)
- **Puzzle solving** (like finding minimum moves in a game)

## Common Pitfalls

- **Forgetting to mark nodes as visited**: This causes infinite loops
- **Not using a queue**: Using a stack instead turns it into DFS
- **Modifying the graph during traversal**: Can lead to inconsistent results
- **Not handling disconnected graphs**: BFS only visits connected components
- **Memory issues with large graphs**: The queue can grow very large

## BFS vs DFS Quick Comparison

| Aspect | BFS | DFS |
|--------|-----|-----|
| Data Structure | Queue | Stack |
| Memory Usage | Higher (stores more nodes) | Lower |
| Path Found | Shortest path | Any path |
| Use Case | Shortest path, level-order | Topological sort, cycle detection |

## Visual Example

```
Graph:     BFS Order (starting from 0):
    0      Step 1: Visit 0, add neighbors 1,2 to queue
   / \     Step 2: Visit 1, add neighbors 3,4 to queue  
  1   2    Step 3: Visit 2, add neighbor 5 to queue
 /|   |   Step 4: Visit 3 (no new neighbors)
3 4   5   Step 5: Visit 4 (no new neighbors)
          Step 6: Visit 5 (no new neighbors)
          
Result: [0, 1, 2, 3, 4, 5]
```

## Related Concepts

- **[[Depth first search]]**: Alternative graph traversal method
- **[[Queues]]**: The data structure that makes BFS possible
- **[[Graphs]]**: The data structure BFS operates on
- **[[Dijkstra's Algorithm]]**: Weighted graph shortest path (uses BFS principles)