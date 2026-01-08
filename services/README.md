# Services

Hosted services, scripts, cron jobs, and automation systems that extend Personal OS.

These run externally (Railway, Vercel, local cron, etc.) and enhance your system—whether by pushing data into the vault, exposing APIs, running scheduled tasks, or anything else that needs to run persistently.

## What belongs here

- Webhooks that receive data from external tools
- APIs you expose for integrations
- Scheduled jobs (daily digests, sync tasks, cleanup)
- Background workers
- Hosted utilities

## Example: Call Ingestion Webhook

A service that receives call transcripts from tools like Granola or Fathom, matches attendees to your entities, and saves structured call notes to the vault.

```
Granola/Fathom → POST /calls → Match attendees → Save to brain/calls/
```

The service handles entity lookup and wiki-linking so calls automatically connect to the people and companies in your knowledge base.
