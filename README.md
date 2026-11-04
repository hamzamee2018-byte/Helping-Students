# Imtihan Buddy 📘

A Streamlit app for Pakistani **Matric (9th/10th)** and **Inter (11th/12th)** students.
Upload a chapter from your own textbook and the app will:

1. write a board-style practice paper (MCQs, short and long questions) **only from that chapter**,
2. check your answers against the chapter and give marks,
3. explain mistakes in **English, اردو (Urdu) or Roman Urdu**.

It reads text PDFs, scanned PDFs, Urdu-medium PDFs, phone photos of pages, Word and text files.

## Run locally

```bash
python -m venv .venv && source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .streamlit/secrets.toml.example .streamlit/secrets.toml   # then put your key inside
streamlit run app.py
```

Get an API key at https://console.anthropic.com.

## Put it on GitHub

```bash
git init
git add .
git commit -m "Imtihan Buddy: first version"
git branch -M main
git remote add origin https://github.com/<your-username>/imtihan-buddy.git
git push -u origin main
```

`.streamlit/secrets.toml` is in `.gitignore`, so your key is never uploaded.

## Deploy free on Streamlit Community Cloud

1. Go to https://share.streamlit.io and sign in with GitHub.
2. **Create app** → pick your repo, branch `main`, main file `app.py`.
3. Under **Advanced settings → Secrets**, add: `ANTHROPIC_API_KEY = "sk-ant-..."`
4. Deploy. Share the link with students.

## Settings (environment variables or secrets)

| Name | Default | What it does |
|---|---|---|
| `ANTHROPIC_API_KEY` | none | Your API key. If missing, the sidebar asks for one. |
| `ANTHROPIC_MODEL` | `claude-sonnet-5-5` | Switch to `claude-haiku-5-5` for cheaper runs. |
| `MAX_PAPERS_PER_SESSION` | `10` | Stops one visitor from running up your bill. |

## How it works

- `load_chapter` extracts text from the upload. Scanned and Urdu PDFs (where text extraction gives garbage) are sent as page images instead.
- `generate_paper` asks the model for a JSON paper with model answers and key points taken from the chapter.
- MCQs are marked in the app; written answers are marked in one model call against the chapter's key points, in any language.
- Urdu text is shown right-to-left in the Noto Nastaliq Urdu font.
- The chapter is prompt-cached, so grading and re-explaining a paper costs much less than the first call.

## Ideas for next steps

- Save scores per student (Supabase or Google Sheets) to show weak topics over time.
- A "ask a doubt" chat box on the chapter.
- Past-paper style presets per board and subject.
- Timed mode that mimics the real exam.
