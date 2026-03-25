# gitagent Hackathon

## The Challenge

Build an AI agent that lives in a git repo. Define it using the gitagent standard, bring it to life with gitclaw, and optionally deploy it serverlessly with clawless.

## How It Works

### Step 1: Define Your Agent (gitagent format)

Create a git repo with this structure:

```
my-agent/
├── agent.yaml          # Manifest: name, version, description, skills, model
├── SOUL.md             # Who your agent is — personality, values, expertise
├── RULES.md            # What your agent must/must never do
├── skills/
│   └── my-skill/
│       └── SKILL.md    # A capability with YAML frontmatter + instructions
└── tools/              # Optional: tool definitions (YAML schemas)
```

**`agent.yaml`** — the manifest:
```yaml
spec_version: "0.1.0"
name: my-hackathon-agent
version: 0.1.0
description: "What your agent does in one line"
model:
  preferred: claude-sonnet-4-5-20250929
skills:
  - my-skill
tags:
  - hackathon
```

**`SOUL.md`** — your agent's identity:
```markdown
# Soul

## Core Identity
I am a [role]. I specialize in [domain].

## Communication Style
[How your agent talks, thinks, responds]

## Values
- [What matters to your agent]
```

**`RULES.md`** — hard constraints:
```markdown
# Rules

## Must Always
- [Non-negotiable behaviors]

## Must Never
- [Hard boundaries]
```

**`skills/my-skill/SKILL.md`** — a capability:
```markdown
---
name: my-skill
description: "What this skill does"
allowed-tools: Bash Read Write
---

# My Skill

Instructions for how the agent should execute this skill.
```

Validate your agent:
```bash
npx gitagent validate
npx gitagent info
```

### Step 2: Build Your Agent (gitclaw SDK)

Use [gitclaw](https://github.com/open-gitagent/gitclaw) to turn your repo into a running agent:

```bash
npm install gitclaw
```

gitclaw reads your gitagent repo and creates a fully functional AI agent — with the identity from SOUL.md, rules from RULES.md, skills from skills/, and tools from tools/. Your agent definition is the git repo. gitclaw is the runtime.

See the [gitclaw README](https://github.com/open-gitagent/gitclaw) for full SDK docs, examples, and API reference.

### Step 3 (Optional): Deploy Serverlessly (clawless)

Want your agent running in the browser with zero infrastructure? Use [clawless](https://github.com/open-gitagent/clawless) — a serverless runtime powered by WebContainers.

```bash
npm install clawless
```

> **Important:** clawless runs in a WebContainer environment with **Node.js/npm only**. Your agent's skills and tools must be Node-compatible. No Python, no system binaries, no Docker. If your skill runs `node` or `npx`, it works. If it needs `python` or `apt-get`, use gitclaw instead.

See the [clawless README](https://github.com/open-gitagent/clawless) for setup, deployment, and limitations.

## Judging Criteria

| Criteria | Weight | What We're Looking For |
|----------|--------|----------------------|
| **Agent Quality** | 30% | Does the agent do something useful? Is the SOUL.md compelling? Are the rules well-defined? |
| **Skill Design** | 25% | Are skills focused, well-documented, and practical? Do they follow the SKILL.md standard? |
| **Working Demo** | 25% | Does it actually run via gitclaw or clawless? Can we see it in action? |
| **Creativity** | 20% | Surprise us. Novel use cases, clever skill compositions, unexpected domains. |

## Resources

| Resource | Link |
|----------|------|
| gitagent standard | https://github.com/open-gitagent/gitagent |
| gitagent spec | https://github.com/open-gitagent/gitagent/blob/main/spec/SPECIFICATION.md |
| gitclaw SDK | https://github.com/open-gitagent/gitclaw |
| clawless (serverless) | https://github.com/open-gitagent/clawless |
| Example agents | https://github.com/open-gitagent/gitagent/tree/main/examples |
| gitagent CLI docs | Run `npx gitagent --help` |

## Quick Reference: gitagent CLI

```bash
npx gitagent init                    # Scaffold a new agent
npx gitagent validate                # Validate your agent
npx gitagent info                    # Show agent summary
npx gitagent export -f system-prompt # Preview as system prompt
npx gitagent export -f claude-code   # Export for Claude Code
npx gitagent export -f cursor        # Export for Cursor
```

## FAQ

**Q: Do I need to use a specific LLM?**
A: No. gitagent is model-agnostic. Set `model.preferred` in agent.yaml to whatever you want — Claude, GPT, Gemini, Llama, etc. gitclaw handles the runtime.

**Q: Can my agent have multiple skills?**
A: Yes. Add as many as you want under `skills/`. Each gets its own directory with a SKILL.md file.

**Q: Can I use sub-agents?**
A: Yes. Add them under `agents/` — each sub-agent is a full gitagent directory with its own agent.yaml, SOUL.md, etc.

**Q: What if my skill needs Python?**
A: Use gitclaw (not clawless). clawless only supports Node.js/npm environments. gitclaw has no such limitation.

**Q: Can I start from an existing agent config?**
A: Yes. `gitagent import --from claude <path>` or `gitagent import --from cursor <path>` converts existing configs to gitagent format.

---

Build something great. Your agent is a git repo. Make it count.
