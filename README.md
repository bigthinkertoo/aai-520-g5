# AAI-520 Group 5 — Investment Research Agent

Repository: https://github.com/bigthinkertoo/aai-520-g5

## Team contributions

- Hyunju Yu: Student 1 data acquisition, preprocessing and NLP prompt chain.
- Jim McCarthy: Student 2 planner, tool manager, routing, orchestration and memory.
- Glen Salazar: Student 3 specialist analysis, evaluation and reporting.

`AAI520_Integrated_Team.ipynb` brings the components into one notebook. It
preserves Hyunju's manual source controls and intermediate demonstrations.

## Setup

Use Python 3.10+ and select the intended Jupyter kernel. In that kernel:

```python
%pip install numpy pandas matplotlib ipython
# Live providers and model clients:
%pip install requests yfinance feedparser openai anthropic python-dotenv
```

NLTK/VADER is optional; absent packages or the lexicon produce a missing baseline,
not an automatic download. No notebook cell installs packages automatically.

Put credentials in environment variables or a local `.env` excluded from Git:

```dotenv
LIVE=0
USE_MANUAL_SOURCES=0
OPENAI_API_KEY=your_real_key
SEC_USER_AGENT="AAI520 Group 5 your_real_contact_email"
```

Use real values locally; do not commit secrets. Optional alternatives/providers:
`ANTHROPIC_API_KEY`, `NEWSAPI_KEY`, `ALPHAVANTAGE_KEY`. Model names and article
limits are in CONFIG. Set `LIVE=1` to enable external requests and paid LLM use.
The environment takes precedence over an existing `.env` value. Restart the
kernel and Run All after changing modes. Set SYMBOL, PEERS_TEXT and lookback in
the research-input cell. `USE_MANUAL_SOURCES=1` makes `selected_sources()` control
the groups; otherwise the planner selects them.

## Workflow and contracts

1. Normalize inputs and load bounded per-symbol JSON memory.
2. Choose source groups with validated LLM JSON or deterministic routing.
3. Execute registered tools with retry/error envelopes and audit logs.
4. Preprocess news; classify → extract → summarize → digest with four gates.
5. Route the shared DataBundle to earnings/news/market specialists.
6. Add role-specific LLM reasoning or an explicitly labeled rules fallback.
7. Evaluate data and analysis, revise once using feedback, broaden thin news once.
8. Preserve paired snapshots, render the selected report and save reflection/lessons.

Tools use `{ok, source, data, error}` envelopes. DataBundle retains nested
`market`, `fundamentals`, `news`, `filings`, `macro`, `external_sentiment`,
`sources` and `pipeline_meta` fields.
Specialist claims copy exact values and use stable evidence paths. Numeric and
reference checks do not prove every interpretation is correct. Tool selection
uses source groups with fixed within-pipeline ordering, not native LLM tool calls.

## Modes and outputs

- Offline: explicit synthetic providers exercise the real assembler and NLP chain.
- Live: configured external providers; live data is not silently replaced by samples.
- Both: a separate controlled offline example deliberately injects an imperfect
  draft, shows evaluator correction, persists a lesson, and shows the next run
  avoiding that defect. Tables and a chart make the effect visible.

Run results and memory are isolated under `outputs/demo` and `outputs/live`.
Memory persists across runs, is bounded to ten entries per symbol, and assumes
one writer. The controlled example uses fresh temporary memory and restores state.

## Validation and limitations

The revised notebook is validated offline with fresh state. The input notebook
contained prior live outputs; revised LLM specialists still need a configured
live run. Offline checks cover feedback revision, learning across runs, routing,
missing values, invalid plans/tickers, bad references, mismatched claim values,
and propagation of the wider news window. Synthetic scores are not accuracy.

News is snippet based; numeric gates cannot validate all semantic claims, units,
fiscal alignment or missing facts. The recommendation is a classroom heuristic.
SEC values require period/unit review. Large specialist inputs are rejected at
an explicit limit and use a labeled fallback. External APIs may change or fail.

