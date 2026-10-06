# Lead Qualification Assistant

Paste a free-form lead (email, event note, referral, web form) -> AI extracts contact/company info,
scores it 0-100, writes a summary, a recommended next action and a draft reply. Leads are stored in SQL
and shown in Hot / Warm / Cold tables by score.

Stack: React (Vite) + Spring Boot 3 (Java 17) + SQL (H2 file DB by default, MySQL/Postgres optional) + Claude API.

## Run
Backend (port 8080):
    cd backend
    export ANTHROPIC_API_KEY=sk-ant-...      # optional: without it a rule-based fallback is used
    mvn spring-boot:run

Frontend (port 5173):
    cd frontend
    npm install
    npm run dev

Open http://localhost:5173 and click "Load sample lead" -> "Qualify lead".

## API
- POST /api/leads        {"rawText": "...", "source": "WEBSITE|EVENT|REFERRAL|EMAIL"}  -> qualified lead
- GET  /api/leads        all leads, highest score first (optional ?tier=HOT|WARM|COLD)
- GET  /api/leads/{id}
- DELETE /api/leads/{id}

## Scoring tiers
HOT >= 70, WARM 40-69, COLD < 40 (applied server-side, see LeadService).

## Config (backend/src/main/resources/application.properties)
ANTHROPIC_API_KEY, ANTHROPIC_MODEL (default claude-sonnet-5-5). To use MySQL, swap the datasource
properties (commented in the file) and add the MySQL driver to pom.xml. schema.sql shows the table.
