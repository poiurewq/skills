---
name: save-conversation
description: "Saves the full conversation history from the current session as a markdown file in the working directory. Use when user wants to save, export, or dump the conversation. Trigger phrases include 'save conversation', 'export chat', 'save this session'."
---

# Save Conversation

Save the conversation history from the current session as a strategically summarized markdown file.

## Instructions

1. Generate a short kebab-case summary title (3-5 words) that captures the main topic of the conversation (e.g., `fix-auth-token-bug`, `add-dark-mode-toggle`). Do not include the word "conversation" in the title.

2. **Determine output location and filename:**
   - **If the user specified a directory**, use that directory (create it if it doesn't exist).
   - **Otherwise**, auto-detect in this order:
     1. Check if `conversation/` exists in the current working directory — if so, use it.
     2. Check if `conversation/` exists in the parent directory (`../`) — if so, use it.
     3. If neither exists, create `conversation/` in the current working directory and use it.
   - **Index selection — user-provided index always wins.** If the user specified an index (e.g., `112.003`, `47b`, `2026-04-13`), use it *verbatim* even if it doesn't match the default `NNN` zero-padded format. The user's index reflects a project-specific naming convention (dotted multi-level indices, date-based indices, etc.) that must be honored over this skill's default.
   - **Only if the user did not specify an index**, list the chosen directory's contents to find the highest existing numeric index (e.g., `002-older-topic.md` → next index is `003`). Start at `001` if the directory is empty. When inferring the next index from existing files, match the format already in use in that directory rather than forcing zero-padded `NNN`.
   - Name the file `<index>-<summary-title>.md` (hyphen separator, no "conversation" prefix). The default index format is zero-padded 3-digit `NNN`, but defer to user-supplied or directory-established formats as described above.
   - Write the file to the chosen directory.

3. Determine the current user's name for use in the file. For the agent model name: scan the conversation history (excluding this final save-conversation exchange) and identify which model was used for the majority of agent turns. Use that model as the recorded participant — not the model currently executing this skill. If the model varied, pick the one that handled the most turns.
4. Structure the markdown file as follows:
   - A top-level heading: `# <Summary Title>` (in natural Title Case, not kebab-case)
   - The next line: `Date: YYYY-MM-DD Participants: <user name>, <agent model>`
   - A horizontal rule (`---`)
   - Each exchange in the conversation as a section with:
     - `## <User Name> — <short turn summary>` or `## <Agent Name> (<model>) — <short turn summary>` heading — the turn summary should be 3-5 words capturing what this specific turn accomplished in the context of the overall conversation (e.g., `## Claude (claude-opus-4-6) — added frontmatter to agent file`); for agent turns, omit the model if unknown
     - The content of that message, applying the summarization rules below
     - A horizontal rule (`---`) between messages

## Summarization Rules

Apply these rules to keep the output readable and information-dense without losing meaning:

**User messages:** Reproduce verbatim — they are typically short and represent intent.

**Agent prose:** Reproduce verbatim — explanations, decisions, and reasoning should be preserved as written.

**Tool calls:** Do NOT dump raw tool call syntax. Instead, inline a one-line italicized summary of what was invoked and why, followed by the outcome:
> _Read `path/to/file.md` to inspect the existing skill format._

**Tool results:**
- **Short results** (< ~20 lines): include verbatim in a fenced code block.
- **Long results** (file listings, search output, command output): summarize as a bullet or sentence describing what was found/returned.
- **File contents that were generated or written**: instead of repeating the full content, note the file path and summarize what it contains (e.g., _"Wrote `agents/save-conversation.md` — a YAML-frontmattered agent spec with 8-step instruction set."_).
- **Errors or empty results**: include verbatim, as they are important context.

**Consecutive tool calls within a single agent turn:** Group them under a single `## <Agent Name>` section; do not create a new heading per tool call.

## After Writing

Confirm the output file path and approximate size (line count or KB) to the user.
