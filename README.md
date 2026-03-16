# SymptoScan (lightweight AI-assisted)

This project is a rules-based symptom checker with an optional, lightweight AI helper layer.

## Local dev

```bash
npm install
npm run dev -- --host
```

Open `http://localhost:5173/`.

## Backend (Supabase Edge Functions)

Existing rules engine:
- `check-symptoms` (rules/scoring + emergency warning)

AI helper endpoints (optional):
- `list-symptoms` (loads the approved symptom list from the database)
- `ai-normalize-symptoms` (free-text → approved labels; local-first; AI fallback only)
- `ai-explain-results` (very short plain-language explanation; never triage/diagnose)
- `ai-next-question` (chooses from pre-approved follow-up questions only)

### Configure OpenAI (optional)

The app works without external AI. To enable OpenAI for the helper endpoints, set these **Supabase function secrets**:

- `OPENAI_API_KEY`
- `OPENAI_MODEL` (optional; default: `gpt-4o-mini`)

### Deploy functions

Use the Supabase CLI to deploy the functions you need, e.g.:

```bash
supabase functions deploy check-symptoms
supabase functions deploy list-symptoms
supabase functions deploy ai-normalize-symptoms
supabase functions deploy ai-explain-results
supabase functions deploy ai-next-question
```

## Safety notes

- Emergency/red-flag logic is enforced in backend code (`check-symptoms`) and is never overridden by AI.
- AI outputs are informational only and never confirm a diagnosis.

