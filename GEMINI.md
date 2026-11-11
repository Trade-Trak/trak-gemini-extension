# Using Trak

This extension connects Gemini CLI to the user's own Trak account (https://www.trak.co) through the official remote MCP server at https://mcp.trak.co/mcp. Trak is job management software for trade and service businesses in Australia and New Zealand.

Trak tools act on the user's own Trak account with exactly their permissions.
Read tools (`search_*`, `get_*`, `list_*`, `view_job_file`) never change data.
Every `create_*`, `update_*`, `complete_report`, `close_task`, `upload_file_to_job`
and `delete_job_file` changes data.

On first use, authenticate with `/mcp auth trak`. A browser opens on mcp.trak.co
for Trak login and consent. Tokens are stored by Gemini CLI and refreshed
automatically. The user chooses read-only (`trak:read`) or read and write
(`trak:write`) during consent.

## Resolve ids before acting

- Never guess an id. Turn names into ids with search tools first:
  jobs `search_jobs`, people `search_users`, clients `search_clients`,
  contacts `search_contacts`, suppliers `search_suppliers`, quotes `search_quotes`,
  invoices `search_invoices`, report templates `search_report_templates`.
- A job number the user gives (for example "job 50") is the job id; use `get_job`.
- If a search returns several plausible matches, list them and ask which one.
  Do not pick one silently. If nothing matches, say so; do not create a record
  to fill the gap unless the user asks.

## Use the fixed values Trak expects

- Statuses, types, teams and roles are fixed lists. Look them up with
  `list_job_statuses`, `list_job_types`, `list_quote_statuses`,
  `list_material_order_statuses`, `list_purchase_order_statuses`,
  `list_day_docket_statuses`, `list_report_statuses`, `list_booking_statuses`,
  `list_teams` and `list_user_roles`, and pass the exact value or id returned.
- Pass dates as `YYYY-MM-DD`. Turn relative dates ("this week", "Friday") into
  explicit dates and state them in the reply.
- Fill only fields the user gave or that a lookup returned; leave the rest unset.

## Confirm before changing data

- Before any write tool, show what will be created or changed (record, key fields,
  people who may be notified) and wait for the user's yes.
- `update_*` tools overwrite the fields you send (omitted fields stay as they
  are, but list fields such as `additional_contacts` replace the whole list).
  Read the current record first and send only the fields being changed.
- The only delete is `delete_job_file`. Jobs, clients, invoices and other records
  cannot be deleted through the connector; say this is done in the Trak web app
  rather than working around it with updates.
- Invoices cannot be edited after creation; create them from a quote with
  `create_invoice`.

## Common workflows

- Week schedule: `search_job_calendar` for the date range, then
  `search_schedule_bookings` for who is booked.
- Money: `search_invoices` (filter by status), `search_payments` for amounts paid,
  `get_job_profitability` for a job's profit or loss.
- Job detail: `get_job`, `get_job_notes`, `get_job_files`, `list_job_sign_ons`.
- Files: to add a file to a job use `upload_file_to_job`; to attach it to a note,
  task, quote, order, expense, timesheet or day docket, pass it in that tool's own
  `files` (or `upload_ids`) field.

## Reporting results

- Quote ids, numbers, amounts and dates exactly as returned. Do not invent totals,
  owners or due dates.
- After a write, give the new or changed record's id and the key values saved.
- If a tool reports missing write access, explain that the connection is
  read-only and the user can reconnect with write access.
- Trak allows about 4 requests per second per company; avoid large loops.
