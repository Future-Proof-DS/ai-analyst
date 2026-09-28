# AI Analyst

An AI data analyst that runs inside Claude Code and answers business questions about Beam's data. Beam is a media app where users browse, listen to and read content on free, monthly and annual plans. You ask a question in plain words; the analyst frames it, profiles the data, analyses it with pandas, checks its own work and hands back a written brief.

## How It Works

1. **Frame** - Pins the question, the decision it informs and the audience
2. **Profile** - Describes each table from the data and writes it down
3. **Analyse** - Writes pandas code that can be rerun, where every number carries a comparison
4. **Validate** - Peer reviews its own work: mechanics first, then reasoning
5. **Deliver** - Writes a brief with the recommendation first and a Checks section

## Setup

### Prerequisites

- Python 3.10 or 3.11
- [Poetry](https://python-poetry.org/docs/#installation)
- [Claude Code](https://docs.claude.com/en/docs/claude-code/overview)

### Installation

1. **Clone the repo**:
   ```bash
   git clone <this-repo-url> ai-analyst
   cd ai-analyst
   ```

2. **Install dependencies**:

   > **Note:** This project uses Poetry for dependency management. If you don't have Poetry installed, you can install it with:
   > ```bash
   > curl -sSL https://install.python-poetry.org | python3 -
   > ```
   > See the [official Poetry documentation](https://python-poetry.org/docs/#installation) for alternative installation methods.

   ```bash
   poetry install
   ```

3. **Start Claude Code**:
   ```bash
   claude
   ```
   Run `claude` from the repo root so it picks up the project's skills, commands and instructions.

## Usage

Ask a question with `/analyst`. Each question gets its own folder under `outputs/` holding the frame, the analysis script, the charts, the review and the brief.

The steps can also be run one at a time with `/frame`, `/profile`, `/analyse`, `/validate` and `/deliver`.

Before touching any data, the analyst reads `knowledge/beam/`, which holds the known quirks of Beam's tables and any corrections it has been given.

## Project Structure

```
ai-analyst/
├── data/                 # Beam's exported tables (CSV)
├── knowledge/beam/       # Known data quirks and corrections
├── helpers/              # Shared Python helpers for analysis scripts
├── outputs/              # One folder per question (not tracked)
├── .claude/              # Skills and commands for Claude Code
└── pyproject.toml        # Dependencies
```

## Data

- `users.csv`: 31,165 rows, one per user
- `subscriptions.csv`: 30,747 rows, one per subscription
- `payments.csv`: 100,914 rows, payments from 2025-01-01
- `trials.csv`: 2,073 rows, one per trial
- `user_activity_metrics.csv`: 998 daily rows of active user counts (dau, wau, mau)

Session-level data is not included.

---

*Future Proof Data Science - Teaching data scientists to optimize workflows with AI*
