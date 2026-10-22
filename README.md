# SupportPilot AI — Customer Support Automation

An independent portfolio demo of an AI customer-support assistant built with n8n. It answers policy questions from indexed Vodafone Ireland support material and creates a human-review ticket when a request needs an agent.

> **Practice project only.** This is not affiliated with, endorsed by, or operated by Vodafone. It does not connect to customer accounts, order systems, or billing systems. Use fictional information only.

## What it demonstrates

- **Retrieval-augmented generation (RAG):** indexes public support documents and retrieves relevant passages before drafting a policy answer.
- **Vector search:** stores document embeddings in Qdrant and searches semantically relevant chunks.
- **AI response drafting:** uses Google Gemini embeddings and a Gemini chat model, with retrieved source text grounding the response.
- **Intent and priority routing:** classifies incoming questions and routes requests that need a person to human review.
- **Ticket logging:** records demo handoffs in Google Sheets and updates ticket rows when additional contact details arrive.
- **Notifications:** emails the support team and sends a customer-facing ticket acknowledgement in the demo flow.
- **Website chat demo:** sends chat messages to an n8n webhook and supports starting a fresh conversation.
- **Error handling:** captures workflow failures for diagnosis and tested recovery.

## Architecture

```mermaid
flowchart TD
    A[Website chat] --> B[n8n webhook]
    B --> C[Normalize request]
    C --> D[Classify intent and priority]
    D --> E{Human review needed?}
    E -- No --> F[Retrieve policy chunks from Qdrant]
    F --> G[Draft grounded answer with Gemini]
    G --> H[Return answer to chat]
    E -- Yes --> I[Prepare handoff ticket]
    I --> J[Append or update Google Sheets ticket]
    J --> K[Notify support team by email]
    K --> L[Return ticket acknowledgement]
    M[Public support documents] --> N[Extract and split text]
    N --> O[Create embeddings with Gemini]
    O --> P[Index chunks in Qdrant]
    P --> F
```
## Workflow screenshots

![SupportPilot workflow screenshot 1](./0776B825-17F1-4DC5-9306-4B9D211F2E08.png)

![SupportPilot workflow screenshot 2](./0E3C9476-E81E-4904-B84E-1F3833B53FA7.png)

![SupportPilot workflow screenshot 3](./3A222F79-5B3A-4C1C-8A1F-9DD4B780FA20.png)

![SupportPilot workflow screenshot 4](./CDF4B77F-3E30-4B71-8EC8-FB18FFB66E68.png)

## Tools

- **n8n** — workflow orchestration and webhooks
- **Qdrant** — vector database for policy chunks
- **Google Gemini** — embeddings and response generation
- **Google Sheets** — demo ticket tracking
- **Gmail** — support and customer notifications
- **HTML, CSS, JavaScript** — browser chat interface
- **Docker** — local Qdrant setup

## Example flow

1. A visitor asks a policy question in the chat widget.
2. n8n normalizes the request and classifies its intent and priority.
3. For routine questions, the workflow retrieves relevant policy text from Qdrant and drafts a grounded response.
4. For complaints, account-specific questions, or other requests requiring a person, the workflow creates or updates a demo ticket, notifies support, and returns a handoff acknowledgement.
5. The chat can ask for fictional contact details so the demo ticket contains callback information.

## Run your own demo

This repository documents the project; it does **not** contain the original n8n workflow export or any credentials. To recreate it:

1. Run n8n and Qdrant in a local or hosted environment.
2. Create a Qdrant collection and configure the Gemini embedding model in n8n.
3. Import public support documents that you are permitted to use, extract and split their text, then index the chunks in Qdrant.
4. Build the support workflow shown above and connect Google Sheets and Gmail credentials through n8n's credential manager.
5. Point the chat widget at your own webhook URL. Keep credentials and webhook secrets out of browser code and GitHub.
6. Test with fictional questions and contact information before enabling the workflow for other users.

## Scope and limitations

- Responses are a demonstration and may be incomplete or stale; verify policy details against the source material.
- The demo cannot look up bills, orders, account records, or customer-specific status.
- Ticket storage and email delivery require the owner to configure their own Google credentials and destination sheet.
- The public repository intentionally excludes workflow exports, credentials, customer records, and private webhook URLs. Add a sanitized workflow export only after reviewing it for secrets and personal data.
- Cancellation-to-retentions routing is a possible extension and is not described as implemented here.

## Next steps

- Add a sanitized n8n workflow export and screenshots after a secrets review.
- Add evaluation examples for correct retrieval, escalation decisions, and failure handling.
- Deploy the chat interface and n8n workflow to a secure environment with appropriate access controls.

## Disclaimer

Vodafone and related marks belong to their respective owners. Public support information is referenced only to demonstrate a personal learning project. This repository is not an official Vodafone product or service.
