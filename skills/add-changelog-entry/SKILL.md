---
name: add-changelog-entry
description: Append an entry to the `[Unreleased]` section of CHANGELOG.md, creating the file and section if they do not exist. Use when the user asks to "log a change", "add a changelog entry", "note this in the changelog", or mentions updating CHANGELOG.md for a new feature, fix, or breaking change.
---

# Add Changelog Entry

Appends a single bullet to the `[Unreleased]` section of `CHANGELOG.md` following the [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) convention.

## Input

The user provides:

- **category** — one of `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`. If unclear, ask.
- **entry** — a short, imperative description of the change (e.g. "Support for dark mode toggle").

## Algorithm

1. Locate `CHANGELOG.md` at the repo root.
   - If it does not exist, create it with this skeleton:
     ```markdown
     # Changelog

     All notable changes to this project will be documented in this file.

     The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
     and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

     ## [Unreleased]
     ```
2. Ensure an `## [Unreleased]` section exists. If not, insert it immediately below the header block.
3. Under `## [Unreleased]`, ensure a `### <category>` subsection exists (preserve the Keep a Changelog ordering: Added → Changed → Deprecated → Removed → Fixed → Security). If not, insert it in the correct position.
4. Append `- <entry>` as the last bullet under that subsection.
5. Do not modify any other sections.
6. Do not commit — leave the staging to the user.

## Output

Report the file path and the exact line added. Do not summarize existing entries.

## Example

User: "log a changelog entry: fixed a crash when opening empty files"

Result — `CHANGELOG.md` gains:

```markdown
## [Unreleased]

### Fixed
- Crash when opening empty files
```
