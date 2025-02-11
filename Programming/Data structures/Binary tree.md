[Trees](https://en.wikipedia.org/wiki/Tree_(abstract_data_type)) are a widely used data structure that simulate a hierarchical... well... _tree_ structure. That said, they're typically drawn upside down - the "root" node is at the top, and the "leaves" are at the bottom.

Trees are kind of like linked lists in the sense that the root node simply holds references to its child nodes, which in turn hold references to their children. The difference between a Linked List and a Tree is that a tree's nodes can have _multiple_ children instead of just one.

A generic tree structure has the following rules:
- Each node has a value and a list of "children"
- Children can only have a single "parent"

Trees aren't particularly useful data structures unless they're ordered in some way. One of the most common types of ordered tree is a [Binary Search Tree](https://en.wikipedia.org/wiki/Binary_search_tree) or `BST`. In addition to the properties we've already talked about, a `BST` has some additional constraints:

1. Instead of an unbounded list of children, each node has at most 2 children
2. The left child's value must be less than its parent's value
3. The right child's value must be greater than its parent's value
4. No two nodes in the `BST` can have the same value

By ordering the tree this way, we'll be able to _add_, _remove_, _find_, and _update_ nodes much more quickly.

#### Inserts

Inserting into a binary tree (like most of its operations) is _very_ fast.
![[Pasted image 20241223180817.png|600]]
### Traversals
- Preorder traversal
- Postorder traversal
- Inorder traversal

### BST problem
BST's have a problem. While it's true that on average a `BST` has `O(log(n))` lookups, deletions, and insertions, that fundamental benefit can break down quickly.

If mostly sorted data, or even worse, _completely sorted_ data, is inserted into a binary tree, the tree will be much deeper than it is wide. As you know by now, the Big O complexity of the tree's operations depend entirely on the _depth_ of the tree.

**Unbalanced tree**
![[Pasted image 20241223181255.png|500]]
**Balanced tree**
![[Pasted image 20241223181309.png|500]]

A [[Red black tree]] tree is a _kind_ of binary search tree that solves the "balancing" problem. I