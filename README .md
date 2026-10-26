# 📄 AI Resume ATS Analyzer

Upload a resume (PDF, DOCX or TXT) and get:

- An **ATS score out of 100** with a category breakdown
- **Prioritised improvements** (High / Medium / Low)
- **Missing keywords** (optionally matched against a job description you paste in)
- **Bullet-point rewrites** with stronger action verbs and measurable impact
- A downloadable Markdown report

Built with [Streamlit](https://streamlit.io) and the Google Gemini API (Flash model).

## Scoring rubric

| Category | Max |
|---|---|
| Contact & Formatting | 15 |
| Keywords & Relevance | 25 |
| Work Experience & Impact | 25 |
| Skills | 15 |
| Education & Certifications | 10 |
| Clarity & Language | 10 |

The AI scores each category; the overall score is calculated in code as the sum, so the total always matches the breakdown.
This is an AI estimate, not the output of any specific employer's ATS.

## Project files

```
app.py            # the Streamlit app
requirements.txt  # Python dependencies
README.md         # this file
```

## Run locally

1. Install Python 3.10 or newer.
2. Get a free Gemini API key from <https://aistudio.google.com/apikey>.
3. In a terminal:

```bash
pip install -r requirements.txt
streamlit run app.py
```

4. Paste your API key in the sidebar (or set it as an environment variable `GEMINI_API_KEY` before starting).

## Configuration

| Setting | Where | Purpose |
|---|---|---|
| `GEMINI_API_KEY` | Streamlit secret or environment variable | Your Gemini API key. If missing, the sidebar asks for it. |
| `GEMINI_MODEL` | Streamlit secret or environment variable (optional) | Model to use. Default: `gemini-flash-latest`, which always points to Google's current Flash model. |

Google retires old model versions regularly. If you ever see a "model not found" error, set `GEMINI_MODEL` (or use **Advanced** in the sidebar) to a current model name from <https://ai.google.dev/gemini-api/docs/models>.

For local use you can create `.streamlit/secrets.toml`:

```toml
GEMINI_API_KEY = "your-key-here"
```

**Never commit this file or your key to GitHub.** Add this to a `.gitignore` file:

```
.streamlit/secrets.toml
__pycache__/
.env
```

## Push to GitHub (using the website only)

1. Sign in at <https://github.com> and click **+** (top right) → **New repository**.
2. Name it (for example `resume-ats-analyzer`), choose **Public** (needed for the free Streamlit Cloud tier unless you connect a private repo), and click **Create repository**.
3. On the new repo page click **uploading an existing file**.
4. Drag in `app.py`, `requirements.txt`, `README.md` (and `.gitignore` if you made one). Do **not** upload any file containing your API key.
5. Scroll down, write a commit message such as `Initial commit`, and click **Commit changes**.

## Deploy on Streamlit Community Cloud

1. Go to <https://share.streamlit.io> and sign in with GitHub.
2. Click **Create app** → **Deploy a public app from GitHub**.
3. Pick your repository, branch `main`, and main file path `app.py`.
4. Open **Advanced settings → Secrets** and paste:

```toml
GEMINI_API_KEY = "your-key-here"
```

5. Click **Deploy**. After a minute or two you get a public `*.streamlit.app` link.

If you would rather not hold your own key on the server, skip the secret step: visitors will then be asked to enter their own key in the sidebar.

To update the app later, edit or re-upload the file on GitHub (**Add file → Upload files**, or the pencil icon on a file) and commit. Streamlit Cloud redeploys automatically.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Very little text could be extracted" | Your PDF is a scanned image. Export a text-based PDF or DOCX from Word/Google Docs. |
| "API key was rejected" | Re-copy the key from Google AI Studio and make sure there are no spaces. |
| "Rate limit or quota reached" | The free tier has limits. Wait a minute or try again later. |
| "Model name was not found" | Set a current model in **Advanced** (see Configuration). |
| Build fails on Streamlit Cloud | Check that `requirements.txt` is in the repo root and spelled correctly. |

## Privacy

Resume text is sent to the Gemini API for analysis. Nothing is stored by this app. Don't upload documents you are not comfortable sharing, and review Google's Gemini API data terms for your account type.
