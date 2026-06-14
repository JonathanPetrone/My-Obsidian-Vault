[[Creating Handoff-Ready Systems]]
____
TIPS on sources

- **Reliability Engineering / Site Reliability Engineering (SRE)**
    - **Why:** They explicitly design for operations by people who aren't the original builders
    - **Key concept:** Error budgets, runbooks, toil reduction
    - **Resource:** Google's "Site Reliability Engineering" book (free online) - specifically chapters on "Eliminating Toil" and "Simplicity"
    - **Warning:** Heavy on technical systems, but the principles transfer to organizational processes
- **Standard Work / Toyota Production System**
    - **Why:** Entire methodology built around making expertise transferable through standardization
    - **Key concept:** Standardized work is a baseline for improvement, not a straitjacket
    - **Resource:** "Toyota Kata" by Mike Rother - focuses on teaching the _improvement process_ not just the current standard
    - **Relevant bit:** Distinction between "standard work" (what) and "improvement kata" (how to evolve it)
- **Cognitive Task Analysis / Knowledge Elicitation**
    - **Why:** Techniques for extracting expert decision-making and making it explicit
    - **Key concept:** Critical decision method - interviewing yourself about actual incidents to surface hidden judgment criteria
    - **Resource:** "Working Minds" by Beth Crandall et al. - practical methods for knowledge capture from experts
    - **Application:** How to document the decision trees you actually use vs. what you think you use
- **Cynefin Framework (Dave Snowden)**
    - **Why:** Directly addresses when to use procedures vs. judgment vs. experimentation
    - **Key concept:** Complicated domains = good practice, Complex domains = emergent practice
    - **Resource:** Snowden's talks/articles on "managing complexity" - helps you identify which parts of your system need rigidity vs. flexibility
    - **Connection to your framework:** Helps distinguish which aspects of CAB are complicated (procedural) vs complex (requires judgment)
- **Decision Forcing Cases (Military)**
    - **Why:** Method for training judgment by presenting decision points without revealing outcomes
    - **Key concept:** Build decision-making muscle through structured case analysis
    - **Resource:** Look for "tactical decision games" or "decision forcing cases" - Marine Corps Gazette has archives
    - **Application:** How to train successors on the _thinking_ not just the steps
- **Checklists / Error Prevention**
    - **Why:** When procedures work vs. when they create brittleness
    - **Resource:** "The Checklist Manifesto" by Atul Gawande
    - **Key distinction:** Communication checklists (coordination) vs. discipline checklists (don't forget steps)
    - **Warning:** Oversimplifies, but useful for understanding when codification helps vs. hinders

**Exploration strategy:**

Start with **Toyota Kata** (most accessible, organizational focus) and **SRE chapters on Toil/Simplicity** (concrete examples). Those will give you contrasting perspectives - Toyota is about human-centered standardization, SRE is about automation-first with human fallback.

Then based on which resonates more, either:

- Go deeper on knowledge elicitation (Working Minds) if the problem feels like "I can't articulate my decision-making"
- Go deeper on Cynefin if the problem feels like "I don't know which parts should be rigid vs. flexible"

Worth searching: "knowledge transfer in organizations," "succession planning for process owners," "institutionalizing practices."

%% automation vs human %%
Automation vs. Human process
- **Automation:** "I built it in a way only I can maintain" → Need better code architecture, documentation
- **Human process:** "Only I have the judgment to run it" → Need to externalize decision-making criteria