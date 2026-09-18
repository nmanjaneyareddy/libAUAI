# LibAU AI

LibAU AI is a knowledge-base-only reference assistant for the Alliance
University Central Library. It uses Streamlit, BM25 retrieval, the live AU
Library website, local documents, and the Ollama Cloud chat API.

## Knowledge sources

The app loads:

- the approved website seed in `knowledge/urls.txt`;
- linked pages under `https://aulibrary.alliance.edu.in/web/`;
- linked PDF and XLSX documents; and
- PDF, XLSX, XLS, CSV, TXT, and Markdown files placed in `knowledge/`.

The crawler is intentionally bounded to 60 pages and two link levels so a
Streamlit restart cannot crawl the entire website. External webpages are not
recursively crawled. Relevant service or resource links are displayed only
when the exact URL is present in the retrieved knowledge context.

## Repository structure

```text
libAUAI/
├── .streamlit/
│   └── secrets.toml.example
├── knowledge/
│   └── urls.txt
├── .gitignore
├── README.md
├── requirements.txt
└── streamlit_app.py
```

## Deploy on Streamlit Community Cloud

1. Sign in at <https://share.streamlit.io> with GitHub.
2. Create an app from the `nmanjaneyareddy/libAUAI` repository, `main`
   branch, using `streamlit_app.py` as the main file.
3. In **App settings → Secrets**, add:

   ```toml
   OLLAMA_API_KEY = "your_real_ollama_api_key"
   OLLAMA_MODEL = "gpt-oss:120b"
   ```

4. Deploy the app. On the first run, allow time for the website knowledge
   index to load.

Create an Ollama key at <https://ollama.com/settings/keys>. If the default
model is unavailable to your account, replace `OLLAMA_MODEL` with a cloud
model listed by the Ollama API.

## Run locally

Create `.streamlit/secrets.toml` with the same two settings, then run:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
streamlit run streamlit_app.py
```

On Windows PowerShell, activate the environment with:

```powershell
.venv\Scripts\Activate.ps1
```

## Updating the knowledge base

- Edit `knowledge/urls.txt` to add or remove approved public source URLs.
- Add approved local documents inside `knowledge/`.
- In the running app, select **Refresh knowledge base** after changing a
  source. The automatic cache expires after one hour.

Useful validation questions include:

- What are the library timings?
- How can I access JSTOR outside the campus?
- Which online databases are available?
- How can I contact the Central Library?

## Security

Never commit `.streamlit/secrets.toml`, `.env`, API keys, or private key
files. If a key is exposed, revoke it and update the Streamlit secrets.
