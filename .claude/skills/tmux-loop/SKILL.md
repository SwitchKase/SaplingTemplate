---
name: tmux-loop
description: Manage autonomous loop agents in tmux sessions. Spin up, monitor, attach, detach, and manage multiple concurrent loop executions.
---

# tmux Loop Management

Run autonomous loop agents in background tmux sessions. Start work, detach, do other things, check back later.

## Quick Reference

```bash
# Start a new loop session
tmux new-session -d -s loop-feature-name -c /path/to/project \
  './scripts/loop/loop.sh 50'

# List all sessions
tmux list-sessions

# Attach to watch progress
tmux attach -t loop-feature-name

# Detach (from inside): Ctrl+b, then d

# Check output without attaching
tmux capture-pane -t loop-feature-name -p | tail -50

# Kill a session
tmux kill-session -t loop-feature-name
```

## Workflows

### Start a Loop Session

Before starting, ensure the project has:
- `scripts/loop/prompt.md` - Instructions for the agent
- `prd.json` - Tasks from `/generate-stories` (if using task-based loop)

```bash
# Create named session running the loop
tmux new-session -d -s "loop-{feature}" -c "$(pwd)" './scripts/loop/loop.sh'

# With custom iteration limit
tmux new-session -d -s "loop-{feature}" -c "$(pwd)" './scripts/loop/loop.sh 100'
```

**Naming convention:** `loop-{feature-name}` (e.g., `loop-auth-system`, `loop-api-refactor`)

### Monitor Progress

**Quick peek** (without attaching):
```bash
# Last 50 lines of output
tmux capture-pane -t loop-{name} -p | tail -50

# Full scrollback
tmux capture-pane -t loop-{name} -p -S -1000
```

**Attach to watch live:**
```bash
tmux attach -t loop-{name}
```

**Detach** (while attached): Press `Ctrl+b`, then `d`

### Manage Multiple Sessions

```bash
# List all running sessions
tmux list-sessions

# Example output:
# loop-auth: 1 windows (created Wed Jan  8 10:00:00 2025)
# loop-api: 1 windows (created Wed Jan  8 10:15:00 2025)
# main: 1 windows (created Wed Jan  8 09:00:00 2025)

# Switch between sessions (while attached)
# Ctrl+b, then s (shows session list)
# Ctrl+b, then ( (previous session)
# Ctrl+b, then ) (next session)
```

### Stop a Loop

**Graceful** (let current iteration finish):
```bash
# Attach and Ctrl+C
tmux attach -t loop-{name}
# Then press Ctrl+C
```

**Immediate:**
```bash
tmux kill-session -t loop-{name}
```

### Check if Loop Completed

```bash
# Check if session still exists
tmux has-session -t loop-{name} 2>/dev/null && echo "Running" || echo "Finished/Not found"

# Check last output for completion signal
tmux capture-pane -t loop-{name} -p | grep -q "COMPLETE" && echo "✅ Done" || echo "⏳ In progress"
```

## Common Patterns

### Start Loop and Continue Working

```bash
# 1. Start the loop in background
tmux new-session -d -s loop-feature -c "$(pwd)" './scripts/loop/loop.sh 50'
echo "Loop started in background"

# 2. Continue with other work in current terminal
# ...

# 3. Check on it later
tmux capture-pane -t loop-feature -p | tail -20
```

### Run Multiple Loops in Parallel

```bash
# Different features, different directories
tmux new-session -d -s loop-auth -c ~/projects/app './scripts/loop/loop.sh'
tmux new-session -d -s loop-docs -c ~/projects/docs './scripts/loop/loop.sh'

# Check all
tmux list-sessions | grep loop
```

### Resume After Disconnect

If your terminal disconnects, sessions persist:

```bash
# Reconnect to existing session
tmux attach -t loop-feature

# Or list what's running
tmux list-sessions
```

## Troubleshooting

### "session not found"
Session finished or was killed. Check if work completed:
```bash
cd /path/to/project
cat scripts/loop/progress.txt | tail -20
git log --oneline -5
```

### Loop seems stuck
```bash
# Check what's happening
tmux capture-pane -t loop-{name} -p | tail -100

# If truly stuck, kill and check progress
tmux kill-session -t loop-{name}
cat scripts/loop/progress.txt
```

### tmux not installed
```bash
# macOS
brew install tmux

# Ubuntu/Debian
sudo apt install tmux
```

## Session Naming Guidelines

| Pattern | Use Case |
|---------|----------|
| `loop-{feature}` | Feature development loops |
| `loop-{project}-{task}` | Multi-project work |
| `batch-{name}` | Batch processing jobs |
| `test-loop` | Testing loop behavior |

## Integration with Sapling OS

After a loop completes:

1. **Review commits:** `git log --oneline -10`
2. **Check progress:** `cat scripts/loop/progress.txt`
3. **Archive if done:** `./scripts/loop/loop-archive.sh "feature-name"`
4. **Create traces:** Run `/calibrate` to capture learnings
