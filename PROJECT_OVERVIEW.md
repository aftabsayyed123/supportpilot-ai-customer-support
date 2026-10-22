# SupportPilot AI — Portfolio Case Study

## Problem

Support teams receive repeated policy questions alongside issues that need account access or human judgment. This demo answers routine questions from indexed support documents and creates a traceable handoff for requests that need an agent.

## Approach

SupportPilot uses two n8n workflows. A document-ingestion flow extracts and splits support content, creates Gemini embeddings, and indexes chunks in Qdrant. The customer-support flow accepts chat messages, classifies intent and priority, retrieves relevant policy passages, and drafts a grounded answer. Requests needing human review create or update a Google Sheets ticket and trigger email notifications.

## What I built

- Connected a website chat interface to an n8n webhook.
- Indexed public Vodafone Ireland support material in Qdrant for semantic retrieval.
- Added Gemini-based embeddings and grounded response drafting.
- Added intent and priority routing with a human-review path.
- Logged and updated demo tickets in Google Sheets.
- Added support and customer email notifications.
- Added conversation reset and tested normal questions, escalations, contact-detail updates, and error handling using fictional data.

## Design choices

- No customer-account, billing, or order lookup: the demo has no connection to those systems.
- Escalate complaints, account-specific requests, and uncertain cases to a person.
- Keep provider credentials in n8n's credential manager; never put them in browser code or GitHub.
- Use fictional data only.

## Status and limitations

This is a working **local demo**, not a deployed service and not an official Vodafone product. Responses may be incomplete or stale, so policy details must be checked against the source material. The repository intentionally does not include workflow exports, credentials, customer records, or private webhook URLs. Cancellation-to-retentions routing is a possible future enhancement, not an implemented feature.

## Possible next improvements

- Add a secrets-reviewed n8n export and screenshots.
- Add a small evaluation set for retrieval accuracy and escalation decisions.
- Deploy the interface and workflow with secure credential handling and access controls.
