# PRISM: Local AI Digital Twin

PRISM is a privacy-first AI digital twin that runs entirely on your machine. It stores things you tell it as memories, finds the ones most relevant to your question using semantic search, and passes them to a local LLM to generate a personalised answer. No cloud APIs, and no data leaves your device.

## Features

- **Fully local**: chat and embeddings both run through [Ollama](https://ollama.com)
- **Semantic memory retrieval**: memories are embedded with `nomic-embed-text` and ranked by cosine similarity
- **Persistent memory**: add memories from the UI and they are saved to a local JSON file
- **Streamlit UI**: dark-themed chat interface with a quick-actions panel

## How it works

1. You add memories (e.g. "I love robotics"). Each one is embedded into a vector and saved.
2. You ask a question. It is embedded the same way.
3. The top-k most similar memories are retrieved by cosine similarity.
4. The retrieved memories plus your question are sent to Mistral, which generates the answer.

## Tech stack

Python, Streamlit, NumPy, Ollama (Mistral + nomic-embed-text), flat JSON memory store.

## Getting started

### Prerequisites

- Python 3.10+
- [Ollama](https://ollama.com/download) installed and running
- About 5 GB of free disk space for the models

### Setup

```bash
git clone https://github.com/<your-username>/prism-digital-twin.git
cd prism-digital-twin

pip install -r requirements.txt

ollama pull mistral
ollama pull nomic-embed-text
```

### Run

```bash
streamlit run pp4.py
```

Open http://localhost:8501.

On first run, a `memory_store.json` file is created automatically with a couple of starter memories. Add your own from the **Add Memory** box.

## Project structure

| File | Purpose |
|---|---|
| `pp4.py` | Main Streamlit app (UI, memory, retrieval, LLM calls) |
| `prism_demo.py` | Earlier prototypes of the pipeline (bag-of-words to semantic embeddings) |
| `personality.json` | Sample personality profile |
| `memory_store.json` | Your memories and embeddings (generated locally, git-ignored) |

## Troubleshooting

- **"Fallback response" or irrelevant answers**: Ollama is not running or the models are not pulled. The app silently falls back when it can't reach Ollama. Check with `ollama list`.
- **`ollama` not recognized on Windows**: restart your terminal or VS Code after installing.

## Privacy

Your memories stay in `memory_store.json` on your own machine. This file is git-ignored by default so personal data is not committed by accident.

## Roadmap

- Replace the flat JSON store with a proper vector database
- Use personality settings in the prompt
- Memory deletion and editing from the UI
- Deduplicate near-identical memories

## License

Add a license of your choice (MIT is a common default).
