# Accounts — external services on record

<!-- EXAMPLE — fictional, for reference only — never copied into place. -->

**Accounts are shared, credentials are not.** One founder login per
provider; each venture provisions its own resources inside it (project,
workspace and API key, buckets, domains, `.env`). Nothing is copied from
another venture's `.env`.

Read by venture agents before asking the founder for any account, key,
or spend. A row is fact only when the **Confirmed** column carries a
founder date; anything else is a report from a guide or journal and is
labelled as such.

| Service | Status | Confirmed | Venture resources | Credentials live in | Notes |
|---|---|---|---|---|---|
| GitHub | account | founder 2026-03-02 | one repo per venture | — | |
| Cloudflare | account; `ferrywatch.pt` zone live | founder 2026-03-02 | ferry-watch: zone + scoped API token | `ventures/ferry-watch/.env` → `CLOUDFLARE_API_TOKEN` | each venture its own token |
| Twilio | upgraded; one PT number; sender registration done | founder 2026-03-10 | ferry-watch: subaccount | `ventures/ferry-watch/.env` → `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN` | registrations take days — start first |
| Fly.io | account, card on file | per setup guide — not yet | ferry-watch: app `ferry-watch` | Fly dashboard | |

## Standing spend approvals

- Infra up to €25/mo per venture on existing accounts: proceed. Above
  that, or any new paid account: ask — a gated item.

## Log

- 2026-03-10 — Twilio confirmed live by founder after first delivered SMS.
