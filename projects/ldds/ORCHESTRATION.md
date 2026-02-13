# Latent Demand Detection System (LDDS)
## Orchestration Architecture

**Orchestrator:** Ren (this agent)
**Purpose:** Model-agnostic multi-agent architecture for sensing invisible market pressures
**Source:** [Agent Architecture Spec](https://ianpilon.github.io/Latent-Demand-Detection-System/agent-architecture-llm.html)

---

## System Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         REN (Orchestrator)                          │
│  • Spawns/manages agents  • Routes LLM calls  • Monitors health     │
└─────────────────────────────────────────────────────────────────────┘
                                   │
        ┌──────────────────────────┼──────────────────────────┐
        ▼                          ▼                          ▼
   ┌─────────┐              ┌─────────────┐            ┌───────────┐
   │ LAYER 1 │              │   LAYER 3   │            │  LAYER 4  │
   │Collectors│────────────▶│   Analyst   │───────────▶│Synthesizer│
   └─────────┘    signals   └─────────────┘  clusters  └───────────┘
        │                          │                          │
        ▼                          ▼                          ▼
   ┌─────────┐              ┌─────────────┐            ┌───────────┐
   │ LAYER 2 │              │   Pattern   │            │  Briefs   │
   │  Store  │◀─────────────│   Match     │            │  Output   │
   └─────────┘              └─────────────┘            └───────────┘
        ▲                                                     │
        │                   ┌─────────────┐                   │
        └───────────────────│   LAYER 5   │◀──────────────────┘
                            │Human Feedback│
                            └─────────────┘
```

---

## Agent Roles

### Layer 0: Data Sources (No Agent — Infrastructure)
**Type:** Scrapers, APIs, RSS feeds, cron jobs
**Requires LLM:** No
**Implementation:** Python scripts, scheduled tasks

| Source Type | Examples | Priority |
|-------------|----------|----------|
| Social | Reddit, Twitter/X, HN | High |
| Reviews | App Store, G2, Capterra | High |
| Trends | Google Trends, GitHub trending | Medium |
| Failures | Product Hunt graveyard, Kickstarter fails | Medium |
| Regulatory | Government feeds, industry news | Low (initially) |

---

### Layer 1: Collector Agents ("Eyes and Ears")
**Requires LLM:** Yes (small/local models sufficient)
**Count:** 4 specialized agents

#### Agent 1: Frustration Scanner
```yaml
role: frustration_scanner
purpose: Classify complaints, workarounds, wishes from social data
input: Social posts, reviews, forum threads
model_tier: small (Claude Haiku, GPT-4o-mini, local Llama)
output_schema:
  signal: enum [workaround, complaint, wish]
  need: string
  source: string
  intensity: float [0.0-1.0]
schedule: Hourly
```

#### Agent 2: Failure Tracker
```yaml
role: failure_tracker
purpose: Identify failed products that addressed real needs
input: Shutdown announcements, failed crowdfunding, app removals
model_tier: medium (reasoning required)
output_schema:
  signal: "failed_attempt"
  domain: string
  reason: string
  need_validated: boolean
schedule: Daily
```

#### Agent 3: Environment Shift Monitor
```yaml
role: environment_monitor
purpose: Detect constraint removals (regulatory, tech, cost changes)
input: News APIs, regulatory feeds, tech announcements
model_tier: medium
output_schema:
  signal: "constraint_removed"
  description: string
  affected_domains: string[]
schedule: Daily
```

#### Agent 4: Trend Spotter
```yaml
role: trend_spotter
purpose: Detect rising behaviors (not words — actions)
input: Search trends, job postings, GitHub activity
model_tier: small + rules
output_schema:
  signal: "rising_behavior"
  pattern: string
  domains: string[]
schedule: Daily
```

---

### Layer 2: Signal Store ("Memory")
**Requires LLM:** No
**Implementation:** SQLite initially, PostgreSQL for scale

```sql
CREATE TABLE signals (
  id TEXT PRIMARY KEY,
  timestamp DATETIME,
  source TEXT,
  type TEXT CHECK(type IN ('frustration','workaround','failed_product','constraint_shift','rising_behavior')),
  domain TEXT[], -- JSON array
  description TEXT,
  intensity REAL CHECK(intensity BETWEEN 0 AND 1),
  raw_data TEXT,
  collector_agent TEXT,
  validated BOOLEAN DEFAULT NULL
);
```

---

### Layer 3: Analyst Agent ("Pattern Finder")
**Requires LLM:** Yes (strongest model — reasoning critical)
**Count:** 1

#### Agent: Convergence Analyst
```yaml
role: convergence_analyst
purpose: Find where 3+ independent signals point to same unmet need
input: All signals from past N days, grouped by domain
model_tier: high (Claude Opus, GPT-4, best available)
key_principle: "Single complaint = noise. 3+ signal types converging = pressure."
output_schema:
  clusters:
    - domain: string
      core_need: string
      signal_count: int
      signal_types: string[]
      intensity: float
      constraint_events: string[]
      uncertainties: string[]
schedule: Daily or Weekly
```

**Prompt Template:**
```
You are analyzing signals of latent market demand.

Signals from past {N} days:
{signals_json}

Tasks:
1. Group signals pointing at the SAME underlying human need
2. For each cluster, assess:
   - Core unmet need (one sentence)
   - Independent signal type count (more = stronger)
   - Any constraint being removed? (timing matters)
   - Pressure intensity (0-1)
3. Only surface clusters with 3+ independent signals
4. State what remains UNCERTAIN

Output: JSON array of pressure clusters
```

---

### Layer 4: Synthesizer Agent ("Briefing Writer")
**Requires LLM:** Yes (medium model)
**Count:** 1

#### Agent: Brief Generator
```yaml
role: brief_generator
purpose: Translate pressure clusters into actionable human briefs
input: Clusters from Analyst
model_tier: medium
output_format: Markdown or structured report
delivery: Email, Slack, dashboard, file
output_schema:
  entries:
    - severity: enum [high, medium, watching]
      domain: string
      signal_count: int
      signal_types: string[]
      core_need: string
      contextual_factors: string[]
      suggested_action: string
      confidence: float
      uncertainty: string
schedule: After Analyst runs
```

---

### Layer 5: Human Feedback Loop
**Requires LLM:** No
**Implementation:** UI or simple forms

| Feedback Type | Description | Effect |
|---------------|-------------|--------|
| Signal Validation | Confirm/reject cluster based on real conversations | Adjusts weights |
| Manual Injection | Add interview insights as first-class signals | High-weight signal |
| Domain Priority | Focus on specific verticals | Filters collector scope |

---

## Orchestration Logic (Ren's Role)

### Responsibilities
1. **Agent Lifecycle** — Spawn, monitor, restart agents
2. **LLM Routing** — Route calls to appropriate model tier
3. **Schedule Management** — Trigger agents on schedule
4. **Error Handling** — Retry failed runs, alert on issues
5. **Brief Delivery** — Send outputs to human
6. **Feedback Integration** — Process human input, update system

### LLM Abstraction Layer
```python
def llm_call(system_prompt: str, user_message: str, model_config: dict) -> str:
    """
    Universal LLM interface — swap models by changing config, not code.
    
    model_config example:
    {
        "provider": "anthropic",  # or "openai", "ollama", "openrouter"
        "model": "claude-3-haiku-20240307",
        "temperature": 0.3,
        "max_tokens": 2000
    }
    """
    # Route to appropriate provider
    pass
```

### Model Allocation Strategy
| Agent | Model Tier | Reasoning |
|-------|------------|-----------|
| Frustration Scanner | Small | High volume, simple classification |
| Failure Tracker | Medium | Needs cause analysis |
| Environment Monitor | Medium | Context understanding |
| Trend Spotter | Small | Mostly pattern matching |
| Convergence Analyst | **High** | Complex reasoning, synthesis |
| Brief Generator | Medium | Structured writing |

---

## Implementation Phases

### Phase 1: MVP (Week 1)
- [ ] Signal Store (SQLite)
- [ ] 1 Collector (Frustration Scanner — Reddit + HN)
- [ ] Analyst Agent (Convergence Analyst)
- [ ] Brief output (file/email)
- [ ] Manual orchestration (Ren triggers via commands)

### Phase 2: Expand Collection (Week 2-3)
- [ ] Add Failure Tracker
- [ ] Add Trend Spotter
- [ ] Add Environment Monitor
- [ ] Automated scheduling (cron)

### Phase 3: Feedback Loop (Week 4)
- [ ] Human validation interface
- [ ] Signal weight adjustment
- [ ] Manual signal injection

### Phase 4: Scale (Future)
- [ ] Dashboard UI
- [ ] More data sources
- [ ] Model fine-tuning based on feedback
- [ ] Multi-domain parallel processing

---

## File Structure
```
ldds/
├── ORCHESTRATION.md          # This file
├── README.md                  # Project overview
├── config/
│   ├── models.yaml            # LLM configurations
│   ├── sources.yaml           # Data source configs
│   └── schedules.yaml         # Agent schedules
├── agents/
│   ├── __init__.py
│   ├── base.py                # Base agent class
│   ├── frustration_scanner.py
│   ├── failure_tracker.py
│   ├── environment_monitor.py
│   ├── trend_spotter.py
│   ├── convergence_analyst.py
│   └── brief_generator.py
├── collectors/
│   ├── reddit.py
│   ├── hackernews.py
│   ├── twitter.py
│   └── ...
├── store/
│   ├── schema.sql
│   ├── db.py
│   └── queries.py
├── orchestrator/
│   ├── scheduler.py
│   ├── llm_router.py
│   └── monitor.py
├── output/
│   └── briefs/
└── tests/
```

---

## Key Design Decisions

### 1. Model-Agnostic
Every agent uses `llm_call()`. No hardcoded models. Swap providers via config.

### 2. Convergence > Volume
- 1 signal = noise
- 3+ independent signal types = investigate
- This prevents false positives

### 3. Human Judgment Required
System surfaces patterns. Humans validate with real conversations.

### 4. Start Simple
MVP is a weekend project. Scale only after proving value.

---

## Next Steps

**Ren will:**
1. Create the repo structure
2. Implement Signal Store (SQLite)
3. Build first Collector (Frustration Scanner)
4. Build Analyst Agent
5. Build Brief Generator
6. Test end-to-end pipeline
7. Iterate based on your feedback

**Awaiting your go-ahead to begin Phase 1.**
