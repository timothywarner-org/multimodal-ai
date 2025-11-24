# Init: Multimodal AI Documentation Repo

## Purpose and scope

* Docs-only repo for a 45-minute session plus 10-minute Q&A on multimodal AI
* Audience: consulting staff across skill levels; keep content clear, practical, and demo-friendly
* Focus: M365 Copilot (primary), Google Gemini, GitHub Copilot, ChatGPT, Claude/Claude Code
* Keep contributions to Markdown docs; no application code expected

## Key references

* README.md: session overview, format, and learning objectives
* CLAUDE.md: repository rules, markdown lint guidance, CI, and link-check notes
* .github/copilot-instructions.md: workflow reminders and commands
* tutorials/: add or update tutorials with descriptive file names
* session-links.md and multimodal-ai-resources.md: curated links to keep current

## Writing guidelines

* Follow .markdownlint.json: <=100-char lines, ATX headings, asterisk lists, 2-space nesting
* Use fenced code blocks with language identifiers; avoid trailing whitespace
* Prefer concise, stepwise instructions and conversation starters that suit live demos
* Keep tone accessible for mixed skill levels; include citations or source notes when useful
* Use conventional commits (e.g., `docs: add copilot tutorial`) for any check-ins

## Quality checks

* Lint Markdown: `npx markdownlint-cli2 "**/*.md" --config .markdownlint.json --fix`
* Check links: `lychee --recursive --exclude-mail --timeout 10 *.md`
* GitHub Actions in .github/workflows mirror these checks; resolve their findings before release

## Content boundaries

* Keep scope to multimodal AI and M365 Copilot-centric scenarios; avoid unrelated code drops
* Ensure examples are grounded in real tools and workflows, not theory alone
* When adding resources, verify URLs and prefer official or reputable sources
