## Core Concept/Principle:
A software testing oracle is a tool that tells you whether the program passed your test. Oracles are inherently heuristic (useful but not always correct) and incomplete because we cannot fully specify the postcondition state of the system under test. This creates two fundamental risks: believing the program passed when it failed, or believing it failed when it behaved appropriately.

## Context/Example:
Cem Kaner's definition highlights that no oracle can be "complete" in itself. Testing oracles face the fundamental challenge of incompleteness - we cannot specify every possible postcondition or expected behavior. This creates uncertainty in test results where we may get false positives (thinking something passed when it didn't) or false negatives (thinking something failed when it worked correctly).

## Reference:
- https://kaner.com/?p=190

## Connections:
- **Similar**: [[Test coverage analysis problems]], [[Heuristic decision-making]], [[Incomplete information systems]], [[Specification uncertainty]]
- **Opposite**: [[Binary pass/fail conditions]], [[Fully specified test requirements]], [[Complete verification]], [[Deterministic validation]]
- **Builds on**: [[Heuristic approaches]], [[Testing theory]], [[System specification challenges]]
- **Enables**: [[Better testing strategies]], [[Multi-oracle approaches]], [[Sophisticated test validation]]


---

_Created: 2025-05-31_ _Tags: #systems-thinking #testing