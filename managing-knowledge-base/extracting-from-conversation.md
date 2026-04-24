# Extracting from Conversation — Workflow

Mode-specific steps for extracting knowledge from the current conversation.
Shared rules (frontmatter, conciseness, naming, folders, quality checklist) are in SKILL.md.

## Constraints

- ONLY analyze conversation history visible in the current session
- DO NOT use WebFetch, WebSearch, or external knowledge sources
- DO NOT supplement with information outside the conversation
- If concepts mentioned but not explained in conversation, note as "mentioned but not detailed"

## Phase 1: Scope & Analysis

1. **Determine topic scope:**
   - User specifies topics → extract ONLY those (be generous: "Docker stuff" includes closely related containerization concepts)
   - No topics specified → extract ALL relevant concepts from conversation

2. **Analyze conversation for concepts:**
   - Key concepts, technologies, patterns, architectures
   - Technical definitions and methodologies
   - Relationships and hierarchies between concepts
   - Examples and use cases (minimal length, generic names only)
   - Best practices and recommended approaches
   - Pitfalls, edge cases, things to avoid

## Phase 2: Organization

3. **Glob KB structure:** `{{KB_PATH}}/**/*.md` → assess current organization

4. **Apply domain segregation and lazy folder rules** (see SKILL.md)

5. **Determine file paths** using file naming and management rules (see SKILL.md)

## Phase 3: Content Creation

6. **Read existing files** (only if merging needed)

7. **Structure concepts hierarchically** and write files following content structure rules (see SKILL.md)

## Phase 4: Validation & Reporting

8. **Run quality checklist** (see SKILL.md) — additionally verify:
   - [ ] Only information from conversation included (no external supplementation)

9. **Report results** using reporting template (see SKILL.md) with `Source: conversation`
