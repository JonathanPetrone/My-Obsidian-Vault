## Core Concept/Principle: 
Build explicit feedback mechanisms into AI interactions to prevent hallucinations and create improvement loops. Always provide "escape hatches" where AI can report uncertainty or confusion rather than fabricating answers.

## Context/Example: 
Include parameters like: "If you lack sufficient information, respond with 'INSUFFICIENT_DATA: [specific gap]' rather than guessing" or "Report any confusing instructions in a DEBUG section."

## System Components:
- **Individual Elements**: Uncertainty declarations, debug parameters, complaint mechanisms, meta-prompting loops
- **Interactions**: Feedback creates prompt improvement cycles; escape hatches prevent error propagation
- **Environment/Field**: Requires iterative development mindset and tolerance for initial prompt failures

## Reference: 
https://www.youtube.com/watch?v=DL82mGde6wo&ab_channel=YCombinator
Claude conversation on AI prompting best practices, 2025-06-07

## Connections:
- **Similar**: [[Error Handling in Code]], [[Continuous Integration Feedback]]
- **Opposite**: [[Assume AI Omniscience]]
- **Builds on**: [[AI Prompting as Code Architecture]]
- **Enables**: [[Reliable AI Systems]], [[Prompt Quality Improvement]]

## Applications:
- Test automation where false positives are costly
- Requirements gathering where precision matters
- System design validation where assumptions need explicit checking
- Documentation review where gaps need identification

## Questions/Next Steps:
- [ ]  Design standard escape hatch templates for different testing scenarios
- [ ]  Create feedback collection system for prompt performance

---

Created: 2025-06-07 Tags: #systems-thinking #ai-reliability #feedback-loops