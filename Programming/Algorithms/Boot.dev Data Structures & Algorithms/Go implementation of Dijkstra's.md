```
type Graph map[string]map[string]int
```

This defines a **map of nodes**, where each node points to a map of **its neighbors** with the cost to reach them.

Graph{
  "Minas Tirith": {
    "Gondor": 1,
    "Isengard": 4,
  },
  "Gondor": {
    "Minas Tirith": 1,
    "Bree": 2,
    "Misty Mountains": 6,
  },
  ...
}


