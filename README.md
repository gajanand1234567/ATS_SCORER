# 🚀 ATS Resume Scorer

An AI-powered web application designed to evaluate how well a resume aligns with a specific job description, providing actionable, data-driven feedback. Built with a FastAPI backend and a Streamlit frontend, this tool leverages spaCy and Sentence Transformers for Natural Language Processing (NLP) and integrates the Groq API to generate intelligent, LLM-powered suggestions.

## 🌐 Live Demo

Try out the live application here: **[ATS Resume Scorer on Streamlit](https://appapppy-fhglaa9ic9hozonryxcgug.streamlit.app/)**

## ✨ How It Works

1. **Upload & Input:** Upload your resume (PDF, DOC, or DOCX) and paste the target job description.
2. **Intelligent Parsing:** The backend parses the resume to extract skills and experience, comparing them to the job description using semantic similarity.
3. **Comprehensive Scoring:** Receive a detailed ATS score broken down by category (Formatting, Keywords, Content, Skill Validation, and ATS Compatibility), alongside actionable LLM-written suggestions for improvement.
4. **History Tracking:** Past analyses are securely saved to your account so you can revisit and review them at any time.

## 🛠️ Tech Stack

*   **Frontend:** Streamlit
*   **Backend:** FastAPI (Python)
*   **NLP:** spaCy (`en_core_web_md`), Sentence Transformers (`all-MiniLM-L6-v2`)
*   **LLM:** Groq API (Llama 3)
*   **Auth & Database:** Supabase (Email/Password and Google OAuth)
*   **PDF Report Export:** WeasyPrint + Jinja2

## 📁 Project Structure

```text
ATS_SCORER/
├── backend/              # FastAPI app, NLP services, API routes
├── frontend/             # Streamlit app, views, components
├── jupyter notebooks/    # Research and dataset prep (not used at runtime)
├── ml model/             # Exported ML artifacts
├── requirements.txt      # Combined backend + frontend dependencies
└── .env.example          # Template for environment variables
```

## 💻 Local Setup & Installation

### 1. Clone and Create a Virtual Environment

```bash
git clone <repo-url>
cd ATS_SCORER
python -m venv venv

# Activate the virtual environment:
# On macOS/Linux:
source venv/bin/activate  
# On Windows: 
venv\Scripts\activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
python -m spacy download en_core_web_md
```

> **Note for Linux Users:** WeasyPrint requires specific system libraries for PDF generation.
> *   **Fedora:** `sudo dnf install -y cairo pango gdk-pixbuf2 libffi`
> *   **Debian / Ubuntu:** `sudo apt install -y libcairo2 libpango-1.0-0 libpangoft2-1.0-0 libffi-dev`

### 3. Configure Environment Variables

Copy the template file to create your local `.env` file:

```bash
cp .env.example .env
```

Open the `.env` file and populate your keys:
*   **Supabase:** You will need `SUPABASE_URL`, `SUPABASE_KEY` (service role), and `SUPABASE_ANON_KEY` from your Supabase Project Settings → API.
*   **Groq API:** Get your key from the [Groq Console](https://console.groq.com).
*   *(Optional)* Configure Google OAuth in the Supabase dashboard if you want to enable Google sign-in.

*Frontend Configuration:* The Streamlit frontend reads Supabase config from `frontend/.streamlit/secrets.toml`. Copy `secrets.toml.example` to `secrets.toml` and fill in the required credentials.

### 4. Run the Backend

Start the FastAPI server from the project root:

```bash
uvicorn backend.main:app --reload --host 0.0.0.0 --port 8000
```
*The API will be available at `http://localhost:8000`*

### 5. Run the Frontend

Open a **new terminal window**, activate your virtual environment, and start Streamlit:

```bash
streamlit run frontend/streamlit_app.py
```
*The app will open automatically at `http://localhost:8501`*

## ⚠️ Important Notes

*   **Security Warning:** Never commit your `.env` or `secrets.toml` files, as they contain sensitive API keys. Ensure they remain listed in your `.gitignore` before pushing any code.
*   **First Run Initialization:** The first time you run the backend, the application will download the Sentence Transformer model (~80 MB). It will be cached for faster subsequent loads.
*   **Running Without Groq:** If you do not have a Groq API key, the core scoring system will still function properly; however, the LLM-generated suggestions section will remain empty.
*   **Development Folders:** The `jupyter notebooks/` and `ml model/` directories are strictly for research, experimentation, and dataset preparation. They are not required to run the live application.
