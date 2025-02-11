
A **Trie** (pronounced "try") is a type of search tree used in computer science to store and retrieve a dynamic set of strings, often for applications such as auto-complete, dictionary search, or IP routing.

 In Python, a trie is easily implemented as a nested tree of dictionaries where each key is a character that maps to the next character in a word.
 ```
{
	"h": {
		"e": {
			"l": {
				"l": {
					"o": {
						"*": True
					}
				},
				"p": {
					"*": True
				}
			}
		},
		"i": {
			"*": True
		}
	}
}
```

The term "trie" comes from "retrieval," and it is sometimes called a **prefix tree** because it is designed to handle prefixes efficiently.

### Key Characteristics of a Trie:

1. **Hierarchical Structure**: A trie is a tree where each node represents a single character of a string.
2. **Edge Relationships**: Edges between nodes represent characters. A path from the root to a node represents a prefix of one or more stored strings.
3. **Root Node**: The trie starts with a root node that does not contain any character.
4. **Leaf Nodes**: These represent the end of a complete string. They often include a marker or flag indicating the string's completion.
5. **Shared Prefixes**: Strings with common prefixes share the same initial part of the path, reducing redundancy.