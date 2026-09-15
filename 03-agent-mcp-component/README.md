# Tutorial 3 — Model Context Protocol (MCP)
## August 29 (Saturday) and September 02 (Wednesday)

3A == A **Campus Course Advisor** built twice: once with hand-written tool calling,
then again through MCP. Runs entirely on your local machine.

3B == A **Formulary Assistant** built tools through MCP.

## Files

- `Tutorial_3A_Sat_Aug29_Aneetta.ipynb` and `Tutorial_3B_Wed_Sep02_Aneetta.ipynb` — the tutorials
- `data/course_catalog.csv` — 25 courses with related details
- `data/drug_formulary.csv` — 20 drugs, prices and regulatory status
- `data/drug_interactions.csv` — 9 known drug-drug interactions
- `requirements.txt` — requirements

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate          # Windows: .\.venv\Scripts\activate
pip install -r requirements.txt
```

Windows PowerShell users: `py -m venv .venv` then `.\.venv\Scripts\Activate.ps1`

## Ollama

```bash
ollama pull qwen3.5:4b      # 3.4 GB, supports tool calling
ollama run qwen3.5:4b "hi"  # warm it up, then /bye
```

The model **must** support tool calling. Check with `ollama show qwen3.5:4b`
and look for `tools` in the capabilities. `gemma3` does **not** support tools.

Smaller alternatives: `qwen3.5:2b` (2.7 GB), `llama3.2:3b` (~2 GB).

## Running in VS Code

1. Open **this folder** (not just the file) — the notebook loads `data/` by relative path.
2. Install the **Python** and **Jupyter** extensions if prompted.
3. Open the `.ipynb`, click **Select Kernel** → your `.venv`.
4. Run cells from the top.

## About MCP versions

This notebook uses **MCP 2.x**, where the server class is `MCPServer`.

## Connecting a real external MCP server

The last section connects to the **official fetch server** - a public MCP server that retrieves a web page as markdown.

It has its own dependencies which **conflict with ours** (it needs `mcp` 1.x, this notebook uses 2.x). So we never install it here. We run it as a separate process with `uvx`, which is both the fix and the correct architecture.

```bash
pip install uv                    # one time
uvx mcp-server-fetch --help       # warm the cache BEFORE the session
```


## Disclaimer

Teaching data only. Prices are illustrative; regulatory and interaction data are
simplified for learning. The authoritative source for India is CDSCO:
https://cdsco.gov.in/opencms/opencms/en/consumer/List-Of-Banned-Drugs/

Never use a teaching file for a real clinical, coverage or compliance decision.