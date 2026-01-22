# n8n Workflow: B2B Trend Monitoring & Content Engine

This repository versions an **n8n workflow** for the automated detection of relevant
B2B trends, business-impact scoring, and AI-assisted creation of social posts and
newsletter content for mid-market decision-makers.

![n8n](https://img.shields.io/badge/n8n-FF6C37?style=for-the-badge&logo=n8n&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=for-the-badge&logo=google-sheets&logoColor=white)
![Slack](https://img.shields.io/badge/Slack-4A154B?style=for-the-badge&logo=slack&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

---

## Workflow Architecture

![B2B Trend Monitoring & Content Engine](b2b-trend-monitoring-workflow.png)

---

## Contents

- `n8n/workflows/` – exported n8n workflow (JSON)
- `docs/` – architecture diagrams and screenshots
- `README.md` – project documentation

---

## Workflow: B2B Trend Monitoring & Content Engine

### Purpose

- Loads target audience and industry context from Google Sheets
- Generates **decision-oriented search queries** (SEO & GAIO)
- Fetches current B2B news via Google News (SerpAPI)
- Filters irrelevant content (advertorials, PR, outdated articles)
- Scores articles by **business impact**
- Automatically selects the most relevant top story
- Creates AI-generated content:
  - LinkedIn post
  - Instagram post
  - Facebook post
  - Newsletter
  - News summary
  - Headline
- Stores all results in a structured Google Sheet
- Sends status notifications to Slack

---

## Input

### Google Sheets – Target Audience & Context Data

Expected columns:

- `Branche`
- `Zielgruppenrolle`
- `Alter`
- `Ziele der Zielgruppe`
- `Ängste der Zielgruppe`
- `Land/Region der Zielgruppe`
- `Keywords für die Zielgruppe`

Each row represents **one target audience** and is processed fully automatically.

---

## Processing

1. **Schedule Trigger**
   - Weekly execution

2. **Query Generation (OpenAI)**
   - Exactly 5 search queries
   - B2B-focused
   - Decision- and purchase-oriented
   - SEO- and GAIO-ready

3. **News Retrieval (Google News via SerpAPI)**
   - Multiple articles per query

4. **Quality Filtering & Business Impact Scoring**
   - Excludes:
     - Advertorial / sponsored content
     - PR distribution platforms
     - Outdated articles
   - Score range: 0–100

5. **Top Story Selection**
   - Selects the most relevant article

6. **Article Text Extraction**
   - HTML → plain text
   - Fallback if insufficient content is extracted

7. **Content Creation (OpenAI Agent)**
   - Consistent structure:
     - Hook
     - Insight
     - Opinion
     - Call to action
   - No hashtags
   - No emojis
   - No external assumptions

8. **Persistence & Notification**
   - Append results to Google Sheets
   - Slack notification on success or failure

---

## Output

### Google Sheets – Content Data

For each article, **one row** is written with the following columns:

- Title
- News
- Headline
- URL
- Post LinkedIN
- Post Instagram
- Post Facebook
- Newsletter

---

## Import into n8n

1. Open n8n
2. Workflows → Import from File
3. Select the JSON from `n8n/workflows/`
4. Reconnect credentials
5. Configure the Google Sheet
6. Activate the workflow

---

## Dependencies

This repository does not require a `requirements.txt`.

All execution happens inside n8n using built-in runtimes.
External dependencies (Python, Node, etc.) are managed by n8n itself.

---

## Important Notes (Security)

- **Do not commit secrets**
- No API keys or tokens in the JSON
- Credentials must be managed exclusively in n8n:
  - Settings → Credentials
- Placeholders used in the workflow:
  - `GOOGLE_SHEET_ID`
  - `GOOGLE_SHEET_GID_OR_NAME`
  - `SLACK_CHANNEL_ID`

---

## Recommended Repository Conventions

- Branches:
  - `feature/...`
  - `fix/...`
  - `chore/...`

- Commits:
  - `feat: ...`
  - `fix: ...`
  - `chore: ...`
  - `docs: ...`

---

## License

This project is licensed under the MIT License.  
For details, see the [LICENSE](LICENSE) file.
