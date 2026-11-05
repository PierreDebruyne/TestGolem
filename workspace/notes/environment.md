# Environment of the sessions

Checked on 2026-11-05, on the machine that runs the sessions.

- `python3` is the system Python 3.10.12, so there is no `tomllib` (it arrives in 3.11): a TOML configuration needs
  `tomli` on 3.10. `zoneinfo` is there, with the tz database (`Europe/Brussels` loads).
- `python3 -m venv` fails on the system Python: it has no `ensurepip`, because `python3.10-venv` is not installed, and
  installing it needs root (request R3). The product's `energy-forecast/.venv` already exists, on 3.10.12, ignored by
  git. To make it again, use the second install command of the product's README, which works here. pip may warn of
  `unknown 0.0.0 requires packaging==26.0`, which comes from no installed package: `pip check` is clean.
- pyenv also offers 3.9, 3.11, 3.12 and 3.13, with environments of unrelated projects under `versions/*/envs/`: leave
  those alone. Its base interpreters do have `ensurepip`, so to run the README's first install command, use a fresh
  clone in a temporary folder with `PATH="$HOME/.pyenv/versions/3.12.13/bin:$PATH"`. None of them is a 3.10.
- `pytest` and `ruff` on the PATH are pyenv shims that fail under the system Python (`pyenv: pytest: command not
  found`), and `python3 -m pytest` has no module either. Use the product's `.venv/bin/pytest`.
- To check that the tests need no network: `unshare -rn .venv/bin/pytest -q`. That namespace leaves loopback down too,
  so tests with a loopback server need `unshare -rn sh -c 'ip link set lo up && .venv/bin/pytest -q'`.
- To check that the product works on the pip package `tzdata` alone, as on a machine with no system database:
  `PYTHONTZPATH= .venv/bin/pytest -q` (the empty variable stops `zoneinfo` from searching the system folders).
- To test what is committed rather than the working tree, clone the product to a temporary folder and run, from the
  clone, `PYTHONPATH=$PWD/src <product>/.venv/bin/python -m pytest -q`: `PYTHONPATH` takes precedence over the editable
  install, so the clone's package is the one imported.
- Golem's review folder has not held the product so far: M2.2, M2.3 and M2.4 came back with that finding, and their `base`
  says product `none` (R4, R5). A session cannot fix it: check the commit and a test run, and say so in the `summary`.
- numpy, requests, httpx and PyYAML happen to be in the system Python, pandas is not. Declare what the product needs
  and do not count on them.
- PyPI, GitHub and api.open-meteo.com answer. No Prometheus answers on localhost:9090, and no `prometheus` binary is
  installed. A session's permission check refuses to run a binary downloaded from outside, as tried with Prometheus's
  release on 2026-11-05 ("Code from External"), whereas fetching its source as text works. Python packages from PyPI
  have not met this.
- The workspace's `.gitignore` names `energy-forecast/`: without it, `git add .` from the workspace would record the
  product's repository as an embedded gitlink. The product's repository has no remote while R2 is open.
