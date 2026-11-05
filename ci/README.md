# CI for Django on GitHub

1. add any of the `yml` files in this directory to your own repo at the path: `REPO_ROOT/.github/workflows` **NOTE**:
    1. `REPO_ROOT/` is the directory in which the hidden `.git/` directory exists
    2. you will likely need to create `.github` directory and within it, the `workflows/` directory
2. make sure GitHub allows the actions these workflows use. On your repo's GitHub page go to **Settings → Actions → General → Actions permissions** and choose either:
    1. **Allow all actions and reusable workflows** (simplest), or
    2. **Allow OWNER, and select non-OWNER, actions and reusable workflows**, then check **Allow actions created by GitHub** and add `astral-sh/setup-uv@*` to the list of allowed actions
3. make sure the workflows can open issues:
    1. under **Settings → General → Features**, make sure **Issues** is checked
    2. each workflow already asks for `issues: write` in its `permissions:` block, so you normally don't need to change anything else. If issues still aren't being created, check **Settings → Actions → General → Workflow permissions**
    3. if your repo belongs to an organization, an org admin may need to allow these settings first under the organization's **Settings → Actions → General**. A repo can't allow more than its organization does
4. commit and push. The workflows will show up under your repo's **Actions** tab. If a run fails with "action is not allowed", go back to step 2

## Workflows

| file | runs on | opens an issue when |
| --- | --- | --- |
| `django-check.yml` | every push to `main` | `python manage.py check` fails |
| `uv-lock-check.yml` | pushes to `main` that change `pyproject.toml` | `uv.lock` is out of date with `pyproject.toml` |
| `django-migrate.yml` | every push to `main` | `python manage.py migrate` fails on a fresh, empty database |

All three assume a [uv](https://docs.astral.sh/uv/) project with `pyproject.toml`, a committed `uv.lock`, and `manage.py` at `REPO_ROOT/`.