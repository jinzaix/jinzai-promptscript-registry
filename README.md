# Jinzai PromptScript Registry

Shared AI instruction standards for all Jinzai repositories. Compiles to Cursor rules, Claude Code, GitHub Copilot, and other targets via [PromptScript](https://getpromptscript.dev).

## Packages

| ID | Purpose |
|----|---------|
| `@jinzai/base` | Platform rules, docs map, writing standards |
| `@jinzai/stacks/rust-worker` | Rust workspace + Cloudflare Worker |
| `@jinzai/stacks/cloudflare` | D1/R2/Vectorize/AI Search roles |
| `@jinzai/fragments/data-quality` | Real data, Japanese validation |
| `@jinzai/fragments/security-compliance` | Security + Japan PII |
| `@jinzai/fragments/documentation` | Diátaxis documentation rules |
| `@jinzai/fragments/agent-development` | CLI-first agent workflow |

## Usage in a project

### Git registry (recommended after publish)

```yaml
# promptscript.yaml
version: '1'

input:
  entry: .promptscript/project.prs

targets:
  - cursor
  - claude
  - github

registry:
  git:
    url: https://github.com/jinzaix/jinzai-promptscript-registry.git
    ref: main

registries:
  '@jinzai': github.com/jinzaix/jinzai-promptscript-registry
```

```promptscript
# .promptscript/project.prs
@meta {
  id: "jinzai-core"
  syntax: "1.0.0"
}

@inherit @jinzai/stacks/rust-worker
@use @jinzai/fragments/data-quality
@use @jinzai/fragments/security-compliance
@use @jinzai/fragments/documentation
@use @jinzai/fragments/agent-development
```

```bash
prs compile
```

### Local registry (development)

```yaml
registry:
  path: ../jinzai-promptscript-registry
```

## Validate

```bash
prs registry validate --strict
```

## Publish

```bash
prs registry publish . -m "Add Jinzai platform packages"
prs registry publish . --tag v0.1.0
```

## Create GitHub repo

```bash
cd jinzai-promptscript-registry
git init
git add .
git commit -m "Initial Jinzai PromptScript registry"
gh repo create jinzaix/jinzai-promptscript-registry --public --source=. --push
```

Replace `jinzaix` with your GitHub org.

## Related

- [jinzai-core](https://github.com/jinzaix/jinzai-core) — primary consumer
- [PromptScript registry guide](https://getpromptscript.dev/dev/guides/registry/)
