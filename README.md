# 📚 AI Notes Summarizer & Study Assistant

A Streamlit web app that helps students understand study material and prepare for exams. Upload your notes and the app uses Google Gemini (Flash) to summarise them, pull out key concepts, and generate practice questions and flashcards.

## Features

- Upload **PDF, TXT and DOCX** files (up to 5 files, 10 MB each)
- Automatic text extraction with validation (empty, corrupted, password-protected and scanned files are reported clearly)
- **Summaries** in three lengths: Brief, Standard, Detailed
- **Key topics, definitions and concepts**
- **Multiple-choice questions** with correct answers and explanations
- **Short-answer and long-answer questions** with model answers
- **Flashcards** for terms and definitions
- **Ask your notes**: questions answered from your uploaded material
- **Simple-language explanations** for hard concepts (pick the audience level)
- **Downloads** as Markdown (`.md`) or plain text (`.txt`), plus a combined study pack

## Project structure

```
.
├── app.py             # The whole Streamlit application
├── requirements.txt   # Python dependencies
└── README.md          # This file
```

`app.py` is divided into numbered sections (configuration, API key helpers, document reading, prompt builders, Gemini wrapper, output helpers, UI). To change what the AI produces, edit the prompt-builder functions in section 4.

## Requirements

- Python 3.10 or newer
- A free Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey)

## Installation (local)

```bash
# 1. Get the code (or just put the three files in one folder)
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # macOS / Linux
venv\Scripts\activate           # Windows (Command Prompt / PowerShell)

# 3. Install dependencies
pip install -r requirements.txt
```

## API key configuration

The app looks for `GEMINI_API_KEY` in this order:

1. **Streamlit secrets** (recommended)
2. **Environment variable**
3. A key pasted into the sidebar (only kept for that browser session, never saved)

### Option A: Streamlit secrets (recommended)

Create a file `.streamlit/secrets.toml` in the project folder:

```toml
GEMINI_API_KEY = "your-api-key-here"
```

### Option B: Environment variable

```bash
export GEMINI_API_KEY="your-api-key-here"        # macOS / Linux
$env:GEMINI_API_KEY="your-api-key-here"          # Windows PowerShell
set GEMINI_API_KEY=your-api-key-here             # Windows Command Prompt
```

### Choosing a different model (optional)

The default model is `gemini-3.6-flash`. Model names change over time, so you can override it without touching the code by adding this to your secrets or environment:

```toml
GEMINI_MODEL = "gemini-2.5-flash"
```

If you see a "model was not found" message, set `GEMINI_MODEL` to a Flash model listed in your [Google AI Studio](https://aistudio.google.com) account.

> ⚠️ **Never commit your API key.** Keep `.streamlit/secrets.toml` out of Git (see the GitHub steps below).

## Running locally

```bash
streamlit run app.py
```

The app opens at <http://localhost:8501>.

## How to use

1. Upload one or more PDF, TXT or DOCX files.
2. Check the word count and the text preview to confirm the file was read correctly.
3. Adjust output options (summary length, number of questions, etc.) in the sidebar.
4. Open a tab and click its button: Summary, Key Concepts, MCQs, Written Questions, Flashcards, Ask Your Notes, or Simple Explanation.
5. Use the **⬇️ Markdown / ⬇️ Text** buttons under any result, or **Download everything** in the sidebar.

**Limits:** files over 10 MB are rejected. Very long material is trimmed to the first 150,000 characters before it is sent to Gemini (a warning is shown). Scanned/image-only PDFs have no extractable text, because OCR is not supported.

## Pushing to GitHub

1. Create an empty repository at <https://github.com/new> (for example `ai-study-assistant`). Choose Public or Private; do **not** add a README, because you already have one.
2. In your project folder (containing `app.py`, `requirements.txt`, `README.md`), create a `.gitignore` file so secrets are never uploaded:

   ```
   .streamlit/secrets.toml
   .env
   venv/
   __pycache__/
   ```
3. Run:

   ```bash
   git init
   git add app.py requirements.txt README.md .gitignore
   git commit -m "Initial commit: AI Notes Summarizer & Study Assistant"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```

## Deploying on Streamlit Community Cloud

1. Make sure the repository on GitHub contains `app.py`, `requirements.txt` and `README.md`.
2. Go to <https://share.streamlit.io> and sign in with GitHub.
3. Click **Create app** and choose to deploy from an existing GitHub repo.
4. Select your repository, branch `main`, and main file path `app.py`.
5. Open **Advanced settings**, choose a Python version (3.11 or 3.12 works well), and paste your secret into the **Secrets** box:

   ```toml
   GEMINI_API_KEY = "your-api-key-here"
   ```
6. Click **Deploy**. The first build installs `requirements.txt` and takes a few minutes.
7. To update the app later, push changes to GitHub and it redeploys automatically. To change the key afterwards, edit **Settings → Secrets** in the app's menu.

## Troubleshooting

| Problem | What to do |
|---|---|
| "No Gemini API key found" | Add `GEMINI_API_KEY` to secrets or the environment, or paste it in the sidebar. |
| "Gemini rejected the API key" | Re-create the key in Google AI Studio and make sure it has no extra spaces. |
| "Model was not found" | Set `GEMINI_MODEL` to a Flash model available to your key. |
| "Rate limit or quota reached" | Wait a minute; free keys have per-minute and daily limits. |
| "Almost no readable text" for a PDF | The PDF is probably scanned images. Use a text-based PDF or run OCR first. |
| `ModuleNotFoundError` | Activate your virtual environment and run `pip install -r requirements.txt`. |

## Privacy note

Uploaded text is sent to the Google Gemini API to generate results. Do not upload confidential material you are not allowed to share. Files are processed in memory and are not stored by this app.# mudassir-khattak
