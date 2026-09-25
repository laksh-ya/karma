# KARMA

> Knowledge-Aware Reinforced Multi-Agent Framework for Autonomous Financial Investment Decision-Making

KARMA is an agentic investing research system that combines multi-agent analysis, retrieval-augmented generation, and an active knowledge base. It does not just retrieve context for a single run; it stores trade history, market insights, and outcome-driven lessons so later sessions can learn from earlier ones.

## What KARMA Does

- Runs a structured 12-node pipeline with domain analysts, bull/bear researchers, a risk manager, a trader, and explainability.
- Uses an Active Knowledge Base (AKB) with three semantically separated stores: Market Insights, Trade History, and Lessons Learned.
- Injects prior context before analysis, stores decisions after execution, and reflects on realised outcomes once returns are known.
- Pulls market, fundamentals, news, and social data with caching and fallback sources.
- Supports evaluation, result logging, and backtesting workflows.

## Highlights From the Paper

| Area | Summary |
|---|---|
| Framework | Agentic RAG with an AKB agent that enables pre-query, post-learn, and outcome reflection |
| Knowledge stores | Three ChromaDB-backed stores: MI, TH, and LL |
| Agents | 4 domain analysts, bull/bear researchers, research manager, risk manager, trader, explainability, KB updates |
| Models | GPT-4o for synthesis-heavy nodes, GPT-4o-mini for analyst nodes |
| Evaluation | 5-day live intraday window on AAPL, GOOGL, and AMZN |
| Result snapshot | 15/15 decisions aligned with market movement, 2/2 active trades profitable, +$114.26 net P&L |

## Why It Is Different

Most finance-oriented RAG systems are read-only: they retrieve documents, answer a query, and forget the session. KARMA closes that loop. Each trade can become a new training signal for the next one, which makes the system more useful over time rather than merely more verbose.

The core lifecycle is:

1. Pre-query: retrieve historical context from MI, TH, and LL.
2. Post-learn: store the full decision trail after the trade decision is made.
3. Outcome reflection: compare the decision with realised returns and write a lesson back into LL.

## Quick Start

```bash
python -m venv venv
source venv/bin/activate

pip install -r requirements.txt

python main.py
```

If you plan to use the web apps, install the dependencies first and make sure your API keys are available in your environment or `.env` file.

## Run the Apps

```bash
streamlit run apps/app_main.py
streamlit run apps/app_essential.py
streamlit run apps/app_master_eval.py
```

- `app_main.py` provides the full interactive trading workflow.
- `app_essential.py` focuses on the lighter-weight experience.
- `app_master_eval.py` is intended for evaluation runs.

## Repository Layout

```text
karma/
├── main.py
├── apps/
├── data/
├── docs/
├── src/karma/
│   ├── agents/
│   ├── data/
│   ├── evaluation/
│   ├── graph/
│   ├── rag/
│   └── utils/
├── storage/
└── tests/
```

Key modules live in `src/karma/`:

- `agents/` contains the analyst, researcher, risk, trader, and KB logic.
- `graph/` wires the LangGraph workflow.
- `rag/` handles embeddings, knowledge storage, retrieval, and explainability.
- `data/` manages loaders and source fallbacks.
- `evaluation/` contains the backtester.

## Configuration

Primary runtime settings live in `src/karma/config.py`.

- Model/provider configuration.
- Supported tickers and portfolio presets.
- Cache and storage settings.
- Data-source fallback behavior.

## Data and Storage

- Sample and reference datasets live in `data/`.
- Cached runtime artifacts are stored in `storage/cache/`.
- Knowledge base data is stored in `storage/kb/`.
- Results and evaluation outputs are written to `storage/results/`.

## Testing

```bash
pytest tests -q
```

## Project Notes

- The project is designed around explainable, context-aware decision-making rather than automated execution.
- The paper evaluation uses a five-day intraday window as a reproducible baseline, not a profit guarantee.
- The system is model-agnostic and can be configured for different providers through the central config layer.

## Contributing

See `docs/CONTRIBUTING.md` for contribution guidelines.

## Citation

If you use this project in research, please cite the accompanying KARMA paper.
