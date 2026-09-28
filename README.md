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
   git clone https://github.com/Future-Proof-DS/ai-analyst.git
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

Session-level data is not included as a CSV; it is reached through the live database (about 4.3 million rows).

## Connecting to the live database

The analyst can also query Beam's database directly through [MCP Toolbox for Databases](https://github.com/googleapis/mcp-toolbox).

1. **Install the Toolbox**. It is a single binary, with no Python or Docker needed. Check the [releases page](https://github.com/googleapis/mcp-toolbox) for the current version.

   macOS:
   ```bash
   brew install mcp-toolbox
   ```
   Or download the binary directly (Apple Silicon shown; use `darwin/amd64` on Intel):
   ```bash
   export VERSION=1.13.1
   curl -L -o toolbox https://storage.googleapis.com/mcp-toolbox-for-databases/v$VERSION/darwin/arm64/toolbox
   chmod +x toolbox
   ```
   Windows (PowerShell, where `curl.exe` is the real curl):
   ```powershell
   $VERSION = "1.13.1"
   curl.exe -o toolbox.exe "https://storage.googleapis.com/mcp-toolbox-for-databases/v$VERSION/windows/amd64/toolbox.exe"
   ```

2. **Add your credentials**. Copy `.env.example` to `.env` and fill in the read-only credentials you were given. Then load them into the shell.

   macOS and Linux:
   ```bash
   set -a && . ./.env && set +a
   ```
   Windows (PowerShell):
   ```powershell
   Get-Content .env | Where-Object { $_ -match '^[A-Z_]+=' } | ForEach-Object { $k, $v = $_ -split '=', 2; Set-Item "env:$k" $v }
   ```

3. **Register the server with Claude Code**. From the repo root, copy `.mcp.json.example` to `.mcp.json` and replace `/path/to/toolbox` in it with where you put the binary. Claude Code fills in the `${POSTGRES_*}` values from the variables you loaded in step 2, so the password never lands in a config file.

   Alternatively, register it with one command. This stores the expanded values, password included, in your user Claude config, and it uses bash variable syntax, so on PowerShell use the `.mcp.json` route instead:
   ```bash
   claude mcp add --transport stdio course-db -e POSTGRES_HOST=$POSTGRES_HOST -e POSTGRES_PORT=$POSTGRES_PORT -e POSTGRES_DATABASE=$POSTGRES_DATABASE -e POSTGRES_USER=$POSTGRES_USER -e POSTGRES_PASSWORD=$POSTGRES_PASSWORD -- /path/to/toolbox --config tools.yaml --stdio
   ```

4. **Check the connection**. `claude mcp list` shows `course-db`. Inside Claude Code, ask it to list the tables. Three tools are available: list the tables, describe a table, and run one read-only SQL statement.

The connection is read-only, and every statement runs in a read-only transaction. The analyst aggregates in SQL and never pulls raw rows. `knowledge/beam/*.yaml` holds the definitions it uses to write correct SQL.

---

*Future Proof Data Science - Teaching data scientists to optimize workflows with AI*
