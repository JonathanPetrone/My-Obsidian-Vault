Dijkstra's Algorithm is used to find the shortest path between two vertices in a weighted graph. Thus it is a weighted graph algorithm.

Dijkstra's Algorithm starts at a source vertex and traverses the graph to find the shortest path between the source and a destination.

- The algorithm keeps track of the "shortest distance so far" while it travels and it updates it each time it finds a shorter path. (This requires the algorithm to store this information)
- Once all of a node's (aka vertex) neighbors have been checked and their distances have been updated, that node is marked as visited.
- At each step, the algorithm moves to the nearest unvisited neighbor with the shortest known distance. This process repeats until the shortest path to the destination is determined.

![[Skärmavbild 2025-04-08 kl. 11.09.44.png]]

**Dijkstra’s algorithm** is a greedy algorithm. It finds the shortest path in a graph by always picking the closest unvisited node and checking if going through that node gives shorter paths to others.

This works **as long as all the edges (connections) have non-negative weights** (no negative costs). In this case, once it finds the shortest path to a node, we know it won’t find a better one later. So being greedy is safe. If there are negative weights, you'll need to use something different like Bellman-Ford.