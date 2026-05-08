# 🚀 Ruflo Quick Reference Card

## Installation & Setup ✅
```bash
# Already done! But for reference:
cd ruflo-main
pnpm install              # 748 packages
pnpm run build            # TypeScript → dist/
npx ruflo init --full     # Full initialization
```

---

## 🎯 Start Services

```bash
# Start all services at once
npx ruflo init --start-all

# Or individually:
npx ruflo daemon start      # Background worker
npx ruflo mcp start         # Claude Code MCP server (stdio)
pnpm run dev               # Development watch mode
```

---

## 📊 Check Status

```bash
# System health
npx ruflo status

# Daemon logs
tail -f .claude-flow/daemon.log

# MCP server test
npx ruflo mcp test
```

---

## 🤖 Agent Operations

```bash
# List available agents
ls .claude/agents/

# Common agents:
- analysis      (Data analysis & insights)
- architecture  (System design)
- browser       (Web automation)
- development   (Code generation)
- review        (Code review)
- research      (Information gathering)
- consensus     (Multi-agent voting)
```

---

## 🧠 Memory Operations

```javascript
// Store a fact
memory_store({
  namespace: "project",
  key: "pattern-x",
  value: {...}
})

// Search memory
memory_search({
  query: "similar patterns",
  limit: 10
})

// Clear unused entries
memory_cleanup()
```

---

## 🐝 Swarm Commands

```javascript
// Initialize a swarm
swarm_init({
  agents: ["analysis", "development"],
  task: "refactor code",
  strategy: "consensus"
})

// Get swarm status
swarm_status()
```

---

## 📁 Key Files & Directories

| Path | Purpose |
|------|---------|
| `.claude/` | Claude Code integration (agents, skills, commands) |
| `.claude-flow/` | V3 runtime (config, logs, data) |
| `.claude-flow/config.yaml` | Main configuration |
| `.mcp.json` | MCP server configuration |
| `.swarm/memory.db` | Vector memory database |
| `dist/` | Compiled JavaScript |
| `CLAUDE.md` | Claude Code guidance |

---

## ⚙️ Configuration Files

### `.claude-flow/config.yaml`
```yaml
swarm:
  topology: hierarchical-mesh
  maxAgents: 15
  autoScale: true

memory:
  backend: hybrid
  enableHNSW: true
  cacheSize: 100
```

### `.claude/settings.json`
```json
{
  "hooks": {
    "onError": "error-handler",
    "onTaskComplete": "learning"
  }
}
```

---

## 🔥 Common Tasks

### Spawn an agent for analysis
```
/agent spawn analysis --task "analyze my code"
```

### Enable auto-learning
Edit `.claude/settings.json`:
```json
{
  "learning": {
    "autoConsolidate": true,
    "extractPatterns": true
  }
}
```

### Monitor performance
```bash
ls -la .claude-flow/metrics/
tail -f .claude-flow/logs/*
```

### Reset everything (WARNING!)
```bash
npx ruflo reset --full
```

---

## 📈 Architecture at a Glance

```
Claude Code ← MCP (27 tools) → Ruflo V3
                ↓
        ┌─ Daemon (background)
        ├─ Memory (HNSW vectors)
        ├─ Swarm (15 agents max)
        ├─ Hooks (7 types)
        └─ Learning (Self-optimize)
```

---

## 🎯 Available Agents (Sample)

```
✓ analysis          ✓ devops
✓ architecture      ✓ documentation
✓ browser           ✓ execution
✓ consensus         ✓ federation
✓ core              ✓ intelligence
✓ custom            ✓ learning
✓ data              ✓ memory
✓ development       ✓ optimization
✓ review            ✓ research
✓ security          ✓ testing
✓ tools             ✓ and 11+ more...
```

---

## 💡 Pro Tips

1. **Enable hooks** for automatic behavior routing
2. **Use swarms** for complex multi-step tasks
3. **Monitor memory** size to avoid bloat
4. **Check logs** when agents misbehave
5. **Test patterns** before enabling auto-consolidation
6. **Scale agents** up to max of 15 for parallel work

---

## 🐛 Troubleshooting

| Problem | Solution |
|---------|----------|
| MCP not starting | Check `npx ruflo mcp test` |
| Daemon crashed | Review `.claude-flow/daemon.log` |
| Memory growing | Run `memory_cleanup()` |
| Agents not responding | Verify with `npx ruflo status` |
| High latency | Check swarm size (max 15) |

---

## 📚 Learn More

- Full architecture: [SETUP_REVIEW.md](./SETUP_REVIEW.md)
- Agents details: [AGENTS.md](./AGENTS.md)
- Security: [SECURITY.md](./SECURITY.md)
- Configuration: [CLAUDE.md](./CLAUDE.md)
- Changes: [CHANGELOG.md](./CHANGELOG.md)

---

**Status:** ✅ Production Ready | **Agents:** 23+ active | **Skills:** 33 | **Tools:** 27+

*Last updated: 2026-05-08*
