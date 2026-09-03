# Omnitrainer: Multimodal Customer Service Moderation Trainer

AI-powered content moderation system for customer service interactions at a fictional company called ACME Enterprise. The system performs text, images, video, audio using Google Gemini and Pydantic AI. Gradio provides the UI with Arize Phoenix prviding the Observability & Evaluation.

---

## Quickstart

### 1. Prerequisites
- Python 3.12+
- `uv` package manager (`curl -LsSf https://astral.sh/uv/install.sh | sh`)

### 2. Setup & Environment

All code and commands run from the `starter/` directory:

```bash
cd starter
cp env.example .env
```

Edit `starter/.env` and provide your credentials:

```env
GEMINI_API_KEY="your-gemini-api-key"
USER_API_KEY="udacity"
# (Optional) If using Vocareum proxy:
# GOOGLE_GEMINI_BASE_URL="https://gemini.vocareum.com"
```

Install dependencies:

```bash
uv sync --dev
uv pip install -e .
```

### 3. Execution

#### i. Run Automated Tests

```bash
# Run all 50 unit and integration tests
uv run pytest tests/ -vv

# Or run without calling external APIs:
uv run pytest tests/ -m "not integration" -vv
```

#### ii. Run Moderation Evaluations (Pydantic Evals)

```bash
# Text Moderation Evals (PII, unfriendly, professional)
uv run evals/text/test_cases.py

# Image Moderation Evals (PII, disturbing, low quality)
uv run evals/image/test_cases.py
```

#### iii. Run the Live Application

Start the full stack (FastAPI backend, Arize Phoenix, and Gradio Chat UI):

```bash
uv run multimodal-moderation
```

##### Access the services:

- Gradio Chat UI: http://localhost:7860
- Arize Phoenix Observability: http://localhost:6006
- FastAPI Documentation: http://localhost:8000/docs

#### iv. Exercising the application

1. Open the Gradio UI at `http://localhost:7860`.
2. Good message: Send `"Thank you for calling ACME Enterprise, this is James Bond. How can I help you today?"`
3. Violation message: Send `"Jane Whiner, credit card 4111 1111 1111 1111: I don't care, stop whining and get lost!"` 
4. End Conversation: Click `End Conversation`.
5. Inspect Phoenix Traces: Open `http://localhost:6006` and verify `conversation` (with `session.id`), `chat_turn`, and the `feedback` attribute on the blocked turn.