# Environment of the sessions

Checked on 2026-11-05, on the machine that runs the sessions.

- `python3` is the system Python 3.10.12, so there is no `tomllib` (it arrives in 3.11): a TOML configuration needs
  `tomli` on 3.10. `zoneinfo` is there, with the tz database (`Europe/Brussels` loads).
- pyenv also offers 3.9, 3.11, 3.12 and 3.13, with environments of unrelated projects. Do not use those.
- `pytest` and `ruff` on the PATH are pyenv shims that fail under the system Python (`pyenv: pytest: command not
  found`), and `python3 -m pytest` has no module either. Use a virtual environment in the product:
  `python3 -m venv .venv`, then `.venv/bin/pip install ...` and `.venv/bin/pytest`.
- numpy, requests, httpx and PyYAML happen to be in the system Python, pandas is not. Declare what the product needs
  and do not count on them.
- PyPI and api.open-meteo.com answer. No Prometheus answers on localhost:9090, and no `prometheus` binary is installed.
- The workspace's git repository (rooted one folder up) has no `.gitignore`: without one naming the product's folder,
  `git add .` from the workspace would record the product's repository as an embedded gitlink.
