# Python environment for this workspace

This repository uses a local virtual environment located at `.venv/`.

Activate it (macOS / Linux, zsh/bash):

```bash
source .venv/bin/activate
```

Then install dependencies:

```bash
pip install -r requirements.txt
```

Or use the workspace Python directly:

```
/Users/ziban/github/digital-impact-on-teen-minds/.venv/bin/python env_check.py
```

Files added:

- `requirements.txt` — minimal data/plotting packages
- `env_check.py` — quick script to show interpreter and installed packages
- Updated notebook `digital-impact-on-teen-minds.ipynb` with an env-check cell
