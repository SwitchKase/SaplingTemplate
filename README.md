# SaplingOS Template

A personal operating system built on Claude Code that learns your preferences over time. This template includes the [Compound Engineering Plugin](https://github.com/EveryInc/compound-engineering-plugin) for structured planning and multi-agent code review.

## Starting a New Project

### Prerequisites

Install these tools first:

| Tool | Install Command | Purpose |
|------|-----------------|---------|
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | `npm install -g @anthropic-ai/claude-code` | AI coding assistant |
| [Beads](https://github.com/steveyegge/beads) | `brew tap steveyegge/beads && brew install bd` | Git-native issue tracking |
| Python 3.8+ | (system dependent) | Schema validation hooks |
| [fzf](https://github.com/junegunn/fzf) (optional) | `brew install fzf` | Fuzzy file matching |

### Step 1: Create Your Project from Template

On GitHub:
1. Go to [SaplingTemplate](https://github.com/SwitchKase/SaplingTemplate)
2. Click **"Use this template"** → **"Create a new repository"**
3. Name your project and create it

Then clone locally:
```bash
git clone https://github.com/YOUR_USERNAME/your-new-project.git
cd your-new-project
```

### Step 2: Initialize

```bash
# Initialize Beads (issue tracking)
bd init

# Start Claude Code
claude

# Run onboarding (first-time setup)
/onboard
```

The `/onboard` command will:
- Ask about you, your project, and your preferences
- Populate `brain/context/` with your info
- Let you choose a creature companion (ember, drift, or bloom)

### Step 3: Start Working

You're ready to go! See the [Workflow Guide](#workflow-guide) below.

---

## What's Included

### SaplingOS (Core System)

| Command | Description |
|---------|-------------|
| `/task` | Execute work with automatic decision tracing |
| `/calibrate` | Review decision traces and improve the system |
| `/onboard` | Initial setup - populate your context |
| `/today` | Morning routine - gather tasks from inbox/email |
| `/commit` | Intelligent atomic git commits |
| `/push` | Push to remote |

### Compound Engineering Plugin

| Command | Description |
|---------|-------------|
| `/workflows:plan` | Create detailed implementation plans (80% planning) |
| `/workflows:work` | Execute plans with git worktrees and task tracking |
| `/workflows:review` | Multi-agent code review before merging |
| `/workflows:compound` | Document learnings for future work |

Plus **27 specialized agents** for review, research, and design tasks.

---

## Workflow Guide

### Daily Workflow

```bash
# Start your day
/today                    # Gather tasks from inbox, email, yesterday

# Pick a task and work on it
/task "Fix the login bug"    # For smaller tasks
# OR
/workflows:plan "Add user auth"  # For substantial features
/workflows:work                   # Execute the plan
```

### When to Use Which

| Scenario | Command |
|----------|---------|
| Bug fix with known solution | `/task` |
| Quick content/copy change | `/task` |
| New feature, unclear approach | `/workflows:plan` → `/workflows:work` |
| Refactor or architectural change | `/workflows:plan` → `/workflows:work` |
| Before merging a big PR | `/workflows:review` |
| After completing 10+ tasks | `/calibrate` |

### Example: Building a Feature

```bash
# 1. Plan the feature
/workflows:plan "Add Stripe payment integration"

# 2. Execute the plan
/workflows:work

# 3. Review before merge
/workflows:review

# 4. Commit
/commit

# 5. Document learnings
/workflows:compound
```

### The Learning Loop

SaplingOS gets smarter over time:

1. **You work** → `/task` or `/workflows:work`
2. **Decisions get traced** → Stored in `brain/traces/`
3. **You calibrate** → `/calibrate` reviews traces
4. **System improves** → Skills and CLAUDE.md get updated
5. **Your creature evolves** → Egg → Hatchling → Juvenile → Adult → Legendary

---

## Directory Structure

```
your-project/
├── CLAUDE.md              # System instructions for Claude
├── agents.md              # Multi-agent workflow instructions
├── brain/                 # Your knowledge vault
│   ├── context/           # About you, your project, your voice
│   ├── entities/          # People and companies
│   ├── calls/             # Call notes and transcripts
│   ├── outputs/           # Deliverables (posts, PRDs, emails)
│   ├── traces/            # Decision traces for calibration
│   ├── notes/             # Daily and weekly notes
│   ├── inbox/             # Tasks pending action
│   └── triage/            # Unclassified items
├── schemas/               # YAML schemas for file validation
├── .claude/               # Claude Code configuration
│   ├── commands/          # Slash commands
│   ├── skills/            # Reusable workflows
│   ├── hooks/             # Automation scripts
│   └── creatures/         # Companion creatures
└── .beads/                # Issue tracking data
```

---

## Configuration

### Environment Variables (Optional)

Create a `.env` file for optional features:

```bash
GEMINI_API_KEY=xxx        # Image generation via /generate-visuals
GITHUB_TOKEN=xxx          # GitHub CLI auth (if not using gh auth login)
```

### Customizing for Your Project

1. **Add project context** - Edit files in `brain/context/` or run `/onboard`
2. **Modify CLAUDE.md** - Add project-specific instructions at the top
3. **Create custom commands** - Add `.md` files to `.claude/commands/`

---

## Creature Companions

Choose your companion during `/onboard`:

| Creature | Element | Theme |
|----------|---------|-------|
| **Ember** | Fire | Fast iteration, breaks blockers |
| **Drift** | Water | Adaptable, flows around obstacles |
| **Bloom** | Nature | Organic growth, cultivates knowledge |

Your creature evolves as you calibrate:
- **Egg** → 0 traces
- **Hatchling** → 10 traces
- **Juvenile** → 100 traces
- **Adult** → 500 traces
- **Legendary** → 1500 traces

---

## License

MIT License - see [LICENSE](LICENSE)

---

Built on [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin), [Obsidian](https://obsidian.md), and [Beads](https://github.com/steveyegge/beads).
