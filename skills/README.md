# OpenClaw Skills

This repository contains custom skills for [OpenClaw](https://github.com/openclaw/openclaw). Skills are modular, self-contained packages that extend OpenClaw's capabilities by providing specialized knowledge, workflows, and tools.

## What Are Skills?

Skills transform OpenClaw from a general-purpose AI agent into a specialized assistant equipped with domain-specific knowledge and workflows. Think of them as "onboarding guides" for specific domains or tasks — they provide procedural knowledge that no AI model can fully possess.

### What Skills Provide

- **Specialized workflows** — Multi-step procedures for specific domains
- **Tool integrations** — Instructions for working with specific APIs or file formats
- **Domain expertise** — Company-specific knowledge, schemas, business logic
- **Bundled resources** — Scripts, references, and assets for complex tasks

## Repository Structure

```
openclaw-skills/
├── skill-name/              # Individual skill directory
│   ├── SKILL.md            # Required: Skill definition and instructions
│   ├── scripts/            # Optional: Executable code (Python/Bash/etc.)
│   ├── references/         # Optional: Documentation loaded as needed
│   ├── assets/             # Optional: Templates, images, icons
│   └── _meta.json          # Auto-generated: Metadata (do not edit)
└── README.md               # This file
```

## Skill Anatomy

Every skill consists of:

### SKILL.md (Required)

The core skill file with two parts:

1. **YAML Frontmatter** — Always loaded into context (~100 words)
   ```yaml
   ---
   name: duckduckgo-search
   description: Performs web searches using DuckDuckGo. Use when the user needs to search for current events, documentation, or any information that requires web search capabilities.
   allowed-tools: Bash(duckduckgo-search:*), Bash(python:*)
   ---
   ```

2. **Markdown Body** — Loaded only when skill triggers (<5k words)
   - Instructions and guidance for using the skill
   - Workflow steps and patterns
   - Links to bundled resources

### Bundled Resources (Optional)

#### Scripts (`scripts/`)

Executable code for tasks that require deterministic reliability.

- **When to include**: When code is rewritten repeatedly or needs consistency
- **Examples**: PDF rotation scripts, API wrappers, data processors
- **Benefits**: Token efficient, deterministic, may run without loading into context

#### References (`references/`)

Documentation loaded as needed to inform the agent's process.

- **When to include**: For documentation the agent should reference while working
- **Examples**: Database schemas, API docs, company policies, detailed guides
- **Benefits**: Keeps SKILL.md lean, loaded only when needed

#### Assets (`assets/`)

Files used in output, not loaded into context.

- **When to include**: Files used in final output
- **Examples**: Templates, logos, fonts, boilerplate code
- **Use cases**: PowerPoint templates, HTML/React boilerplate, brand assets

## Creating a New Skill

### Step 1: Understand the Use Case

Gather concrete examples of how the skill will be used. Ask:

- What functionality should this support?
- What would users say to trigger this skill?
- Are there other similar skills we should reference?

### Step 2: Plan the Resources

Analyze examples to identify reusable resources:

- **Scripts**: What code gets rewritten repeatedly?
- **References**: What documentation is needed repeatedly?
- **Assets**: What templates or resources are needed?

### Step 3: Initialize the Skill

Use the OpenClaw skill-creator to generate a template:

```bash
# From your OpenClaw workspace
cd /path/to/openclaw
python3 scripts/init_skill.py my-skill --path skills/public --resources scripts,references
```

This creates:
- `my-skill/SKILL.md` with template structure
- `my-skill/scripts/` and `my-skill/references/` directories

### Step 4: Implement the Skill

1. **Add scripts**: Create and test executable scripts in `scripts/`
2. **Add references**: Document schemas, APIs, patterns in `references/`
3. **Add assets**: Include templates, images, or other resources in `assets/`
4. **Write SKILL.md**: Provide clear instructions and workflow guidance

#### SKILL.md Guidelines

- **Use imperative/infinitive form**: "Do X" not "You should do X"
- **Keep it concise**: Context window is a public good
- **Match freedom to task**: High freedom for flexible tasks, low for fragile operations
- **Progressive disclosure**: Core workflow in SKILL.md, details in references

**Frontmatter Best Practices:**

```yaml
---
name: my-skill
description: Concise summary of what the skill does. Include specific triggers: Use when Codex needs to X, Y, or Z.
allowed-tools: Bash(some-tool:*)
---
```

- `name`: Lowercase, hyphens only (e.g., `pdf-editor`)
- `description`: Primary triggering mechanism — be clear about when to use
- `allowed-tools`: Tools this skill requires access to

### Step 5: Package the Skill

Use the packaging script to validate and create a distributable `.skill` file:

```bash
cd /path/to/openclaw
python3 scripts/package_skill.py path/to/skill-name
```

The script validates:
- YAML frontmatter format
- Required fields (name, description)
- Directory structure
- File organization

If validation passes, it creates `skill-name.skill` (a zip archive).

### Step 6: Test and Iterate

- Use the skill on real tasks
- Notice struggles or inefficiencies
- Update SKILL.md or bundled resources
- Re-package and test again

## Design Principles

### 1. Conciseness

Assume the AI is already smart. Only add context it doesn't have. Challenge each piece: "Does this justify its token cost?"

### 2. Progressive Disclosure

Three-level loading system:
1. **Metadata** (~100 words) — Always loaded
2. **SKILL.md** (<5k words) — Loaded on trigger
3. **Resources** (unlimited) — Loaded as needed

### 3. Appropriate Freedom

- **High freedom** (text instructions): Multiple valid approaches, context-dependent
- **Medium freedom** (pseudocode): Preferred pattern exists, some variation OK
- **Low freedom** (scripts): Fragile operations, consistency critical

### 4. No Extraneous Files

Skills should NOT include:
- README.md
- INSTALLATION_GUIDE.md
- CHANGELOG.md
- Any user-facing documentation

Only include files the AI agent needs to do the job.

## Existing Skills

| Skill | Description |
|-------|-------------|
| duckduckgo-search | Web search using DuckDuckGo for real-time information |

## Contributing

1. Fork this repository
2. Create a new skill or improve an existing one
3. Test thoroughly using the packaging script
4. Submit a pull request with a clear description of changes

### Skill Review Checklist

- [ ] SKILL.md has proper frontmatter with `name` and `description`
- [ ] Description clearly explains when to use the skill
- [ ] Body uses imperative/infinitive form
- [ ] SKILL.md is under 500 lines
- [ ] All scripts are tested and working
- [ ] References are clearly linked from SKILL.md
- [ ] No extraneous documentation files
- [ ] Skill packages successfully with no validation errors

## Resources

- [OpenClaw Documentation](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [OpenClaw Community](https://discord.com/invite/clawd)

## License

Skills in this repository may have varying licenses. Check individual skill directories for license information.

---

Built for [OpenClaw](https://github.com/openclaw/openclaw) — the AI agent that grows with you.
