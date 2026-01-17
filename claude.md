# Claude Instructions for this Vault

## Important: Vault Location
- **ALWAYS work in the main vault location:** `/Users/jonathanpetrone/Library/Mobile Documents/iCloud~md~obsidian/Documents`
- This is NOT a worktree - it's the main repository that syncs with Obsidian
- Changes made here appear immediately in Obsidian
- User uses git commits as backup/versioning, not for collaboration workflow

## General Guidelines
- **Do NOT explore the vault** unless explicitly directed to a specific folder
- You may look at the overall structure when needed
- The user will be very explicit with instructions
- Ask clarifying questions before diving into work
- Some parts of the vault will be more or less developed
- Avoid unnecessary token usage

## Vault Structure (for efficient navigation)

### Safe to Explore Freely:
- **Thought Collection/** - Contains all reading notes and extracted knowledge
  - `Books (Single source and note)/[Book Name]/` - Individual concepts from books
  - `Multi-Sourced Notes/` - Synthesized concepts from multiple sources
  - `Definitions/` - Term definitions
  - `Todos and Drafts/Raw Source Extracts/` - Raw extracts from books (marked with comments)
  - When book notes exist, check both the extracted note AND the raw source extract for fuller context

### Files You're Working On:
- **Jonathan Inc./Meta-Patterns/** - Meta-level patterns and frameworks being developed
  - Primary file: `Building Adaptive Expertise.md`

### Finding Linked Files:
- Obsidian links look like `[[File Name]]`
- Most linked files are in `Thought Collection/Books (Single source and note)/[Book Name]/`
- Use `find "./Thought Collection" -type f -name "*keyword*"` to locate files efficiently

## Working Protocol
1. Wait for explicit instructions about which folder/file to work on
2. Ask clarifying questions to ensure understanding
3. User may point to resources and ask if something is applicable
4. Only proceed after getting clear direction

## Default Mode: Analyze & Recommend (NOT Write)
- Unless explicitly asked to write content, provide analysis and recommendations
- Point to sources in the vault that support claims
- Identify gaps where external resources may be needed
- Let the user decide when to draft content

## How to Provide Suggestions

When working on files in Meta-Patterns or similar development areas:

1. **Add suggestions at the bottom of the file** under a header: `##### Claude Suggestions:`
2. **Structure each suggestion with:**
   - **What to add** - Specific concept or strategy
   - **Why** - The reasoning or benefit
   - **Source** - Exact file path and line numbers from the vault (or note if external resource needed)
   - **Key insight/evidence** - Direct quote or summary from source material
   - **Application** - How it applies to the specific context

3. **Organize suggestions into:**
   - Main content recommendations (numbered, with full traceability)
   - Gaps requiring external resources (what to look for)
   - Supporting material already in vault (file paths for reference)

4. **All claims must be traceable:**
   - Prefer vault sources with specific line numbers
   - If suggesting external resources, explain why they're needed and what gap they fill
   - Help synthesize and connect existing vault material rather than introducing unsourced ideas

**Example format:**
```
##### Claude Suggestions:

**For: [Section Name]**

**What to add & Why:**

1. **[Concept Name]**
   - **Why:** [Reason this matters]
   - **Source:** `path/to/file.md` (lines X-Y)
   - **Key insight:** [Quote or summary]
   - **Application:** [How to use it]

**Gaps requiring external resources:**
1. **[Topic]**
   - [What to look for and why]

**Supporting material already in vault:**
- `path/to/relevant/file.md` - [What it contains]
```
