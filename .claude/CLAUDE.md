# CLAUDE.md

Claude Code reads AGENTS.md only as a fallback when no CLAUDE.md exists in the
repo or its parents. This shim guarantees the project instructions are always
loaded regardless of parent-directory CLAUDE.md files.

All project knowledge lives in the root AGENTS.md:

@../AGENTS.md
