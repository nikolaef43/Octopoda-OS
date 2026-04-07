# Octopoda

### The open-source memory operating system for AI agents.

Persistent memory, semantic search, knowledge graphs, loop detection, agent messaging, crash recovery, and real-time observability. Local-first. Works offline. Optionally sync to cloud.

[![PyPI](https://img.shields.io/pypi/v/octopoda)](https://pypi.org/project/octopoda/)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![Tests](https://img.shields.io/badge/tests-208%20passing-brightgreen)]()
[![GitHub release](https://img.shields.io/github/v/release/RyjoxTechnologies/Octopoda-OS)](https://github.com/RyjoxTechnologies/Octopoda-OS/releases)

---

## Quick Start (Local, No Signup)

```bash
pip install octopoda
```

```python
from octopoda import AgentRuntime

agent = AgentRuntime("my_agent")
agent.remember("user_pref", "Alice is vegetarian and lives in London")
result = agent.recall("user_pref")
# Works immediately. SQLite on your machine. No API key. No cloud.
```

That's it. Memory persists across restarts, crashes, and deployments.

---

## Why Octopoda

AI agents forget everything between sessions. Every framework treats memory as disposable. Octopoda fixes that with a proper memory layer that gives agents:

1. **Persistent memory** that survives restarts and crashes
2. **Semantic search** to find memories by meaning, not just exact keys
3. **Loop detection** that catches agents stuck in repetitive patterns
4. **Agent-to-agent messaging** for multi-agent coordination
5. **Knowledge graphs** that map entities and relationships automatically
6. **Real-time observability** so you can see what your agents know and why they make decisions

### How It Compares

| | Octopoda | Mem0 | Zep | LangChain Memory |
|---|---|---|---|---|
| **Open source** | MIT | Apache 2.0 | Partial (CE) | MIT |
| **Local-first** | Yes (SQLite) | Cloud-first | Cloud-first | In-process |
| **Loop detection** | 5-signal engine | No | No | No |
| **Agent messaging** | Built-in | No | No | No |
| **Temporal versioning** | Full history | No | No | No |
| **Crash recovery** | Snapshots + restore | N/A | No | No |
| **Cross-agent sharing** | Shared memory bus | No | No | No |
| **MCP server** | 25 tools | No | No | No |
| **Knowledge graph** | spaCy NER | No | No | No |
| **Semantic search** | Local embeddings | Cloud embeddings | Cloud embeddings | Needs vector DB |
| **Framework integrations** | LangChain, CrewAI, AutoGen, OpenAI | LangChain | LangChain | Own only |
| **Pricing** | Free (open core) | Free + paid | Free CE + paid | Free |

---

## Core Features

Everything below works out of the box with `pip install octopoda`. No extras needed unless noted.

### Semantic Search

Find memories by meaning, not exact keys. Works automatically on cloud. For local-only semantic search, add `pip install octopoda[ai]` (downloads a 33MB embedding model).

```python
agent.remember("bio", "Alice is a vegetarian living in London")
agent.remember("work", "Alice is a senior engineer at Google")

results = agent.recall_similar("where does the user work?")
# Returns: "Alice is a senior engineer at Google" (score: 0.82)
```

### Loop Detection

Catches agents stuck in repetitive patterns. Five signals: write similarity, key overwrites, velocity spikes, alert frequency, goal drift.

```python
status = agent.get_loop_status()
# {"severity": "orange", "score": 45, "signals": [...]}
# Every signal tells you what's wrong and exactly what to do about it.
```

### Agent Messaging

Agents communicate through shared inboxes. No shared database needed.

```python
agent_a.send_message("agent_b", "Found a bug in auth", message_type="alert")
messages = agent_b.read_messages(unread_only=True)
agent_a.broadcast("Deploy starting in 5 minutes")
```

### Goal Tracking

Set goals with milestones. Integrates with drift detection.

```python
agent.set_goal("Migrate to PostgreSQL", milestones=["Backup", "Schema", "Migrate", "Validate"])
agent.update_progress(milestone_index=0, note="Backup done")
agent.get_goal()  # {"progress": 0.25, "milestones_completed": 1}
```

### Memory Management

```python
agent.forget("outdated_config")              # Delete specific memories
agent.forget_stale(days=30)                  # Remove old memories
agent.consolidate()                          # Merge duplicates
agent.summarize_old_memories(older_than_days=7)  # Compress into summaries
agent.memory_health()                        # {"score": 78, "issues": [...]}
```

### Export / Import

```python
bundle = agent.export_memories()
new_agent.import_memories(bundle)
```

### Crash Recovery

```python
agent.snapshot("before_migration")
# ... something goes wrong ...
agent.restore("before_migration")
```

### Shared Memory

Agents share knowledge across processes with conflict detection.

```python
agent_a.share("research_pool", "analysis", {"findings": "..."})
data = agent_b.read_shared("research_pool", "analysis")
```

### Knowledge Graph

Auto-extracts entities and relationships. Requires `pip install octopoda[nlp]` for local spaCy.

```python
agent.remember("team", "Alice manages the London team with Bob and Carol")
related = agent.related("Alice")
```

---

## Framework Integrations

Drop-in memory for the frameworks you already use.

```python
# LangChain
from synrix_runtime.integrations.langchain_memory import SynrixMemory
memory = SynrixMemory(agent_id="my_chain")

# CrewAI
from synrix_runtime.integrations.crewai_memory import SynrixCrewMemory
crew_memory = SynrixCrewMemory(crew_id="research_crew")

# AutoGen
from synrix_runtime.integrations.autogen_memory import SynrixAutoGenMemory
memory = SynrixAutoGenMemory(group_id="dev_team")

# OpenAI Agents SDK
from synrix.integrations.openai_agents import octopoda_tools
tools = octopoda_tools("my_agent")
```

---

## MCP Server

Give Claude, Cursor, or any MCP-compatible AI persistent memory with zero code.

```bash
pip install octopoda[mcp]
```

Add to your Claude Desktop config (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "octopoda": {
      "command": "octopoda-mcp"
    }
  }
}
```

**20+ tools available:** memory operations, semantic search, loop detection, goal tracking, agent messaging, memory health, summarization, filtered search, and more.

---

## Cloud Dashboard

Real-time monitoring, memory exploration, anomaly detection, and agent health — all from your browser.

![Octopoda Dashboard](docs/images/dashboard-overview.png)

Sign up free at [octopodas.com](https://octopodas.com) to get your API key and dashboard access.

---

## Cloud Setup

**1. Create an account**

Sign up at [octopodas.com](https://octopodas.com). You'll receive an API key and a verification code via email.

**2. Set your API key**

```bash
export OCTOPODA_API_KEY=sk-octopoda-...
```

Or run `octopoda-login` in your terminal to sign up or log in interactively.

**3. Use the cloud API**

```python
from octopoda import Octopoda

client = Octopoda()  # Reads OCTOPODA_API_KEY from env
agent = client.agent("my_agent")
agent.write("preference", "dark mode")
results = agent.search("user preferences")
# Returns results with similarity scores
```

Your existing local code works with cloud too — just set the API key and `AgentRuntime` automatically syncs to the cloud.

**4. Open the dashboard**

Go to [octopodas.com/dashboard](https://octopodas.com/dashboard) to see your agents, memories, loop detection, anomalies, and performance metrics in real time.

### Plans

| | Free | Pro ($19/mo) | Business ($79/mo) |
|---|---|---|---|
| **Agents** | 5 | 25 | 75 |
| **Memories** | 5,000 | 250,000 | 1,000,000 |
| **AI extractions** | 100 (platform key) | 100 (then your own key) | 100 (then your own key) |
| **Dashboard** | Yes | Yes | Yes |
| **Semantic search** | Yes | Yes | Yes |
| **Rate limit** | 60 rpm | 300 rpm | 1,000 rpm |

---

## Installation Options

```bash
pip install octopoda              # Core (local memory, ~5 dependencies)
pip install octopoda[ai]          # + Local embeddings for semantic search
pip install octopoda[nlp]         # + spaCy for knowledge graph extraction
pip install octopoda[mcp]         # + MCP server for Claude/Cursor
pip install octopoda[server]      # + FastAPI cloud server
pip install octopoda[all]         # Everything
```

---

## Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `OCTOPODA_LLM_PROVIDER` | `none` | LLM for fact extraction: `openai`, `anthropic`, `ollama`, `none` |
| `OCTOPODA_OPENAI_API_KEY` | | OpenAI API key |
| `OCTOPODA_OPENAI_BASE_URL` | `https://api.openai.com/v1` | Any OpenAI-compatible endpoint |
| `OCTOPODA_ANTHROPIC_API_KEY` | | Anthropic API key |
| `OCTOPODA_OLLAMA_URL` | `http://localhost:11434` | Ollama server URL |
| `OCTOPODA_EMBEDDING_MODEL` | `BAAI/bge-small-en-v1.5` | Embedding model (33MB, CPU) |
| `SYNRIX_DATA_DIR` | `~/.synrix/data` | Data directory |

---

## Architecture

```
octopoda/                    — Public entry point (pip install octopoda)
synrix/                      — SDK layer
  sqlite_client.py           — SQLite + WAL + vector search + knowledge graph
  embeddings.py              — Local embeddings (bge-small-en-v1.5, 33MB)
  cloud.py                   — Cloud SDK client (Octopoda class)
  fact_extractor.py          — Multi-provider LLM fact extraction
synrix_runtime/              — Runtime layer
  api/
    runtime.py               — AgentRuntime (core: remember, recall, search, loops, goals, messaging)
    cloud_server.py          — FastAPI cloud API (multi-tenant, auth, rate limiting)
    mcp_server.py            — MCP server (20+ tools, stdio transport)
  monitoring/
    metrics.py               — Performance metrics + anomaly detection
    audit.py                 — Full audit trail
    brain.py                 — Brain Intelligence (Drift Radar, Contradiction Shield)
  integrations/              — LangChain, CrewAI, AutoGen, OpenAI Agents
  dashboard/                 — Real-time monitoring (Flask + SSE)
```

**Local storage:** SQLite with WAL mode. No external database required.
**Cloud storage:** PostgreSQL with pgvector. Multi-tenant with row-level security.
**Embeddings:** `BAAI/bge-small-en-v1.5` — 384 dimensions, 33MB, CPU-only.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for setup instructions and guidelines.

## Security

See [SECURITY.md](SECURITY.md) for reporting vulnerabilities.

## License

MIT — use it however you want. See [LICENSE](LICENSE).

---

Built by [RYJOX Technologies](https://octopodas.com) | [Documentation](https://octopodas.com/docs) | [Cloud API](https://api.octopodas.com) | [PyPI](https://pypi.org/project/octopoda/)
