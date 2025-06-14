An intelligent documentation crawler and RAG (Retrieval-Augmented Generation) agent built using Pydantic AI and Supabase.

# Install the package
pip install -U crawl4ai

# If you encounter any browser-related issues, you can install them manually:
python -m playwright install --with-deps chromium
playwright install   



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
```

2. Set up environment variables:
   - Rename `.env.example` to `.env`
   - Edit `.env` with your API keys and preferences:
   ```env
   OPENAI_API_KEY=your_openai_api_key
   SUPABASE_URL=your_supabase_url #Go to `Project Settings` and get the URL and Service Key.
   SUPABASE_SERVICE_KEY=your_supabase_service_key
   LLM_MODEL=gpt-4o-mini  # or your preferred OpenAI model
   ```

## Usage

### Database Setup

Execute the SQL commands in `site_pages.sql` to:
1. Create the necessary tables
2. Enable vector similarity search
3. Set up Row Level Security policies

In Supabase, do this by going to the "SQL Editor" tab and pasting in the SQL into the editor there. Then click "Run".
Check the table in the `Table editor` section.


### Crawl Documentation

To crawl and store documentation in the vector database:

```bash
python crawl_pydantic_ai_docs.py
```

This will:
1. Fetch URLs from the documentation sitemap
2. Crawl each page and split into chunks
3. Generate embeddings and store in Supabase

### Streamlit Web Interface

For an interactive web interface to query the documentation:

```bash
streamlit run streamlit_ui.py
```

The interface will be available at `http://localhost:8501`

## Configuration

### Database Schema

The Supabase database uses the following schema:
```sql
CREATE TABLE site_pages (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    url TEXT,
    chunk_number INTEGER,
    title TEXT,
    summary TEXT,
    content TEXT,
    metadata JSONB,
    embedding VECTOR(1536)
);
```

### Chunking Configuration

You can configure chunking parameters in `crawl_pydantic_ai_docs.py`:
```python
chunk_size = 5000  # Characters per chunk
```

The chunker intelligently preserves:
- Code blocks
- Paragraph boundaries
- Sentence boundaries

## Project Structure

- `crawl_pydantic_ai_docs.py`: Documentation crawler and processor
- `pydantic_ai_expert.py`: RAG agent implementation
- `streamlit_ui.py`: Web interface
- `site_pages.sql`: Database setup commands
- `requirements.txt`: Project dependencies


This project uses Crawl4AI (https://github.com/unclecode/crawl4ai) for web data extraction.


<!-- Night Theme (Dark with Neon) -->
<a href="https://github.com/unclecode/crawl4ai">
  <img src="https://raw.githubusercontent.com/unclecode/crawl4ai/main/docs/assets/powered-by-night.svg" alt="Powered by Crawl4AI" width="200"/>
</a>


This project is based on code by [Cole Medin](https://github.com/coleam00/ottomator-agents).
Thanks for the great work!