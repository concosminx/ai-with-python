An intelligent documentation crawler and RAG (Retrieval-Augmented Generation) agent built using Pydantic AI and Supabase.

# Install the package
pip install -U crawl4ai

# If you encounter any browser-related issues, you can install them manually:
python -m playwright install --with-deps chromium



## Prerequisites

- Python 3.11+
- Supabase account and database
- OpenAI API key
- Streamlit (for web interface)

## Installation

1. Install dependencies (recommended to use a Python virtual environment):
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
playwright install
```

2. Set up environment variables:
   - Rename `.env.example` to `.env`
   - Edit `.env` with your API keys and preferences:
   ```env
   OPENAI_API_KEY=your_openai_api_key
   SUPABASE_URL=your_supabase_url
   SUPABASE_SERVICE_KEY=your_supabase_service_key
   LLM_MODEL=gpt-4o-mini  # or your preferred OpenAI model
   ```

## Usage

### Example

```bash
python insert_docs.py <URL> [--collection mydocs] [--db-dir ./chroma_db] [--embedding-model all-MiniLM-L6-v2] [--chunk-size 1000] [--max-depth 3] [--max-concurrent 10] [--batch-size 100]
```

**Arguments:**
- `URL`: The root URL, .txt file, or sitemap to crawl.
- `--collection`: ChromaDB collection name (default: `docs`)
- `--db-dir`: Directory for ChromaDB data (default: `./chroma_db`)
- `--embedding-model`: Embedding model for vector storage (default: `all-MiniLM-L6-v2`)
- `--chunk-size`: Maximum characters per chunk (default: `1000`)
- `--max-depth`: Recursion depth for regular URLs (default: `3`)
- `--max-concurrent`: Max parallel browser sessions (default: `10`)
- `--batch-size`: Batch size for ChromaDB insertion (default: `100`)

#### Chunking Strategy

- Splits content first by `#`, then by `##`, then by `###` headers.
- If a chunk is still too large, splits by character count.
- All chunks are less than the specified `--chunk-size` (default: 1000 characters).

#### Metadata

Each chunk is stored with:
- Source URL
- Chunk index
- Extracted headers
- Character and word counts

### Running the Streamlit RAG Interface

After crawling and inserting docs, launch the Streamlit app for semantic search and question answering:

```bash
streamlit run streamlit_app.py
```

This project uses Crawl4AI (https://github.com/unclecode/crawl4ai) for web data extraction.


<!-- Night Theme (Dark with Neon) -->
<a href="https://github.com/unclecode/crawl4ai">
  <img src="https://raw.githubusercontent.com/unclecode/crawl4ai/main/docs/assets/powered-by-night.svg" alt="Powered by Crawl4AI" width="200"/>
</a>


This project is based on code by [Cole Medin](https://github.com/coleam00/ottomator-agents).
Thanks for the great work!