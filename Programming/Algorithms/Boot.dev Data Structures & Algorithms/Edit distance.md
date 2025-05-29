**Edit distance** (also known as **Levenshtein distance**) is a way to measure **how different two strings are**, by counting the **minimum number of operations** required to transform one string into the other.

---
### 🛠 Allowed Operations:

1. **Insert** a character
2. **Delete** a character
3. **Substitute** (replace) one character with another


> Use **edit distance** anytime you care about how **similar two strings are**, especially when **errors, typos, or mutations** are common.

### 📝 **Spell Checking / Auto-correct**

- Suggest corrections by finding dictionary words with the **lowest edit distance** to the misspelled word.

> Typo: `"exampel"` → Suggest `"example"` (edit distance = 2)


Each operation typically costs 1.
![[Skärmavbild 2025-04-19 kl. 21.44.20.png||400]]