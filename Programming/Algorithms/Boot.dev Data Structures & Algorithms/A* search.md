# A* Search Algorithm

## What is it?

A* (A-star) search is like having a GPS with intuition - it's a pathfinding algorithm that finds the shortest path between two points, but unlike Dijkstra's algorithm, it uses a "heuristic" (educated guess) to head toward the goal more efficiently.

Think of it as exploring a maze where you have a compass pointing toward the exit. Instead of exploring in all directions equally, you prioritize paths that seem to lead closer to your destination.

## Key Properties

- **Optimal**: Finds the shortest path when using an admissible heuristic
- **Complete**: Will find a solution if one exists
- **Efficient**: Uses a heuristic to guide search toward the goal
- **Informed search**: Unlike BFS/DFS, it has knowledge about the goal location
- **Best-first search**: Always explores the most promising node first

## How A* Works

A* uses two functions:
- **g(n)**: Actual cost from start to node n
- **h(n)**: Heuristic estimate from node n to goal (must be admissible)
- **f(n) = g(n) + h(n)**: Total estimated cost of cheapest path through n

## Time/Space Complexity

- **Time Complexity**: O(b^d) where b = branching factor, d = depth
- **Space Complexity**: O(b^d) for storing the frontier
- **In practice**: Much faster than Dijkstra's due to heuristic guidance

## Implementation Example

```go
package main

import (
    "container/heap"
    "fmt"
    "math"
)

type Point struct {
    X, Y int
}

type Node struct {
    Point    Point
    G        float64 // Cost from start
    H        float64 // Heuristic to goal
    F        float64 // Total cost (G + H)
    Parent   *Node
    Index    int     // For heap interface
}

type PriorityQueue []*Node

func (pq PriorityQueue) Len() int { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool { return pq[i].F < pq[j].F }
func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
    pq[i].Index = i
    pq[j].Index = j
}

func (pq *PriorityQueue) Push(x interface{}) {
    n := len(*pq)
    node := x.(*Node)
    node.Index = n
    *pq = append(*pq, node)
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    node := old[n-1]
    node.Index = -1
    *pq = old[0 : n-1]
    return node
}

type Grid struct {
    Width, Height int
    Obstacles     map[Point]bool
}

func NewGrid(width, height int) *Grid {
    return &Grid{
        Width:     width,
        Height:    height,
        Obstacles: make(map[Point]bool),
    }
}

func (g *Grid) AddObstacle(p Point) {
    g.Obstacles[p] = true
}

func (g *Grid) IsValid(p Point) bool {
    return p.X >= 0 && p.X < g.Width && 
           p.Y >= 0 && p.Y < g.Height && 
           !g.Obstacles[p]
}

func (g *Grid) GetNeighbors(p Point) []Point {
    neighbors := []Point{}
    directions := []Point{{0, 1}, {1, 0}, {0, -1}, {-1, 0}} // 4-directional
    
    for _, dir := range directions {
        next := Point{p.X + dir.X, p.Y + dir.Y}
        if g.IsValid(next) {
            neighbors = append(neighbors, next)
        }
    }
    
    return neighbors
}

// Manhattan distance heuristic (admissible for 4-directional movement)
func manhattanDistance(a, b Point) float64 {
    return math.Abs(float64(a.X-b.X)) + math.Abs(float64(a.Y-b.Y))
}

// Euclidean distance heuristic (admissible for 8-directional movement)
func euclideanDistance(a, b Point) float64 {
    dx := float64(a.X - b.X)
    dy := float64(a.Y - b.Y)
    return math.Sqrt(dx*dx + dy*dy)
}

func AStar(grid *Grid, start, goal Point) []*Node {
    openSet := &PriorityQueue{}
    heap.Init(openSet)
    
    closedSet := make(map[Point]bool)
    allNodes := make(map[Point]*Node)
    
    // Create start node
    startNode := &Node{
        Point:  start,
        G:      0,
        H:      manhattanDistance(start, goal),
        Parent: nil,
    }
    startNode.F = startNode.G + startNode.H
    
    heap.Push(openSet, startNode)
    allNodes[start] = startNode
    
    for openSet.Len() > 0 {
        current := heap.Pop(openSet).(*Node)
        
        // Goal reached
        if current.Point == goal {
            return reconstructPath(current)
        }
        
        closedSet[current.Point] = true
        
        // Explore neighbors
        for _, neighbor := range grid.GetNeighbors(current.Point) {
            if closedSet[neighbor] {
                continue
            }
            
            tentativeG := current.G + 1 // Assuming uniform cost
            
            // Check if we've seen this neighbor before
            neighborNode, exists := allNodes[neighbor]
            if !exists {
                neighborNode = &Node{
                    Point: neighbor,
                    H:     manhattanDistance(neighbor, goal),
                }
                allNodes[neighbor] = neighborNode
            }
            
            // If this path to neighbor is better
            if !exists || tentativeG < neighborNode.G {
                neighborNode.Parent = current
                neighborNode.G = tentativeG
                neighborNode.F = neighborNode.G + neighborNode.H
                
                if !exists {
                    heap.Push(openSet, neighborNode)
                } else {
                    heap.Fix(openSet, neighborNode.Index)
                }
            }
        }
    }
    
    return nil // No path found
}

func reconstructPath(node *Node) []*Node {
    path := []*Node{}
    current := node
    
    for current != nil {
        path = append([]*Node{current}, path...)
        current = current.Parent
    }
    
    return path
}

func main() {
    // Create a 10x10 grid with some obstacles
    grid := NewGrid(10, 10)
    
    // Add some obstacles
    obstacles := []Point{
        {3, 3}, {3, 4}, {3, 5},
        {4, 3}, {5, 3}, {6, 3},
        {7, 6}, {7, 7}, {7, 8},
    }
    
    for _, obs := range obstacles {
        grid.AddObstacle(obs)
    }
    
    start := Point{0, 0}
    goal := Point{9, 9}
    
    path := AStar(grid, start, goal)
    
    if path == nil {
        fmt.Println("No path found!")
        return
    }
    
    fmt.Printf("Path found with %d steps:\n", len(path))
    for i, node := range path {
        fmt.Printf("Step %d: (%d, %d) - F: %.1f\n", 
                  i, node.Point.X, node.Point.Y, node.F)
    }
}
```

