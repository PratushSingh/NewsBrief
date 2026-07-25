# Contributing

Thank you for improving NewsBrief.

1. Open an issue for substantial behaviour or source changes.
2. Create a focused branch from `main`.
3. Install `requirements-dev.txt` and run `pre-commit install`.
4. Add or update tests without making them depend on live publisher networks.
5. Run `ruff check .`, `ruff format --check .`, `mypy news_aggregator`, and
   `pytest --cov`.
6. Open a pull request that explains the outcome, verification, and visual
   changes.

Feed changes should use reputable Australian or international English-language
publishers, match the selected category, and preserve publisher attribution.
Do not add paywall bypasses, copied full articles, credentials, or tracking.
