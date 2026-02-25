# Directory Structure

This document describes the current repository layout for SELENE.

The project uses an industry-standard **src-layout** where all Python source code lives under `src/selene/`, cleanly separated from tests, scripts, data, and documentation.

## Current Structure

```
selene/
├── app.py                          # Thin entry point (launches src/selene/ui/app.py)
│
├── src/selene/                     # 📦 Installable Python package
│   ├── __init__.py
│   ├── __main__.py                 # python -m selene
│   ├── settings.py                 # Paths, model config, cache TTLs
│   ├── constants.py                # Shared constants
│   ├── config.py                   # Config management
│   │
│   ├── core/                       # Core business logic
│   │   ├── __init__.py
│   │   ├── med_logic.py            # RAG + LLM orchestration
│   │   ├── context_builder.py      # User context for chat
│   │   ├── context_builder_multi_agent.py  # User context for reports
│   │   ├── deterministic_analysis.py       # Stats/patterns/risk
│   │   └── insights_generator.py   # Clinical report generation
│   │
│   ├── storage/                    # Data access layer
│   │   ├── __init__.py
│   │   ├── data_manager.py         # Persistence (profile, pulse, notes)
│   │   └── chat_db.py              # Chat history DB (Chroma)
│   │
│   └── ui/                         # Streamlit UI components
│       ├── __init__.py
│       ├── app.py                  # Main Streamlit app
│       ├── onboarding.py           # First-run onboarding flow
│       ├── navigation.py           # Sidebar navigation
│       ├── styles.py               # Custom CSS/styling
│       └── views/
│           ├── __init__.py
│           ├── home.py             # Landing / dashboard
│           ├── pulse.py            # Daily Attune logging
│           ├── chat.py             # RAG chat interface
│           └── clinical.py         # Insights & PDF reports
│
├── tests/                          # 🧪 Test suite (pytest)
│   ├── __init__.py
│   ├── test_deterministic_analysis.py
│   ├── test_context_builder.py
│   └── test_med_logic_cache.py
│
├── scripts/                        # 🛠️ Utility scripts
│   ├── setup_project.sh            # One-step project setup
│   ├── update_kb_chroma.py         # Chroma KB import/export
│   └── pdf_processor_medgemma.py   # PDF → KB ingestion
│
├── docs/                           # 📚 Documentation
│   ├── README.md
│   └── technical_reference.md      # Detailed architecture guide
│
├── examples/                       # 💡 Example usage
│   ├── README.md
│   └── basic_usage.py
│
├── data/                           # 📊 Data directories (mostly gitignored)
│   ├── metadata/
│   │   └── stages.json             # Stage definitions (committed)
│   ├── papers/                     # Reference documents (committed)
│   ├── models/                     # Model storage (gitignored)
│   ├── output/                     # Generated outputs (gitignored)
│   ├── reports/                    # Generated reports (gitignored)
│   └── user_data/                  # User data (gitignored)
│       ├── user_profile.json
│       ├── pulse_history.json
│       ├── backups/
│       └── user_med_db/            # Chroma collections
│
├── .github/                        # CI/CD workflows, issue/PR templates
├── .streamlit/                     # Streamlit configuration
├── pyproject.toml                  # Build config & metadata
├── requirements.txt                # Dependencies
├── README.md                       # Main project documentation
├── QUICKSTART.md                   # Contributor quick-start
├── CONTRIBUTING.md                 # Contribution guidelines
├── MIGRATION_GUIDE.md              # Migration history
├── DIRECTORY_STRUCTURE.md          # This file
├── CHANGELOG.md
├── ROADMAP.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── LICENSE
└── CITATION.cff
```

## Package Layout

All source code is under `src/selene/` following the [src-layout convention](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/).

### Install & Run

```bash
# Editable install (recommended for development)
pip install -e ".[dev]"

# Run the app
streamlit run app.py

# Run as Python module
python -m selene

# Run tests
python -m pytest tests/ -v
```

### Import Examples

```python
from selene.core.med_logic import MedLogic
from selene.core.deterministic_analysis import DeterministicAnalyzer
from selene.core.context_builder import ContextBuilder
from selene.storage.data_manager import DataManager
from selene.storage.chat_db import ChatDB
from selene.ui.app import main
from selene.settings import Settings
```

## Key Design Decisions

- **`app.py` at root** - thin entry point (`from selene.ui.app import main; main()`) so `streamlit run app.py` works naturally from the repo root.
- **`src/selene/core/`** - all business logic with no UI imports; testable in isolation.
- **`src/selene/storage/`** - persistence layer (JSON files + Chroma); separated from core for clear boundaries.
- **`src/selene/ui/`** - Streamlit-specific code; views are further isolated in `ui/views/`.
- **`data/`** - runtime data stays out of the package tree; paths configured in `settings.py`.
- **`scripts/`** - standalone utilities not part of the installed package.

## Resources

- [Python Packaging Guide - src layout](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/)
- [pytest good practices](https://docs.pytest.org/en/stable/explanation/goodpractices.html)