## When to Use A* Search

- **Video game pathfinding**: NPCs navigating around obstacles
- **Route planning**: GPS navigation systems
- **Robotics**: Robot navigation and path planning
- **Puzzle solving**: 8-puzzle, 15-puzzle, etc.
- **Network routing**: Finding optimal paths in networks
- **Real-time systems**: When you need fast pathfinding

## Heuristic Functions

The heuristic function h(n) is crucial for A* performance:

### Requirements:
- **Admissible**: Never overestimate the true cost
- **Consistent**: h(n) ≤ cost(n, n') + h(n') for all neighbors n'

### Common Heuristics:

**Manhattan Distance (L1):**
```go
func manhattanDistance(a, b Point) float64 {
    return math.Abs(float64(a.X-b.X)) + math.Abs(float64(a.Y-b.Y))
}
// Use for: 4-directional movement (grid-based games)
```

**Euclidean Distance (L2):**
```go
func euclideanDistance(a, b Point) float64 {
    dx := float64(a.X - b.X)
    dy := float64(a.Y - b.Y)
    return math.Sqrt(dx*dx + dy*dy)
}
// Use for: Any-direction movement (direct line possible)
```

**Chebyshev Distance (L∞):**
```go
func chebyshevDistance(a, b Point) float64 {
    dx := math.Abs(float64(a.X - b.X))
    dy := math.Abs(float64(a.Y - b.Y))
    return math.Max(dx, dy)
}
// Use for: 8-directional movement (chess-like)
```

## A* vs Other Algorithms

| Algorithm | Optimality | Completeness | Time | Space | Notes |
|-----------|------------|--------------|------|-------|-------|
| **A*** | Yes* | Yes | O(b^d) | O(b^d) | *with admissible heuristic |
| **Dijkstra's** | Yes | Yes | O(V²) | O(V) | Uniform cost, no heuristic |
| **BFS** | Yes** | Yes | O(V+E) | O(V) | **for unweighted graphs |
| **DFS** | No | Yes*** | O(V+E) | O(V) | ***for finite graphs |
| **Greedy Best-First** | No | Yes | O(b^d) | O(b^d) | Fast but not optimal |

## Common Pitfalls

- **Non-admissible heuristic**: Can lead to suboptimal paths
- **Inconsistent heuristic**: May cause A* to behave unpredictably
- **Memory usage**: Can consume lots of memory for large search spaces
- **Tie-breaking**: Equal f-values need consistent tie-breaking rules
- **Dynamic obstacles**: Standard A* doesn't handle moving obstacles

## Advanced Optimizations

### Bidirectional A*
Search from both start and goal simultaneously:
```go
// Pseudo-code concept
func BidirectionalAStar(start, goal Point) []*Node {
    // Run A* from start toward goal
    // Run A* from goal toward start  
    // Stop when searches meet
    // Combine paths
}
```

### Hierarchical A*
For very large maps, use hierarchical decomposition:
- Divide map into regions
- Find path between regions first
- Then find detailed path within regions

## Visual Example

```
Grid (S=start, G=goal, X=obstacle, *=path):
S . . X . . . . . G
* . . X . . . . . .
* . . X . . . . . .
* * * . . . . . . .
. . * . . . . . . .
. . * . . . X X X .
. . * . . . X . . .
. . * * * * * . . .
. . . . . . * . . .
. . . . . . * * * G
```

## Problem-Solving with A*

1. **Define your state space**: What represents a node?
2. **Choose appropriate heuristic**: Must be admissible
3. **Implement cost function**: How expensive is each move?
4. **Handle edge cases**: No path, obstacles, boundaries
5. **Optimize for your use case**: Memory vs speed trade-offs

## Related Concepts

- **[[Dijkstra's Algorithm]]**: A* without heuristic
- **[[Breadth first search (BFS)]]**: Unweighted shortest path
- **[[Priority Queues & Heaps]]**: Data structure used in A*
- **[[Graphs]]**: The underlying data structure A* operates on