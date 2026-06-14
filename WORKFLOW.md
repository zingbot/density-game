# Density — Claude.ai + Claude Code Workflow Guide

## The Two-Environment Split

| | Claude.ai (this project) | Claude Code |
|---|---|---|
| **Use for** | Design, prototyping, playtesting, discussions | Code changes, git, deployment |
| **Strengths** | Live game widget, Google Drive access, memory, interactive feedback | File editing, git push/pull, multi-file refactors, terminal |
| **Weaknesses** | Can't push to git, widget code isn't directly in the repo | No live game preview, no Google Drive MCP |
| **Context source** | Memory + Google Doc | CLAUDE.md + Google Doc |

## The Golden Rule

**One source of truth per concern:**
- **Game rules & project management** → Google Doc
- **Code** → GitHub repo
- **Never let them drift.** Every change in either environment triggers a sync.

---

## Best Practices

### Starting a Session

**In Claude.ai:**
Just start talking. Memory and the Google Doc connection mean Claude already has context. For a fresh session after a long break, say: *"Read the project tracker and catch up on where we left off."*

**In Claude Code:**
Claude Code reads CLAUDE.md automatically. For extra context, start with:
```
Read the Google Doc project tracker and tell me the current status before we start.
```
(Claude Code can fetch URLs, so it can read the doc directly.)

### Design Work (Claude.ai)

1. Discuss the mechanic or change you want to make
2. Prototype it with show_widget — play it right in the chat
3. Iterate until it feels right
4. Once finalized, say: **"Update the doc and prep this for Code."**
5. Claude.ai will:
   - Update the Google Doc (rules, decisions, tasks, version)
   - Give you the exact code changes to apply in Claude Code
   - Or give you a clean updated index.html to copy over

### Code Work (Claude Code)

1. Pull latest from GitHub: `git pull`
2. Apply the changes (paste from Claude.ai, or describe what to change)
3. Test in browser: `open index.html`
4. Commit and push:
   ```
   git add -A
   git commit -m "v0.7: Description of change"
   git push
   ```
5. Tell Claude Code: **"Update the Google Doc with what we just shipped."**
   (If Claude Code can't access the doc, note the changes and update it in your next Claude.ai session.)

### Syncing Between Environments

The Google Doc is the bridge. Here's how to keep everything tight:

**After a Claude.ai design session:**
- Claude.ai updates the Google Doc automatically (rules, decisions, version)
- Copy the finalized code to your local repo
- In Claude Code: apply, test, commit, push

**After a Claude Code shipping session:**
- Claude Code updates CHANGELOG.md in the repo
- Next Claude.ai session: *"Read the latest CHANGELOG.md from the repo and sync the Google Doc."*
- Or update the Google Doc manually / have Claude Code do it if it can access the URL

**Weekly sync check:**
Ask either Claude: *"Audit the Google Doc against the code. Are they in sync?"*

### Handoff Phrases

Use these to make transitions clean:

| Phrase | What it triggers |
|--------|-----------------|
| "Update the doc" | Claude updates the Google Doc with all changes from this session |
| "Prep this for Code" | Claude outputs the exact file changes needed, ready to paste into Claude Code |
| "Catch me up" | Claude reads the Google Doc and summarizes current status |
| "Audit docs vs code" | Claude compares documented rules/methods against actual code and flags mismatches |
| "Ship it" | (In Claude Code) Commit, push, update CHANGELOG |

### What NOT to Do

- **Don't make rule changes in Claude Code without updating the Google Doc.** Code changes are invisible to Claude.ai unless the doc reflects them.
- **Don't prototype in Claude Code.** The feedback loop is slower — no live game widget. Design in Claude.ai, ship in Claude Code.
- **Don't keep changes in your head.** If you decided something in a conversation, it needs to be in the Google Doc or it'll be lost next session.
- **Don't skip the audit.** After a few sessions, docs and code will drift if you're not checking. A 30-second audit saves hours of confusion.

---

## Example Workflow: Adding a New Tile Type

### Step 1: Design (Claude.ai)
> "I want to add a Road tile. Roads should connect buildings and give +1 density to all buildings along a connected road network."

Claude.ai discusses tradeoffs, prototypes it in show_widget, you playtest it, iterate.

### Step 2: Finalize (Claude.ai)
> "This feels good. Update the doc and prep for Code."

Claude.ai updates the Google Doc with:
- New rule in Game Rules section
- Decision in Decision Log
- New task in Task Board (marked complete)
- Version bump in Version History
- Code diff or full updated index.html

### Step 3: Ship (Claude Code)
```
git pull
# Apply the code changes
# Test in browser
git add -A
git commit -m "v0.8: Add Road tile type — +1 density along connected network"
git push
```

### Step 4: Verify (either environment)
> "Audit the doc against the code for v0.8."

---

## For Adrian (or Any New Collaborator)

If you're joining this project:

1. **Read the Google Doc** — it has the full rules, all decisions made so far, and the roadmap
2. **Clone the repo** — `git clone` and open `index.html` to play the current version
3. **Set up your own Claude.ai Project** — upload or link the Google Doc so Claude has context
4. **Start designing** — propose changes in Claude.ai, prototype with the widget, then ship via Claude Code
5. **Always update the Google Doc** after making changes, or ask Claude to do it
