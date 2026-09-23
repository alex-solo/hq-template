# Accounts — external services on record

<!-- EXAMPLE — fictional, for reference only — never copied into place. -->

Read by venture agents before asking the founder for any account, key,
or spend. Lists what exists, where its credentials live (a path or
variable name — never a value), and standing spend approvals. A new
signup or approval becomes a line here the same day.

| Service | Status | Used by | Credentials live in | Notes |
|---|---|---|---|---|
| GitHub | exists | all ventures | — | |
| Cloudflare | account; `ferrywatch.pt` registered | ferry-watch | `ventures/ferry-watch/.env` → `CLOUDFLARE_API_TOKEN` | DNS + workers; new domains go here |
| Twilio | account upgraded; one PT number; sender registration done | ferry-watch | `ventures/ferry-watch/.env` → `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN` | registrations take days — start first |
| Fly.io | account, card on file | ferry-watch | Fly dashboard | |

## Standing spend approvals

- Infra up to €25/mo per venture on existing accounts: proceed. Above
  that, or any new paid account: ask — a gated item.
