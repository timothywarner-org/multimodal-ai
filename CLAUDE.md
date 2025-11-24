# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this
repository.

## Repository Purpose

This is a **documentation-only repository** for a 45-minute live training session on multimodal
generative AI. The target audience is consulting company personnel with varying skill levels. Focus
areas include:

* M365 Copilot ecosystem (primary focus)
* Google Gemini Coding Assistant
* GitHub Copilot
* ChatGPT
* Claude and Claude Code

The repository contains Markdown tutorials, resources, and CI/CD workflows but **no application code**.

## Structure

* `tutorials/` - Tutorial files for M365 agents, Copilot Studio, and Foundry agents
* `multimodal-ai-resources.md` - Curated list of resources for the session
* `.github/workflows/` - CI/CD automation for Markdown quality
* `.markdownlint.json` - Markdown linting configuration
* `README.md` - Session overview and learning objectives

## Common Commands

### Markdown Linting

Lint all Markdown files with auto-fix:

```bash
npx markdownlint-cli2 "**/*.md" --config .markdownlint.json --fix
```

### Link Checking

Validate all links in Markdown files:

```bash
lychee --recursive --exclude-mail --timeout 10 *.md
```

## Markdown Style Requirements

All Markdown files must follow `.markdownlint.json` rules:

* **Line length**: 100 characters (excluding code blocks, headings, tables)
* **Heading style**: ATX style (`#`, `##`, etc.)
* **List style**: Asterisks (`*`) for unordered lists
* **Indentation**: 2 spaces for nested lists
* **Line breaks**: Two spaces for explicit breaks
* **Code blocks**: Fenced style with language identifiers

## Workflows

### Markdown Linter Workflow

* **Trigger**: Manual dispatch (`workflow_dispatch`)
* **Purpose**: Lints and auto-fixes Markdown files
* **Auto-commits**: Yes - commits fixes as `chore: fix markdown lint violations`

### Lychee Link Checker Workflow

* **Trigger**: Manual dispatch (`workflow_dispatch`)
* **Purpose**: Checks for broken links recursively
* **Output**: JSON report uploaded as artifact
* **Cache**: Uses 7-day cache for link validation

## Content Guidelines

When adding new tutorials or documentation:

1. Create files in `tutorials/` directory with descriptive names
1. Ensure content adheres to Markdown linting rules
1. All external links should be valid (will be checked by Lychee)
1. Follow conventional commit messages (e.g., `docs: add copilot tutorial`)
1. Test Markdown locally before committing

## Session Context

This is a **live presentation repository** for a 45-minute session plus 10-minute Q&A. Content
should be:

* Accessible to all skill levels
* Focused on practical demonstrations and visual examples
* Structured around learning objectives in README.md
* Ready for interactive Q&A
