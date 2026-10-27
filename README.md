# 📄 ATS Resume Analyzer

Upload a resume and get an **estimated ATS score**, a category breakdown, missing keywords, and prioritised, actionable improvements. Built with **Streamlit** and the **Groq API**.

## Features

- Upload **PDF, DOCX or TXT** resumes (up to 5 MB)
- Overall ATS score (0–100) calculated from 5 weighted categories:
  Keywords & Skills (25%), Experience & Impact (25%), Formatting & Structure (20%), Section Completeness (15%), Language & Clarity (15%)
- Optional **job description** field to measure how well the resume matches a specific role
- Strengths, weaknesses, missing keywords, and high/medium/low priority fixes
- Before/after example rewrites (with placeholders, so nothing is invented)
- Rule-based quick checks: email, phone, LinkedIn, GitHub, word count, quantified results, sections
- Download the full report as Markdown

> **Note:** The score is an AI estimate, not the output of a real ATS. Real systems differ, and text extraction cannot see visual layout (columns, text boxes, graphics), so keep your resume design simple.

## Project structure

```
ats-resume-analyzer/
├── app.py            # Streamlit app
├── requirements.txt  # Python dependencies
└── README.md
```

## Run locally

1. **Get a free Groq API key** at <https://console.groq.com/keys>.
2. **Install dependencies** (Python 3.9+; 3.11 or 3.12 recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate        # Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. **Provide your key** using ONE of these options:
   - Paste it in the app's sidebar when it runs (simplest), or
   - Set an environment variable:
     ```bash
     export GROQ_API_KEY="gsk_..."          # Windows PowerShell: $env:GROQ_API_KEY="gsk_..."
     ```
   - Or create `.streamlit/secrets.toml`:
     ```toml
     GROQ_API_KEY = "gsk_..."
     ```
4. **Start the app:**
   ```bash
   streamlit run app.py
   ```
   It opens at <http://localhost:8501>.

## Deploy on Streamlit Community Cloud

1. Push this project to a **GitHub repository** (`app.py`, `requirements.txt`, `README.md` in the repo root).
2. Go to <https://share.streamlit.io> and sign in with GitHub.
3. Click **Create app** → **Deploy a public app from GitHub**.
4. Choose your repository, branch `main`, and main file path `app.py`.
5. Open **Advanced settings**, select Python 3.11 or 3.12, and paste this in the **Secrets** box:
   ```toml
   GROQ_API_KEY = "gsk_your_key_here"
   ```
6. Click **Deploy**. The first build takes a couple of minutes.

## Security

- **Never commit your API key** to GitHub. Keep it in Streamlit Secrets, an environment variable, or the sidebar field.
- If you create `.streamlit/secrets.toml` locally, add `.streamlit/secrets.toml` to a `.gitignore` file.
- If a key is ever exposed, delete it at <https://console.groq.com/keys> and create a new one.
- Resumes contain personal data. This app does not store uploads, but their text is sent to Groq for analysis.

## Troubleshooting

| Problem | Fix |
|---|---|
| "API key was rejected" | Re-copy the key from the Groq console; make sure there are no extra spaces or quotes. |
| "Rate limit reached" | Wait a minute, or switch to the faster 8B model in the sidebar. |
| "Almost no text could be extracted" | The PDF is probably a scanned image. Export a text-based PDF or DOCX. |
| Model not found error | Models change over time. Update the `MODELS` dictionary in `app.py` using <https://console.groq.com/docs/models>. |
| Build fails on Streamlit Cloud | Confirm `requirements.txt` is in the repo root and the file names are spelled correctly. |

## Customising

- **Models:** edit the `MODELS` dictionary at the top of `app.py`.
- **Scoring weights:** edit `WEIGHTS` (the overall score is computed in Python from the category scores).
- **Review criteria:** edit `SYSTEM_PROMPT`.

## License

MIT: use it however you like.
