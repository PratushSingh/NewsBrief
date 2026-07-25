# NewsBrief

A public, responsive Flask news aggregator that turns Australian and
international RSS feeds into a calm, category-first reading experience.
It extracts article text for concise, complete summaries and always links
readers back to the original publisher.

![NewsBrief desktop interface](docs/images/desktop-home.png)

## Why this project

News feeds are fast but often noisy. NewsBrief demonstrates how to
combine concurrent RSS ingestion, defensive web extraction, caching, and
accessible presentation in a small production-oriented Flask application.
No account or demo authentication is required.

## Highlights

- Melbourne, Victoria, Australian national, technology, sports,
  entertainment, business, science and travel coverage
- A distinct weather experience for Melbourne and Victorian regional centres
- Complete-sentence previews extracted from publisher article paragraphs
- Concurrent fetching, URL validation, response-size limits, deduplication,
  UTC sorting, and graceful partial-failure handling
- In-memory TTL cache with manual refresh and observable health metrics
- Responsive, semantic UI with focused empty and error states
- Pytest, coverage, Ruff, mypy, pre-commit, GitHub Actions, Dependabot,
  Docker, non-root runtime, and container health check

## Quick start

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python main.py
```

Open `http://127.0.0.1:5000`. For a production-style local process:

```bash
waitress-serve --listen=*:8000 --call news_aggregator:create_app
```

## Docker

```bash
docker build -t newsbrief .
docker run --rm -p 8000:8000 newsbrief
```

The health endpoint is available at `GET /health`.

## Development

```bash
pip install -r requirements-dev.txt
ruff check .
ruff format --check .
mypy news_aggregator
pytest --cov
pip-audit -r requirements.txt
```

Copy `.env.example` values into your environment to tune timeouts, response
limits, article counts, cache duration, and logging.

## Architecture

```mermaid
flowchart LR
    B["Browser"] --> R["Flask routes"]
    R --> S["FeedService"]
    S --> C["TTL cache"]
    S --> F["RSS publishers"]
    S --> P["Publisher article pages"]
    F --> N["Normalize, validate, deduplicate"]
    P --> X["Extract article paragraphs"]
    X --> Q["Complete-sentence preview"]
    N --> Q
    Q --> T["Jinja templates"]
    T --> B
```

See [Architecture](docs/architecture.md) for request flow, trust boundaries,
and design decisions.

## Screenshots
<img width="1920" height="1029" alt="Screenshot 2026-07-25 22 24 39" src="https://github.com/user-attachments/assets/a353bb24-5f39-4103-b11b-8224ae56a35b" />
<img width="1920" height="1029" alt="Screenshot 2026-07-25 22 24 54" src="https://github.com/user-attachments/assets/b8400b6c-f4ed-40ca-9ac8-0eb8b595f69b" />
<img width="1920" height="1029" alt="Screenshot 2026-07-25 22 25 29" src="https://github.com/user-attachments/assets/047dc613-f743-4a87-ad87-40c5dcf88d7c" />
<img width="1920" height="1029" alt="Screenshot 2026-07-25 22 25 46" src="https://github.com/user-attachments/assets/441029b4-5574-4f05-a9ba-ba01aa39d14b" />


| Weather theme | Mobile layout |
| --- | --- |
| ![Weather category](docs/images/weather.png) | ![Mobile interface](docs/images/mobile.png) |

## Data and editorial scope

The application reads public RSS metadata and publisher pages from configured
sources. Headlines, imagery, and article rights remain with their publishers.
Summaries are extractive previews, not independent reporting; readers can
always open the original story.

## Project policies

- [Contributing](CONTRIBUTING.md)
- [Security](SECURITY.md)
- [Code of Conduct](CODE_OF_CONDUCT.md)
- [MIT License](LICENSE)
