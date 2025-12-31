# Gyoshu

Scientific research agent extension for OpenCode. Enables hypothesis-driven research with Python REPL, structured output markers, and Jupyter notebook integration.

## Features

- **Persistent REPL Environment**: Variables persist across research steps
- **Structured Output Markers**: Categorized output for easy parsing (HYPOTHESIS, FINDING, METRIC, etc.)
- **Jupyter Notebook Integration**: Research saved as reproducible notebooks
- **Session Management**: Create, continue, and replay research sessions
- **Scientific Skills**: Built-in patterns for data analysis, experiment design, and scientific method

## Installation

Gyoshu is an OpenCode extension. To install:

> **Note**: The GitHub repository is currently named `VibeSci`. The URLs below will be updated after the repository rename to `Gyoshu`.

### One-click global installation

```bash
# One-click global installation
curl -fsSL https://raw.githubusercontent.com/Yeachan-Heo/VibeSci/main/install.sh | bash

# Or clone and install
git clone https://github.com/Yeachan-Heo/VibeSci.git
cd VibeSci && ./install.sh
```

### Manual Installation

To install Gyoshu manually:

1. Copy the `.opencode/` directory to your project root
2. Or copy to `~/.config/opencode/` for global availability

```bash
# Project-level installation
cp -r .opencode/ /path/to/your/project/

# Global installation
mkdir -p ~/.config/opencode/
cp -r .opencode/* ~/.config/opencode/
```

## Quick Start

```bash
# Start OpenCode
opencode

# Create a research plan
/gyoshu-plan analyze the iris dataset to identify species clusters

# Start research (new session)
/gyoshu-run

# Continue research (preserves REPL state)
/gyoshu-continue now cluster the data using k-means

# Generate report
/gyoshu-report
```

## Commands

| Command | Description |
|---------|-------------|
| `/gyoshu-plan <goal>` | Create a detailed research plan |
| `/gyoshu-auto <goal>` | **NEW** Autonomous research with bounded cycles |
| `/gyoshu-interactive <goal>` | **NEW** Single-cycle interactive mode |
| `/gyoshu-run` | Start a new research session (PLANNER mode) |
| `/gyoshu-continue` | Continue research with preserved REPL state |
| `/gyoshu-repl <query>` | **NEW** Direct REPL exploration |
| `/gyoshu-abort` | **NEW** Graceful abort with state preservation |
| `/gyoshu-report` | Generate comprehensive research report |
| `/gyoshu-replay <sessionId>` | Replay session for reproducibility |
| `/gyoshu-unlock <sessionId>` | Manually unlock a stuck session |

## Research Modes

Gyoshu supports three orchestration modes:

| Mode | Command | Description |
|------|---------|-------------|
| **AUTO** | `/gyoshu-auto` | Autonomous execution with bounded cycles (max 10). Runs until completion, blocked, or budget exhausted. |
| **PLANNER** | `/gyoshu-interactive` | Single-cycle interactive mode. Executes one step, returns control to user with options. |
| **REPL** | `/gyoshu-repl` | Direct REPL access for exploration. More autonomy, can explore tangentially. |

### Mode Selection Guide

- Use **AUTO** when: You have a clear goal and want hands-off execution
- Use **PLANNER** when: You want step-by-step control over research direction
- Use **REPL** when: You need quick exploration or debugging

## Agents

### gyoshu (Primary)
The main orchestrator. Switch to it with Tab. Controls:
- Research workflow
- REPL lifecycle (new vs continue)
- Session management

### jogyo (Subagent)
The research executor. Invoked by planner via @jogyo. Handles:
- Python code execution
- Structured output with markers
- Data analysis and visualization

## Output Markers

Gyoshu uses structured markers for organized output:

### Research Process
- `[OBJECTIVE]` - Research goal
- `[HYPOTHESIS]` - Proposed explanation
- `[EXPERIMENT]` - Procedure
- `[OBSERVATION]` - Raw observations
- `[ANALYSIS]` - Interpretation
- `[CONCLUSION]` - Final conclusions

### Data and Calculations
- `[DATA]` - Data description
- `[SHAPE]` - Dimensions
- `[METRIC]` - Named metrics
- `[STAT]` - Statistics
- `[CORR]` - Correlations

### Insights
- `[FINDING]` - Key discoveries
- `[INSIGHT]` - Interpretations
- `[PATTERN]` - Identified patterns

### Scientific
- `[LIMITATION]` - Known limitations
- `[NEXT_STEP]` - Follow-up actions
- `[DECISION]` - Research decisions

## Skills

Load skills for specialized guidance:

- `scientific-method` - Hypothesis-driven research framework
- `data-analysis` - Data loading, EDA, statistical tests
- `experiment-design` - Reproducibility, controls, A/B testing

## Architecture

```
.opencode/
├── agent/
│   ├── jogyo.md                # Research agent with completion signaling
│   └── gyoshu.md               # Planner with multi-mode orchestration
├── command/
│   ├── gyoshu-plan.md
│   ├── gyoshu-auto.md          # NEW: Autonomous mode
│   ├── gyoshu-interactive.md   # NEW: Interactive mode
│   ├── gyoshu-run.md
│   ├── gyoshu-continue.md
│   ├── gyoshu-repl.md          # NEW: Direct REPL
│   ├── gyoshu-abort.md         # NEW: Graceful abort
│   ├── gyoshu-report.md
│   ├── gyoshu-replay.md
│   └── gyoshu-unlock.md
├── tool/
│   ├── python-repl.ts
│   ├── notebook-writer.ts
│   ├── session-manager.ts      # State machine (modes, goals, budgets)
│   ├── gyoshu-snapshot.ts      # NEW: Session state for planner
│   └── gyoshu-completion.ts    # NEW: Completion signaling
├── lib/
│   └── ...
├── bridge/
│   └── gyoshu_bridge.py
├── skill/
│   └── ...
└── plugin/
    └── gyoshu-hooks.ts
```

## Session Storage

Sessions are stored at `~/.gyoshu/sessions/{sessionId}/`:
- `manifest.json` - Session metadata and execution history
- `notebook.ipynb` - Jupyter notebook with report and code cells
- `artifacts/` - Generated plots and files

## Example Workflow

```python
# In the REPL (executed by @jogyo)

print("[OBJECTIVE] Identify factors affecting iris species classification")

import pandas as pd
from sklearn.datasets import load_iris

iris = load_iris()
df = pd.DataFrame(iris.data, columns=iris.feature_names)
df['species'] = iris.target

print(f"[DATA] Loaded iris dataset")
print(f"[SHAPE] {df.shape[0]} samples, {df.shape[1]} features")

print("[HYPOTHESIS] Petal dimensions are most discriminative for species")

corr = df.corr()['species'].drop('species').abs().sort_values(ascending=False)
print(f"[CORR] Feature correlations with species:")
print(corr)

print(f"[FINDING] Petal length (r={corr['petal length (cm)']:.3f}) most correlated")
print("[CONCLUSION:confidence=0.95] Hypothesis supported - petal dimensions are most discriminative")
```

## Requirements

- OpenCode v0.1.0+
- Python 3.10+ (for match statements in bridge)
- Optional: psutil (for memory tracking)

## License

MIT
