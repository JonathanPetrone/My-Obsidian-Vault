## Core Concept/Principle: 
Build explicit feedback mechanisms into AI interactions to prevent hallucinations and create improvement loops. Always provide "escape hatches" where AI can report uncertainty or confusion rather than fabricating answers.

## Context/Example: 
Include parameters like: "If you lack sufficient information, respond with 'INSUFFICIENT_DATA: [specific gap]' rather than guessing" or "Report any confusing instructions in a DEBUG section."

## Reference: 
https://www.youtube.com/watch?v=DL82mGde6wo&ab_channel=YCombinator
Claude conversation on AI prompting best practices, 2025-06-07

## Connections:
- **Similar**: [[Error Handling in Code]], [[Continuous Integration Feedback]]
- **Opposite**: [[Assume AI Omniscience]]
- **Builds on**: [[AI Prompting as Code Architecture]]
- **Enables**: [[Reliable AI Systems]], [[Prompt Quality Improvement]]

---

Created: 2025-06-07 Tags: #systems-thinking #ai-reliability #feedback-loops