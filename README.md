# Trak — Gemini CLI extension

Connect [Gemini CLI](https://geminicli.com) to your [Trak](https://www.trak.co) account: look up jobs, schedules, quotes, invoices and timesheets, and create or update records when you ask — straight from the terminal.

Requires a Trak account ([trak.co](https://www.trak.co)).

## Install

```bash
gemini extensions install https://github.com/Trade-Trak/trak-gemini-extension
```

(Replace the URL with the public repo Owen publishes; the path above is the suggested name under the existing `Trade-Trak` GitHub org.)

On first use, authenticate with your Trak credentials:

```
/mcp auth trak
```

A browser window opens on mcp.trak.co for the standard Trak login and consent. You choose read-only or read-and-write access. Tokens are stored locally by Gemini CLI and refreshed automatically — no API key to copy.

## What you can do

- Find jobs by client, status, type, assignee or date, and see notes, files, photos and sign-on history.
- Check the calendar and scheduler: what is on this week and who is booked.
- Look up clients, contacts and suppliers.
- Review quotes, invoices, payments, expenses, material and purchase orders, timesheets and day dockets, and get the profitability report for a job.
- Create and update jobs, tasks, notes, clients, contacts, quotes, invoices (from a quote), orders, expenses, payments, timesheets, day dockets, schedule bookings and completed reports, and upload files to a job.

Write tools ask for confirmation before changing data. The only delete is removing a file from a job.

## How it works

| Field | Value |
| --- | --- |
| Transport | Remote HTTP MCP (`httpUrl`) |
| Endpoint | `https://mcp.trak.co/mcp` |
| Auth | Automatic OAuth 2.1 (RFC 9728 discovery + dynamic client registration / CIMD + PKCE). Scopes `trak:read`, `trak:write`, `offline_access` |
| Context | `GEMINI.md` loaded for every session |

## Privacy

- Connector privacy policy: https://www.trak.co/privacy-policy
- Terms: https://www.trak.co/terms-and-conditions
- Data you access through this extension flows through your own Gemini account under Google's terms — check your account's data settings.
- You can disconnect at any time; unused access expires after 30 days.

## Support

- Docs: https://www.trak.co/integrations/mcp-server
- Email: contact@trak.co
- Publisher: Trak Services Pty Ltd (ABN 43 662 168 943)

## License

MIT — see [LICENSE](./LICENSE).
