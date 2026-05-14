# tech-docs

A Copilot CLI plugin for navigating and researching technical documentation.

## What it does

The `tech-docs` plugin provides a custom agent that systematically navigates documentation sites, API references, and local markdown files. It follows a structured 3-phase workflow:

1. **Identify & Fetch** — accepts a URL or file path, auto-detects the doc framework (Docusaurus, ReadTheDocs, MkDocs, OpenAPI, etc.), and fetches the entry page
2. **Map & Navigate** — extracts the table of contents or sidebar structure and performs a guided crawl of only the relevant pages
3. **Synthesize & Answer** — summarizes findings with code snippets, cites sources with URLs, and suggests related pages

## Installation

```sh
copilot plugin install jore731/skills:plugins/tech-docs
```

## Usage

Once installed, the `tech-docs` agent is available for documentation research tasks. Ask it to:

- Look up how a feature works in a library's docs
- Research an API endpoint from its reference docs
- Find code examples for a specific use case
- Compare approaches described in documentation
- Explain configuration options from a tool's docs

### Examples

```
@tech-docs How do I configure authentication in FastAPI? Here are the docs: https://fastapi.tiangolo.com/tutorial/security/

@tech-docs What are the available volume types in Kubernetes? https://kubernetes.io/docs/concepts/storage/volumes/

@tech-docs Look up the OpenAPI spec at https://petstore3.swagger.io/api/v3/openapi.json and explain the pet endpoints
```

## Supported doc frameworks

The agent auto-detects and optimizes navigation for:

- Docusaurus
- ReadTheDocs
- MkDocs / Material for MkDocs
- Sphinx
- OpenAPI / Swagger
- GitHub Wiki
- VitePress / Starlight
- Hugo / GitBook / Nextra
- Local markdown files
- Man pages / CLI help

## Components

| Component | Type | Description |
|-----------|------|-------------|
| `tech-docs.agent.md` | Agent | Core research assistant with 3-phase workflow |
| `SKILL.md` | Skill | Trigger description and quick reference |
| `DOC-TYPES.md` | Reference | Detailed doc-framework detection strategies |
