<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="marka/wintra-crm-logo-koyu-zemin.png">
    <img src="marka/wintra-crm-logo-acik-zemin.png" alt="Wintra CRM by Ascendia" width="420">
  </picture>
</p>

# Wintra CRM

**Wintra CRM by Ascendia, built on Twenty.** A B2B sales CRM built on self-hosted [Twenty](https://twenty.com), the open-source CRM — **without forking Twenty's core**.
Everything is added through Twenty's Metadata API, REST API, webhooks and the Twenty app framework, so Twenty
can be upgraded independently.

Built and maintained by [Ascendia](https://ascendia.com.tr) — B2B sales consulting, training and CRM implementation. Developed against Twenty **v2.57.1** (self-hosted, Docker Compose).

> Code comments, UI labels and commit history are in Turkish; this README is in English.

## What it adds to Twenty

| Area | What you get |
|---|---|
| Sales process | Stages from lead to won/lost/disqualified, stage history, close lock, forecast categories, reporting periods (months, quarters), close-date quarter |
| Accounts | Account status (prospect / customer / former), account owners copied to opportunities, competitor services with renewal reminders, channel and supplier partners |
| Quotes (CPQ) | Product catalog, catalog and quote-specific lines, one-time / monthly / yearly billing, multi-currency with daily FX, P&L per quote (cost, gross profit, margin), channel share, option groups, margin rules with approval, revisions, validity reminders, inflation-based price escalation |
| Documents | Company Word / PowerPoint templates are parsed into sections and placeholders; quotes are rendered to DOCX / PPTX and PDF (LibreOffice) |
| Contracts | Contract and contract documents created from accepted quotes; delivery / invoicing status |
| Targets | Personal and manager targets by period with weighted lines (revenue, gross profit, ARR, MRR), attainment and forecast attainment |
| Follow-up | Touchpoints (call, e-mail, meeting, LinkedIn), days without touch, follow-up tasks via a Twenty workflow, planned-meeting tasks |
| E-mail & calendar | Synced Gmail / Google Calendar messages and meetings become touchpoints; "reply" tasks with response time |
| E-signature | DocuSign envelopes from the quote; status polling; signed PDF moves the opportunity forward |
| Invoicing | Paraşüt (Turkish accounting) invoice creation per billing period and collection status — *implemented, not yet connected to a live account* |
| Dashboards | Twenty app with sales widgets: KPIs, stage distribution, forecast, big deals, target tracking, source & channel, system health |
| Operations | Ubuntu Server setup, HTTPS via Tailscale, hourly backups with restore test, role tests, demo data |

## Repository layout

```
metadata/   Idempotent Python scripts that build the data model (run in numeric order: 01_… → 29_…)
motor/      Calculation engine: Python service (standard library only) that receives Twenty webhooks,
            computes and writes back via REST. Feature packages can be enabled per workspace.
belge/      Document service: reads Word / PowerPoint templates, fills them, converts to PDF
apps/       Twenty app (TypeScript, twenty-sdk): dashboard widgets, quote screen, template editor,
            follow-up workflow, system-health objects
ops/        Server setup, deployment, backups, integration setup, end-to-end tests, demo data
```

## Getting started

Requirements: a self-hosted Twenty v2.57.1 (Docker Compose, in `~/twenty`), Python 3, Docker, Node 24 + Yarn 4 (for the app).

1. Create an API key in Twenty (Settings → APIs) and store it on the server: `~/twenty/.twenty-api-key` (mode 600).
2. Copy this repository to the server (e.g. `~/ascendia-crm`).
3. Build the data model: run `python3 metadata/NN_*.py` in numeric order.
4. Engine and document service: `ops/motor-kur.sh`, `ops/belge-kur.sh`.
5. App: `cd apps/ascendia-pano && corepack yarn install && corepack yarn twenty app apply`.
6. Optional integrations: `ops/google-kur.sh` (Gmail / Calendar), DocuSign and Paraşüt settings files (see the
   package headers in `motor/ascendia_motor/paketler/`), `ops/ai-kur.sh` (Twenty AI chat).
7. Demo data (fictional companies on `.example` domains): `python3 ops/tanitim-verisi.py` on an empty workspace.

Secrets are never stored in the repository: every key is read from a `600` file under `~/twenty/`.

## Status

Working on a self-hosted development instance with demo data.

Wintra CRM is a CRM solution for B2B sales teams developed by Ascendia. It is built on Twenty CRM. Development and support are provided by Ascendia. Ascendia is a member of the Twenty Integrator program. The Twenty name and logo belong to Twenty.

## About Ascendia

[Ascendia](https://ascendia.com.tr) helps B2B teams build a repeatable sales process: sales training, coaching and
CRM implementation on Twenty. Need Wintra CRM set up for your team? Visit **[ascendia.com.tr](https://ascendia.com.tr)**.

## License

MIT — see [LICENSE](LICENSE).
