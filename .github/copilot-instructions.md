# Copilot Instructions for Multimodal AI Repository

## Overview

This repository focuses on exploring and demonstrating the capabilities of multimodal AI systems. It
includes Markdown documentation, workflows for CI/CD, and link checking. The repository is
structured to support presentations and tutorials on multimodal AI concepts.

## Key Files and Directories

* **`README.md`**: Provides an overview of the project, its purpose, and learning objectives.
* **`.github/workflows/`**: Contains GitHub Actions workflows for CI and link checking.
  * `ci.yml`: Handles continuous integration tasks.
  * `lychee-link-check.yml`: Checks for broken links in Markdown files.
* **`.markdownlint.json`**: Configuration file for Markdown linting rules.
* **`tutorials/`**: Contains tutorial files for various multimodal AI topics.

## Developer Workflows

### 1. Markdown Linting

* **Purpose**: Ensure all Markdown files adhere to the defined style guide.
* **Command**: The `markdown-linter.yml` workflow automatically lints and fixes Markdown files.
* **Manual Linting**:

  ```bash
  npx markdownlint-cli2 "**/*.md" --config .markdownlint.json --fix
  ```

### 2. Link Checking

* **Purpose**: Validate all links in Markdown files.
* **Workflow**: `lychee-link-check.yml` checks links recursively and excludes email links.
* **Manual Check**:

  ```bash
  lychee --recursive --exclude-mail --timeout 10 *.md
  ```

### 3. Continuous Integration

* **Workflow**: `ci.yml` ensures code quality and runs tests (if applicable).

## Project-Specific Conventions

* **Markdown Style**: Follow the rules in `.markdownlint.json`. Key rules include:
  * Line length: 100 characters.
  * Use `atx` style for headings.
  * Two spaces for line breaks.
* **Commit Messages**: Use conventional commit messages, e.g., `chore: fix markdown lint violations`.

## External Dependencies

* **Lychee**: Used for link checking.
* **Markdownlint CLI**: Used for linting Markdown files.

## Examples

### Adding a New Tutorial

1. Create a new Markdown file in the `tutorials/` directory.
1. Ensure the file adheres to `.markdownlint.json` rules.
1. Add the file to the `lychee-link-check.yml` workflow if it contains links.

### Debugging a Workflow

* Check the logs in the GitHub Actions tab for detailed error messages.
* For local testing, replicate the workflow steps using the provided commands.

## Notes

* Always test changes locally before pushing to avoid breaking workflows.
* Keep the `README.md` updated with any significant changes to the repository structure or purpose.
