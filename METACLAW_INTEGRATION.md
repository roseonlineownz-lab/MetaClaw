# MetaClaw Integration for NovaMaster

MetaClaw is a continual meta-learning framework that enables AI agents to learn and evolve from conversations.

## What MetaClaw Does

- **Skill Injection**: Automatically injects relevant skills at each turn
- **Skill Evolution**: Generates new skills from failure trajectories
- **Memory (Contexture)**: Cross-session memory persistence
- **RL Training**: GRPO-based policy optimization (optional, requires cloud backend)

## Installation

MetaClaw is installed at `/home/faramix/MetaClaw`

```bash
pip install -e ".[rl,evolve,scheduler]"
```

## Configuration

Config file: `~/.metaclaw/config.yaml`

Current mode: `skills_only` (no GPU required)

## Usage with Claude Code

To use MetaClaw as a proxy for Claude Code:

```bash
# Start MetaClaw
metaclaw start --mode skills_only --port 30001

# Set environment variables for Claude Code
export ANTHROPIC_BASE_URL=http://127.0.0.1:30001
export ANTHROPIC_API_KEY=metaclaw
```

## Integration with ClawMem

MetaClaw can work alongside ClawMem:

```
Claude Code
    │
    ├── ClawMem hooks (context-surfacing, etc.)
    │
    ▼
MetaClaw Proxy (localhost:30001)
    │
    ├── Skills injection (6 per turn)
    ├── Contexture memory
    └── Skill evolution
    │
    ▼
Anthropic API (Claude)
```

## Commands

```bash
metaclaw setup              # Configuration wizard
metaclaw start              # Start proxy (default: madmax mode)
metaclaw start --mode skills_only  # Lightweight mode
metaclaw stop               # Stop proxy
metaclaw status             # Check status
metaclaw config show        # Show config
metaclaw config KEY VALUE   # Set config value
```

## Skills Location

Built-in skills: `~/.metaclaw/skills/`

Add custom skills as `SKILL.md` files in that directory.

## Integration with OpenClaw

MetaClaw can auto-configure OpenClaw to use the proxy:

```bash
metaclaw config claw_type openclaw
metaclaw start
# OpenClaw gateway restart automatically
```

## Memory System (v0.4.0+)

The Contexture layer persists:

- Episodic memory (past events)
- Semantic memory (facts)
- Preferences (user preferences)
- Project state (goals, tasks)
- Working summary (recent activity)

Memory location: `~/.metaclaw/memory/`

## Operating Modes

| Mode | Skills | RL Training | GPU Required |
|------|--------|--------------|--------------|
| `skills_only` | ✅ | ❌ | No |
| `rl` | ✅ | ✅ | Cloud |
| `madmax` | ✅ | ✅ (idle) | Cloud |

## Related Projects

- **ClawMem**: Cross-session memory (already integrated)
- **OpenClaw**: Personal AI assistant
- **Hermes Agent**: Python RL agent framework
- **GoClaw**: Go-based multi-tenant gateway

## Links

- GitHub: https://github.com/aiming-lab/MetaClaw
- Paper: https://arxiv.org/abs/2603.17187