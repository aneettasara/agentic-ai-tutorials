# Tutorial 4 — Agentic Frameworks: Google ADK and MOYA
## September 12 (Saturday)

## Files

- `Tutorial_4_Sat_Sep12_Aneetta.ipynb` — the tutorial

## Setup

```bash
python3.12 -m venv .venv
source .venv/bin/activate          # Windows: .\.venv\Scripts\activate
pip install ipykernel
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