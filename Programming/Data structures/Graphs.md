A [graph](https://en.wikipedia.org/wiki/Graph_%28discrete_mathematics%29) is a set of vertices and the edges that connect those vertices. _All trees are graphs, but not all graphs are trees._

![[Pasted image 20241224214721.png|400]]
A matrix to represent the edges in a graph that connect each pair of vertices. For example, here's a matrix that represents the graph above.

![[Skärmavbild 2024-12-24 kl. 21.48.01.png|500]]

In Python, we can use a list of lists to represent this matrix:

```python
[
  [False, True, False, False, True],
  [True, False, True, True, True],
  [False, True, False, True, False],
  [False, True, True, False, True],
  [True, True, False, True, False]
]
```

