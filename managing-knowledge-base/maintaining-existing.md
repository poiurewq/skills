# Maintaining Existing KB — Workflow

Mode-specific steps for reviewing, revising, and reorganizing existing knowledge base files.
Shared rules (frontmatter, conciseness, naming, folders, quality checklist) are in SKILL.md.

## Phase 1: Assessment

1. **Determine scope:**
   - Specific file(s) named → focus only on those
   - "organize kb" / "clean up kb" → review entire KB
   - Topic mentioned (e.g., "React files") → focus on matching files

2. **Load KB structure:** Glob `{{KB_PATH}}/**/*.md`

3. **Read target files:** Assess current organization, identify quality issues.

## Phase 2: Quality Analysis

For each file, check:

**Content issues:**
- [ ] Excessive verbosity — sentences/paragraphs that could be more concise
- [ ] Low information density — filler words, redundant explanations
- [ ] Specific names in examples (`AuthenticationService`, `DatabaseConnectionPool`)
- [ ] Unnecessary examples — simple concepts with code examples
- [ ] Overly long examples — examples with non-essential code
- [ ] Missing examples — complex concepts that would benefit from illustration
- [ ] Context-dependent language — "we discussed", "as mentioned", etc.
- [ ] Undefined acronyms
- [ ] Poor structure — unclear hierarchy, illogical flow

**Organizational issues:**
- [ ] Generic filenames (`concepts.md`, `notes.md`, `temp.md`)
- [ ] Topic mixing — unrelated domains in single file
- [ ] Improper folder structure — violations of lazy folder rules
- [ ] 3+ related files in root — should be in domain folder
- [ ] Missing frontmatter

**Naming convention issues:**
- [ ] Non-kebab-case filenames
- [ ] Vague topic names

## Phase 3: Apply Improvements

**Content refinement:**

1. **Increase information density:** Rewrite verbose sentences concisely; remove filler; convert prose to bullets/tables; compress while preserving clarity.

2. **Fix examples:**
   - Replace specific names with generic ones (`AuthService` → `Service`)
   - Remove unnecessary examples (simple concepts don't need them)
   - Trim to minimal code needed; remove imports, boilerplate, non-essential setup
   - Add examples only where they significantly clarify complex concepts

3. **Ensure context independence:** Remove source-specific references; define acronyms on first use; add context for self-contained understanding.

4. **Improve structure:** Fix heading hierarchy (`#` title, `##` concepts, `###` sub-topics); group related concepts logically; ensure consistent terminology.

**Organizational improvements:**

1. **Fix filenames:** Rename generic names to specific topics (kebab-case). E.g., `concepts.md` → `react-component-patterns.md`

2. **Apply lazy folder rules** (see SKILL.md):
   - 3+ related files in root → create domain folder, move files
   - Files in wrong folder → move to correct domain
   - Premature folders (< 3 files) → move files to root, remove folder

3. **Separate mixed topics:** If file covers React + PostgreSQL → split into two files.

4. **Fix frontmatter:** Add missing source attribution; ensure proper date format.

**File operations:**
```bash
# Content-only changes: Read then Edit/Write
# File moves/renames:
mkdir -p {{KB_PATH}}/domain-folder/
mv {{KB_PATH}}/old-name.md {{KB_PATH}}/new-location/new-name.md
# File splits: create new files, remove mixed content from original
```

Process files systematically — complete one before moving to next.

## Phase 4: Validation & Reporting

**Run quality checklist** (see SKILL.md)

**Maintenance-specific report:**
```
Maintenance Summary:

Files modified:
- /path/to/file1.md
  - Improved: [concision, density changes]
  - Fixed: [naming, structure, etc.]
  - Removed/Added: [examples changes]

Files reorganized:
- Renamed: old.md → new.md
- Moved: file.md → domain-folder/
- Split: mixed.md → topic-a.md + topic-b.md
- Created folder: frontend/ (3+ files threshold met)

Quality improvements:
- Concision: [X files, ~Y% reduction in verbosity]
- Generic naming: [X examples updated]
- Example optimization: [X removed, Y trimmed, Z added]
- Organization: [X files moved/renamed]

Issues not fixed (require user decision):
- [List any ambiguous items]
```

## Additional Rules

- **Ask user before:** renaming files that could break external references, major reorganization (5+ files), splitting files with significant topic mixing, ambiguous domain classification
- **Empty files:** Report to user, recommend deletion
- **Guiding principle:** Preserve all substantive information — only improve presentation. Concision must not sacrifice accuracy or completeness.

**Quality Standards Quick Reference:**

| Standard | Rule |
|----------|------|
| Information density | Maximum info per sentence; bullets/tables over prose |
| Generic naming | `User`, `Item`, `handleClick` — never `AuthenticationService` |
| Examples | Only for complex concepts; minimal; no imports/boilerplate |
| Context independence | No "as discussed", "earlier we..." — define all acronyms |
| Structure | `#` title → `##` concepts → `###` sub-topics; consistent terminology |
| Source attribution | YAML frontmatter with `source` and `extracted` date |
