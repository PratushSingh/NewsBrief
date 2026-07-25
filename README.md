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
<img width="1311" height="874" alt="Screenshot 2026-07-25 22 24 54" src="https://github.com/user-attachments/assets/c937b223-a12e-481b-81da-7db0c426ade3" />
<img width="1306" height="871" alt="Screenshot 2026-07-25 22 25 29" src="https://github.com/user-attachments/assets/da9933d5-3e12-41b8-83ad-8eb027ccffce" />
<img width="1306" height="871" alt="Screenshot 2026-07-25 22 25 46" src="https://github.com/user-attachments/assets/c5e5d86b-5aeb-4c09-b5c2-ba674736b555" />
<img width="1314" height="876" alt="Screenshot 2026-07-25 22 24 39" src="https://github.com/user-attachments/assets/abbe5de8-0836-4223-ac50-4cfa7c8bfa02" />



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
