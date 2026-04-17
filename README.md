# silicoblog
My personal blog

## Developer — pre-commit

To set up pre-commit hooks for this repository, run:

```bash
pre-commit install
# (optional) run all hooks once on the repository
pre-commit run --all-files
```

The repository includes a `.pre-commit-config.yaml` at the project root. The hooks will automatically create isolated venvs and install formatters/linters as needed.
