# Lead Intake Workflow — n8n

n8n workflow simulating the reception of a real estate lead via webhook, with validation, enrichment and email notification.

## Prerequisites

- Docker
- Docker Compose

## Start the project

```bash
docker-compose up -d
```

n8n will be available at `http://localhost:5678`.

## Import the workflow

1. Log in at `http://localhost:5678`
2. Go to **Workflows → Import**
3. Select the `workflow.json` file

## Test the webhook

```bash
curl -X POST http://localhost:5678/webhook/lead-intake \
-H "Content-Type: application/json" \
-d '{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "source": "website",
  "projectType": "seller",
  "city": "Paris",
  "budget": 450000
}'
```

## Workflow structure

- **Webhook** — receive the lead via POST
- **Data validation** — check required fields
- **Data enrichment** — add leadId, date, priority, summary
- **Priority condition** — branch based on priority
- **Email lead prioritaire / Email lead standard** — send the final email
