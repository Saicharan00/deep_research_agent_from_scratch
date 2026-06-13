# Deep Research Agent from Scratch

A multi-agent deep research system built from scratch with **LangGraph**. The system coordinates parallel research agents to investigate complex topics, then synthesizes their findings into a structured report.

## Overview

This project builds a deep research agent end to end — from scoping a user's request, to delegating sub-topics across parallel researchers, to compressing and writing up the final report. Along the way it uses **LangSmith** for tracing and evaluation.

The pipeline is broken into the major stages of a deep research workflow:

- **Scoping** — Clarify the user's request and turn it into a concrete research brief.
- **Supervision** — A supervisor agent decomposes the brief and delegates independent sub-topics to worker agents.
- **Parallel Research** — Multiple research agents run concurrently, each with an isolated context window, using web search tools to gather information.
- **Synthesis** — Findings are compressed and written into a final structured report.

## Architecture

The system uses a **two-node supervisor pattern** (`supervisor` + `supervisor_tools`) that coordinates worker agents:

- **Supervisor Pattern** — A central coordination agent assigns work to research agents via structured delegation tools.
- **Parallel Research** — Independent topics are researched concurrently using `asyncio.gather()` for true parallelism.
- **Context Isolation** — Each research agent operates in its own context window, keeping sub-topics clean and focused.
- **Research Delegation** — Structured tools let the supervisor assign tasks and decide when research is complete (termination logic).

```
User Request
     │
     ▼
  Scoping  ──►  Research Brief
     │
     ▼
 Supervisor  ──►  delegates sub-topics
     │
     ├──► Research Agent 1 ┐
     ├──► Research Agent 2 ├─ run in parallel (asyncio.gather)
     └──► Research Agent N ┘
     │
     ▼
 Compress & Synthesize
     │
     ▼
 Final Report
```

## Features

- Multi-agent supervisor orchestration with LangGraph
- Parallel sub-agent execution for faster research
- Web search integration (Tavily) for live information gathering
- Optional MCP (Model Context Protocol) tool integration via LangChain MCP adapters
- LangSmith tracing and evaluation harness
- Runs locally with the LangGraph dev server / LangGraph Studio

## Tech Stack

- **LangGraph** — agent orchestration and state graphs
- **LangChain** — model and tool abstractions
- **LangSmith** — tracing and evaluation
- **Tavily** — web search
- **uv** — Python package and environment management

## Getting Started

### Prerequisites

- Python 3.11 or later
- [uv](https://docs.astral.sh/uv/) installed

### Installation

Clone the repository and install dependencies:

```powershell
git clone https://github.com/Saicharan00/deep_research_agent_from_scratch.git
cd deep_research_agent_from_scratch
uv sync
```

### Environment Variables

Create a `.env` file in the project root with your API keys:

```env
# Web search
TAVILY_API_KEY=your_tavily_api_key_here

# Model provider (use whichever you've configured)
OPENAI_API_KEY=your_openai_api_key_here
ANTHROPIC_API_KEY=your_anthropic_api_key_here

# LangSmith tracing & evaluation (optional but recommended)
LANGSMITH_API_KEY=your_langsmith_api_key_here
LANGSMITH_TRACING=true
LANGSMITH_PROJECT=deep_research_from_scratch
```

### Running

Launch the LangGraph dev server to interact with the graph in LangGraph Studio:

```powershell
uv run langgraph dev
```

Or run the notebooks directly:

```powershell
uv run jupyter notebook
```

## Project Structure

```
deep_research_from_scratch/
├── src/                    # Agent implementation (scoping, supervisor, research, synthesis)
├── notebooks/              # Course notebooks built progressively
├── langgraph.json          # LangGraph deployment config
├── main.py                 # Entry point
├── pyproject.toml          # Project dependencies (uv)
├── .python-version         # Pinned Python version
└── .env                    # API keys (not committed)
```

## What I Learned

- Building multi-agent systems with the supervisor architecture in LangGraph
- Coordinating parallel agents with `asyncio.gather()` and managing context isolation
- Designing structured delegation tools and termination logic for a supervisor
- Evaluating agent performance with LangSmith evaluation harnesses
- Deploying and debugging graphs with the LangGraph dev server and Studio