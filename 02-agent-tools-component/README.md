# Tutorial 2B - Tools and Function Calling

A drug-formulary assistant that runs **entirely on your own machine**.
No API keys, no rate limits, no data leaving your laptop.

**Teaching data only** — see the disclaimer at the bottom.

## What this tutorial demonstrates

- A "tool" is just an ordinary Python function.
- A local model can *request* a tool call, but **our code runs it**.
- The model answers drug price and regulatory questions from our CSV, not from memory.

## Files

- `Tutorial_2B_Wed_Aug05_Aneetta.ipynb` — the tutorial notebook
- `data/drug_formulary.csv` — 20 drugs with prices and India / USA / UK regulatory status

## One-time setup

You need Python 3 [latest], Ollama, and the packages in `requirements.txt`.

### macOS or Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Windows PowerShell

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### Windows Command Prompt

```bat
py -m venv .venv
.\.venv\Scripts\activate.bat
python -m pip install -r requirements.txt
```

## Ollama setup

Install Ollama from https://ollama.com/download, then pull the model once:

```bash
ollama pull qwen3.5:4b
```

Only **3.4 GB**, so it downloads quickly and runs comfortably on a laptop.

### Choosing a model

The model **must support tool calling**. Check any model with:

```bash
ollama show qwen3.5:4b        # look for "tools" under capabilities
```

These all work for this tutorial:

| Model | Download size | Notes |
|---|---|---|
| `qwen3.5:4b` | 3.4 GB | **Recommended** - small, fast, native tool support |
| `qwen3.5:2b` | 2.7 GB | Smallest option, if your laptop is tight |
| `llama3.2:3b` | ~2 GB | Good at simple tool routing |
| `gemma4:12b` | ~7 GB | The model from Tutorial 3, if you already have it |

**`gemma3` does NOT support tool calling** - do not use it for this tutorial.

To switch models, edit one line at the top of the notebook:

```python
MODEL_NAME = "qwen3.5:2b"
```

### Check Ollama is serving

```bash
ollama list                             # shows your downloaded models
curl http://localhost:11434/api/tags    # should return JSON
```

If nothing responds, start it with `ollama serve`.

## Running in VS Code

1. Open this folder in VS Code (`File > Open Folder`).
2. Install the **Python** and **Jupyter** extensions if prompted.
3. Open the `.ipynb` file.
4. Click **Select Kernel** (top right) → **Python Environments** → the `.venv` you created.
5. Run the cells from the top.

Keep a terminal open with Ollama running while you work.

## Troubleshooting

| Symptom | Fix |
|---|---|
| `ConnectionError` on localhost:11434 | Ollama is not running — start `ollama serve` |
| `model not found` | Run `ollama pull qwen3.5:4b` |
| Model answers but never calls a tool | Run `ollama show <model>` — if `tools` is missing, switch models |
| Very slow responses | Use a smaller model, e.g. `llama3.2:3b` |
| `FileNotFoundError` on the CSV | Open the *folder* in VS Code, not just the file, so the working directory is right |


## Speed

Qwen models "think" before answering, which can take **minutes** on a laptop
without a GPU. Every request in this notebook already sends `"think": False`
to switch that off.

Other things that help:

1. **Warm the model up before you start** - the first call loads 3.4 GB into
   memory. Run this once in a terminal and leave Ollama running:

   ```bash
   ollama run qwen3.5:4b "hi"
   ```

2. **Use a smaller model** if it is still slow:

   ```python
   MODEL_NAME = "qwen3.5:2b"     # 2.7 GB
   MODEL_NAME = "llama3.2:3b"    # ~2 GB, no thinking mode at all
   ```

3. **Check whether it is using your GPU** - run `ollama ps` while a request is
   in flight. If `PROCESSOR` says `100% CPU`, expect slow responses.

## Disclaimer

Practice data for learning only. Drug names and regulatory status are compiled
from published regulatory actions, but **prices are illustrative teaching
values**, and regulatory lists change constantly. The authoritative source for
India is CDSCO: https://cdsco.gov.in/opencms/opencms/en/consumer/List-Of-Banned-Drugs/

Never use a teaching file for a real coverage, clinical or compliance decision.
