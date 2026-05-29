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
- **Data Validation** — check required fields (firstName, email, projectType, city)
- **Data Enrichment** — add leadId, receivedAt, priority, summary
- **Priority Switch** — route the lead based on priority level
- **Email - High Priority Lead** — seller with budget > 400k
- **Email - Medium Priority Lead** — seller with budget ≤ 400k, or buyer with budget > 400k
- **Email - Standard Lead** — buyer with budget ≤ 400k

## Priority logic

| projectType | budget      | priority |
| ----------- | ----------- | -------- |
| seller      | > 400 000 € | high     |
| seller      | ≤ 400 000 € | medium   |
| buyer       | > 400 000 € | medium   |
| buyer       | ≤ 400 000 € | normal   |
