# multi-agent-framework

> Lightweight, production-ready framework for building multi-agent AI systems. Minimal dependencies, maximum flexibility.

## Philosophy

Most agent frameworks are either too heavy (LangChain) or too opinionated (AutoGPT). This framework provides the core primitives needed to build robust multi-agent systems without the overhead.

## Core Concepts

### Agent

```python
from maf import Agent, Tool, Memory

agent = Agent(
    name="researcher",
    model="qwen2.5:14b",        # Local or API
    tools=[Tool.web_search, Tool.code_exec],
    memory=Memory.vector(db="chroma"),
    system_prompt="You are a research specialist...",
)
```

### Task Graph

```python
from maf import TaskGraph, Task

graph = TaskGraph()
recon = graph.add(Task("reconnaissance", agent=scout))
analysis = graph.add(Task("analysis", agent=analyst, depends_on=[recon]))
report = graph.add(Task("report", agent=writer, depends_on=[analysis]))

results = graph.execute(parallel=True)
```

### ACP Protocol

Structured communication between agents:

```
Agent A ──[TASK_ASSIGN]──► Agent B
Agent B ──[STATUS_UPDATE]──► Agent A
Agent B ──[RESULT]──────────► Agent A
Agent A ──[CRITIQUE]──────── ► Agent B  (optional quality loop)
```

## Supported LLM Backends

- **Local**: llama.cpp, Ollama (via OpenAI-compatible API)
- **Cloud**: OpenAI, Anthropic, Mistral, Groq
- **Custom**: Any OpenAI-compatible endpoint

## Installation

```bash
pip install git+https://github.com/Dev-next-gen/multi-agent-framework
```

## Directory Structure

```
multi-agent-framework/
├── maf/
│   ├── agent.py
│   ├── task.py
│   ├── memory.py
│   ├── tools/
│   └── protocols/
├── examples/
│   ├── research_pipeline.py
│   ├── code_review_team.py
│   └── security_audit.py
└── tests/
```

## License

MIT — Léo Camus / [NextGen Labs](https://nextgen-labs.net)
