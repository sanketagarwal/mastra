# Customer Success Renewal Agent

Catch churn risk before renewal and turn it into an approved follow-up plan.

This template reviews product usage, support history, billing status, and CRM
notes; produces an evidence-backed account assessment; drafts personalized
outreach; waits for CSM approval; and records the approved follow-up in CRM. It
runs immediately in Mastra Studio with bundled demo data and does not send
customer-facing messages automatically.

## Why we built this

Customer risk rarely lives in one system. A usage decline may look harmless
until it is combined with an urgent support ticket, a past-due invoice, and an
approaching renewal. This template shows how Mastra can coordinate those
signals as an ongoing operational workflow instead of producing a one-off
summary.

It demonstrates scheduled workflows, structured output, account-scoped memory,
semantic recall, RequestContext, provider-neutral CRM tools, human approval,
retries, evals, monitoring, and redacted traces.

## Demo

The default demo is intentionally one-click:

1. Open Mastra Studio and select **Workflows**.
2. Open `customer-success-account`.
3. Keep the prefilled demo **Account Id** and click **Run**.
4. Review the generated assessment, plan, and outreach when the workflow pauses
   at `request-csm-approval`.
5. Choose `approved`, enter an approver ID such as `demo-csm`, and resume once.
6. The workflow validates that the approval is still current, then creates an
   internal CRM note and follow-up tasks.

The workflow's steps execute automatically. You do not run the collection,
assessment, planning, approval, or CRM phases individually. They remain visible
in Studio so you can inspect data, retries, timing, and decisions.

The bundled at-risk account is **CS Demo — Redwood Retail**. Its account ID is
`340734348989`, and Studio prefills it automatically.

## Quick start

Requires Node.js 22.13 or newer.

```bash
npx create-mastra@latest customer-success-agent --template customer-success-agent
cd customer-success-agent
npm run dev
```

Open [localhost:4111](http://localhost:4111) and follow the demo above. No API
key or `.env` file is required for the default fixture experience.

If you are working directly from this repository instead of using
`create-mastra`, run `npm install` before `npm run dev`.

## Portfolio reviews

`weekly-customer-success` automatically reviews every configured CRM account on
a schedule. It starts the same isolated account workflow with bounded
concurrency, so one provider failure does not stop the portfolio review.

## Try the other outcomes

Enter one of these account IDs in Studio instead of leaving the field blank:

| Account        | Scenario               | Expected outcome                                  |
| -------------- | ---------------------- | ------------------------------------------------- |
| `340739743463` | Healthy account        | `no_action`                                       |
| `340734348989` | Renewal and churn risk | `awaiting_approval`, then `written` or `rejected` |
| `340737895140` | Missing signals        | `insufficient_data`                               |
| `340878324429` | Provider outage        | retries, then `unknown_retry`                     |

## Connect your systems

The workflow depends on small provider-neutral interfaces for product usage,
support, billing, CRM reads, and CRM writes. Replace those adapters with the
systems your company uses; the workflow and its approval, eval, and monitoring
contracts stay the same.

An included HubSpot adapter demonstrates live company discovery, CRM-note
reads, and approved note/task writes. Product usage, support, and billing remain
fixture-backed in the included HubSpot demo so the integration is safe and
repeatable.

Copy `.env.example` to `.env` and set:

```env
DATA_SOURCE=hubspot
TENANT_ID=your-tenant
HUBSPOT_PRIVATE_APP_TOKEN=your-private-app-token
```

The HubSpot private app must be allowed to read companies, notes, tasks, and
association labels and to create notes and tasks. An approved run writes only
an internal note and follow-up tasks; it does not send the outreach draft.

## Learn more

- [Connectors and model-backed generation](./docs/mastra-primitives.md#composition-and-connector-replacement)
- [Mastra primitives](./docs/mastra-primitives.md)
- [Evals](./docs/evals.md)
- [Monitoring and observability](./docs/monitoring.md)
- [Structured contracts](./customer-success-contracts.md)

## Making it yours

- Replace fixture repositories in `src/mastra/composition/create-composition.ts`.
- Implement the interfaces in `src/mastra/ports/index.ts` for your providers.
- Add sanitized connector fixtures and contract tests.
- Set the review cron, timezone, and maximum account concurrency in `.env`.
- Connect the workflow resume API to your application or a CRM-native approval
  button if CSMs should approve outside Studio.

## About Mastra templates

[Mastra templates](https://mastra.ai/templates) are ready-to-use projects that
show what you can build—clone one, explore it in Studio, and make it yours. They
live in the [Mastra monorepo](https://github.com/mastra-ai/mastra) and are
automatically synced to standalone repositories for easier installation.

Want to contribute? See
[CONTRIBUTING.md](https://github.com/mastra-ai/mastra/blob/main/templates/template-customer-success-agent/CONTRIBUTING.md).
