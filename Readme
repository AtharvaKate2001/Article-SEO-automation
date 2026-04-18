# Article Publishing + SEO Automation System

## Project Overview
This is a full-stack automation system. It lets you submit articles through a web form, stores them in Google Sheets, and automatically runs SEO analysis using OpenAI every 2 minutes.

## Live URLs
- Frontend: https://project-pxfpr.vercel.app
- Google Sheet: https://docs.google.com/spreadsheets/d/1ZyeKqQVFjnF2ocSGwhVbPYWGBeQ6yr-RwvTPTAe_2d4
- n8n runs locally on Docker at localhost:5678

---

## How to Test the System

Step 1 — Open the frontend at https://project-pxfpr.vercel.app

Step 2 — Fill in a title and some content (both are required), then hit Publish Article

Step 3 — You'll get back an article_id confirming it was saved

Step 4 — Wait about 2 minutes for the SEO workflow to pick it up

Step 5 — Open the Google Sheet and check the Articles tab. The seo_status column should change from pending to done

Step 6 — Check the SEO_Results tab to see the generated keywords, meta title, meta description, and ranking insights

---

## Sending Data to the Webhook Directly

POST http://localhost:5678/webhook/ingest-article
Content-Type: application/json
{
"title": "Your Article Title",
"content": "Article content goes here",
"category": "Technology",
"author": "Atharva",
"tags": "AI, SEO, automation"
}


---

## How It Works

There are two n8n workflows:

**Workflow 1 — Article Ingestion**
When someone submits the form, the webhook receives the data, validates that title and content are present, generates a UUID for the article, and saves everything to the Articles sheet with seo_status set to pending.

**Workflow 2 — SEO Processing**
This runs every 2 minutes. It picks up any articles where seo_status is pending, sends the title and content to OpenAI, parses the response, saves the SEO data to SEO_Results, and updates the article status to done.

---

## Idempotency

Every article gets a unique UUID when it's submitted so there are no duplicates. Workflow 2 only touches rows that are still pending, so running it multiple times won't reprocess articles that are already done. If OpenAI fails for some reason, the status gets set to failed and the error is stored so you can see what went wrong.

---

## How Ranking Insights Work

The OpenAI prompt asks for a structured JSON response with primary keywords, long tail keywords, a meta title under 60 characters, a meta description under 155 characters, and a ranking_insights object that looks like this:

```json
{
  "primary_target": "main keyword",
  "high_opportunity": ["easier keywords to rank for"],
  "high_competition": ["harder keywords"],
  "notes": "quick strategy note"
}
```

---

## Files
- index.html — the frontend form, deployed on Vercel
- Workflow 1 json.txt — n8n ingestion workflow
- Workflow 2 json.txt — n8n SEO processing workflow
- README.md — this file
