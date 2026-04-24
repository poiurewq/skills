# Extracting from External Source — Workflow

Mode-specific steps for extracting knowledge from a URL or file.
Shared rules (frontmatter, conciseness, naming, folders, quality checklist) are in SKILL.md.

## Phase 1: Source Acquisition

1. **Determine topic scope:**
   - User specifies topics → extract ONLY those (be generous: "React stuff" includes closely related concepts)
   - No topics specified → extract ALL relevant concepts

2. **Fetch content from source:**

   Determine source type:
   - `http://` or `https://` → URL (use WebFetch)
   - `file://` → Local file URI (strip `file://` prefix, use Read tool)
   - `/`, `~`, or `./` → Local file path (use Read tool)

   **Handling `file://` URIs:** Strip `file://` prefix to get absolute path (`file:///home/user/doc.md` → `/home/user/doc.md`)

   **AI Chat Share Links:**

   | Platform | URL Pattern |
   |----------|-------------|
   | Claude | `claude.ai/share/*` |
   | Gemini | `gemini.google.com/share/*`, `g.co/gemini/*` |
   | Grok | `x.com/i/grok/share/*` |
   | Perplexity | `perplexity.ai/search/*` |

   **Local file formats:**

   | Type | Extensions | Notes |
   |------|------------|-------|
   | Markdown | `.md`, `.mdx` | Direct extraction |
   | Plain text | `.txt`, `.text` | Direct extraction |
   | PDF | `.pdf` | Read tool handles automatically |
   | JSON | `.json` | Parse for content |
   | Code | `.py`, `.js`, `.ts`, etc. | Extract documented concepts |
   | Jupyter | `.ipynb` | Code cells + markdown + outputs |

   For local files: use Read tool with absolute path, expand `~` to full home directory.

3. **Parse content:** Identify conversation structure, code blocks, explanations, recommendations.

## Phase 2: Concept Analysis

4. **Extract from content:**
   - Key concepts, technologies, patterns, architectures
   - Technical definitions
   - Relationships between concepts
   - Examples and use cases (minimal, generic names only)
   - Best practices and pitfalls

   Filter based on topic scope from step 1.

5. **Apply domain segregation** (see SKILL.md)

## Phase 3: Organization

6. **Glob KB structure:** `{{KB_PATH}}/**/*.md` → assess current organization

7. **Apply lazy folder rules and file naming** (see SKILL.md)

## Phase 4: Content Creation

8. **Read existing files** (only if merging needed)

9. **Write files** following content structure rules (see SKILL.md)

## Phase 5: Validation & Reporting

10. **Run quality checklist** (see SKILL.md)

11. **Report results** using reporting template (see SKILL.md) with `Source: [URL or file path]`

## Additional Error Handling

- WebFetch fails → Inform user to check URL accessibility
- File not found → Report exact path attempted, ask user to verify
- Permission denied → Report error, suggest checking permissions
- Empty file → Report, no extraction possible
- Binary non-PDF → Report unsupported, suggest text alternative
- URL requires authentication → Ask user
- Source content extremely broad → Suggest focused extraction
