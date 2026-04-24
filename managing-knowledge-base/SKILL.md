---
name: managing-knowledge-base
description: "Manages a personal knowledge base: extracts concepts from conversations or external sources (URLs, files), and maintains/reorganizes existing KB files. Handles 'add to kb', 'extract to kb', 'organize kb', 'review kb', and any request mentioning 'kb' with extraction or maintenance intent."
---

# Managing Knowledge Base

Unified skill for knowledge base extraction, organization, and maintenance.

## First-Run Configuration

1. Read `config.local.md` from this skill's directory
2. If missing or unreadable:
   - Use AskUserQuestion to ask: "What is the path to your knowledge base directory? (It will be created if it doesn't exist.)"
   - No default — wait for user input
   - Run `mkdir -p <chosen path>` to create it if needed
   - Write `config.local.md` with the chosen path (YAML frontmatter: `kb_path: /path/to/kb`, no trailing slash)
3. If present: parse `kb_path` from YAML frontmatter
4. Use `{{KB_PATH}}` throughout all workflows below — replace with the configured path

## Mode Routing

Determine the mode from the user's request, then read the corresponding reference file:

| Condition | Mode | Reference File |
|-----------|------|----------------|
| No URL/file provided; user wants to capture concepts from conversation | **Extract from conversation** | `extracting-from-conversation.md` |
| URL or file path provided | **Extract from external source** | `extracting-from-external.md` |
| Organize, review, revise, clean up, maintain intent | **Maintain existing KB** | `maintaining-existing.md` |

Read the reference file for mode-specific workflow steps. The shared rules below apply to ALL modes.

## Shared Rules

### Source Attribution

**REQUIRED:** Every KB file must include YAML frontmatter:

```markdown
---
source: [source description]
extracted: [YYYY-MM-DD]
---
```

- Conversation extraction: `source: Claude Code`
- External source: `source: [URL or file path]`

### Conciseness and Information Density

- Prioritize high information density over verbosity
- Every sentence must deliver maximum value — eliminate filler
- Use precise, efficient language — favor brevity without sacrificing clarity
- Present information in compact, scannable formats (bullets, tables)
- Distill concepts to their essence while preserving critical details

### Common Mistakes to Avoid

- DO NOT mix unrelated topics in single file (React + PostgreSQL → separate files)
- DO NOT create generic filenames (`concepts.md`, `notes.md`)
- DO NOT create folders prematurely (wait for 3+ related files)
- DO NOT duplicate content when merging with existing files
- DO NOT write source-specific references ("as we discussed...", "in that chat...")
- DO NOT assume reader knows acronyms or context (define on first use)

### Domain Segregation

- Same technology / tightly integrated → ONE file (React hooks + context; Docker + K8s in orchestration)
- Different stacks / disciplines → SEPARATE files (React + PostgreSQL; Python + Git)
- Sub-topics of broader concept can share a file (JWT, OAuth, sessions → `authentication-strategies.md`)

### Lazy Folder Rules

1. First file(s) on any topic → create flat in `{{KB_PATH}}/` root
2. When 3+ related files accumulate → create domain folder, move existing related files
3. Within domain folders → create subdomain folders only when 5+ files for same sub-topic
4. < 3 related files in folder → move back to root or parent folder
5. Common domain folders: `frontend/`, `backend/`, `databases/`, `devops/`, `cloud/`, `languages/`, `frameworks/`, `tools/`, or domain-specific (`music/`, `art/`, `finance/`)

### File Naming and Management

- Filenames: kebab-case, specific topics (good: `react-hooks.md`; bad: `concepts.md`)
- Glob KB structure: `{{KB_PATH}}/**/*.md` → assess organization
- Related files already in folder? → use that folder
- 3+ related files in root? → create domain folder + move existing
- < 3 related files? → place in root
- Existing file on same topic → Read → merge intelligently, avoid duplication
- Use `mkdir -p` for new directories

### Content Structure

- `#` for document title, `##` for main concepts, `###` for sub-topics
- **Bold** for definitions and key terms
- Code blocks with language tags (only when examples significantly clarify complex concepts)
- Keep examples minimal — shortest code illustrating the point, generic names only (`User`, `Item`, `handleClick`)

### Quality Checklist (all must pass before completion)

- [ ] Source attribution frontmatter present
- [ ] Zero source-specific references ("we discussed", "in that chat...")
- [ ] All acronyms defined on first use
- [ ] No topic mixing (each file = one coherent domain)
- [ ] Filename is specific and descriptive (kebab-case)
- [ ] Folder structure follows lazy rules
- [ ] No duplication if merging with existing file
- [ ] Clear hierarchical structure
- [ ] High information density — no verbose filler
- [ ] Examples are minimal, generic names only

### Error Handling

- Missing directory → Create with `mkdir -p`
- File read fails → Treat as new file creation (extraction) or report & skip (maintenance)
- Organization unclear → Default to root until 3+ files threshold
- Domain boundaries ambiguous → Ask user (combine or separate?)
- Topic scope unclear → Ask user

## Reporting Template

```
[Source: conversation | URL | file path]

Files modified:
- /path/file1.md - Created (topic summary)
- /path/file2.md - Updated (what was added)

Organization decisions:
- [Any folder creation or file moves]

Scope:
- Extracted: [topics] OR "all concepts"
- Filtered out: [ignored topics] OR "none"
```
