# Métier Guru AI

A full-stack AI career assistant built with a Python FastAPI backend and a React + Vite frontend.

It provides:  
- AI-powered resume analysis from uploaded PDF files
- Career recommendation generation based on skills, experience, and interests
- Learning roadmap creation for a target role

The backend can connect to Azure OpenAI for live AI responses, or run in local mock mode for development.

## Architecture

- `backend/` - FastAPI application that handles PDF parsing, Azure OpenAI integration, and career/roadmap AI endpoints.
- `frontend/` - React + Vite web app for user interaction and file upload.

## Features

1. Resume Analyzer
   - Upload a PDF resume and receive an ATS-style score, detected skills, weaknesses, and recommendations.
2. Career Recommendation
   - Submit skills, experience, and interests to receive personalized career options.
3. Roadmap Generator
   - Generate a structured learning roadmap for a target career role.

## Prerequisites

- Python 3.11+ (recommended)
- Node.js 18+ / npm
- Azure OpenAI account and deployment (optional for live AI)
- MongoDB URI (optional; backend works without it, but analytics logging uses MongoDB)

## Environment Variables

Create a `.env` file in the `backend/` folder or set these variables in your environment.

Required for Azure OpenAI:

```env
AZURE_OPENAI_API_KEY=your_azure_openai_api_key
AZURE_OPENAI_ENDPOINT=https://your-resource-name.openai.azure.com
AZURE_OPENAI_DEPLOYMENT=your_deployment_name
```

Optional:

```env
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/?retryWrites=true&w=majority
DEV_MOCK=true
```

If `DEV_MOCK` is set to `true`, the backend uses local rule-based AI mocks when Azure credentials are missing.

## Setup

### Backend

```bash
cd backend
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Frontend

```bash
cd frontend
npm install
```

## Run Locally

### Start the backend

From `backend/`:

```bash
source .venv/bin/activate
python3 main.py
```

The backend listens on `http://0.0.0.0:8000`.

### Start the frontend

From `frontend/`:

```bash
npm run dev -- --host
```

Open the Vite app in your browser using the address shown in the terminal.

## API Endpoints

### Health

- `GET /` - Basic status check.

### Resume analysis

- `POST /api/resume/analyze`
- Upload a PDF file as form data under `file`.
- Returns structured resume analysis or raw text if parsing fails.

### Career recommendation

- `POST /api/career/recommend`
- JSON body:
  ```json
  {
    "skills": "React, TypeScript",
    "experience": "1 year building frontends",
    "interests": "UI/UX"
  }
  ```

### Roadmap generation

- `POST /api/roadmap/generate`
- JSON body:
  ```json
  {
    "target_role": "Frontend Engineer",
    "timeline_months": 6
  }
  ```

## Notes

- The frontend uses `import.meta.env.VITE_API_URL` for a custom backend base URL. If unset, it defaults to the current host origin.
- The backend supports CORS from any origin for local development.
- If Azure variables are not configured and `DEV_MOCK` is disabled, the backend returns `503` with a clear configuration error.

## Troubleshooting

- `backend/main.py` may log `Service Initialization Warning` if Azure or MongoDB settings are missing.
- Check that `AZURE_OPENAI_API_KEY`, `AZURE_OPENAI_ENDPOINT`, and `AZURE_OPENAI_DEPLOYMENT` are correct for your Azure OpenAI deployment.
- Use `DEV_MOCK=true` during development when Azure credentials are unavailable.

## License

This repository is provided as-is for development and experimentation.
