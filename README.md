# Jthom – Jmom Financial 2026 (Telegram Bot + Google Sheets)

A Telegram bot that records **Deposits** and **Transfers** into Google Sheets, calculates the 5% transfer fee, and shows a monthly report.
It also has a **mini app dashboard** inside Telegram.

## Files

| File | What it is |
|---|---|
| `Code.gs` | The bot (Google Apps Script) |
| `Index.html` | The mini app dashboard page (`/app`) |
| `Jmom_Financial_Simple.xlsx` | The spreadsheet template (2 sheets) |

## Spreadsheet (Jmom_Financial_2026-V2)

- **Transactions** – every deposit and transfer: No, Date, Type, Customer / Note, Amount, Fee, Total, Status, Recorded by. The bot writes here.
- **Monthly** – one row per month. Type each month's **Fee (Expense)** in the yellow cell (column E). Everything else is calculated.

⚠️ Do not rename the sheets – the bot finds them by name.

## Setup

1. Open the Google Sheet → **Extensions → Apps Script**.
2. Paste `Code.gs`. Add an HTML file named **`Index`** and paste `Index.html`.
3. **Project Settings → Script Properties**, add:

   | Property | Value |
   |---|---|
   | `BOT_TOKEN` | Telegram bot token |
   | `WEB_APP_URL` | Web app link ending in `/exec` |
   | `APP_KEY` | Any secret text (protects the dashboard link) |
   | `ALLOWED_CHAT_IDS` | Allowed chat IDs, comma separated |

4. **Deploy → New deployment → Web app** (Execute as: Me, Who has access: Anyone). Copy the URL into `WEB_APP_URL`.
5. Run `setWebhook` once. (Optional: run `setMenuButton` for a Dashboard button.)
6. After every code change: **Deploy → Manage deployments → ✏️ → New version → Deploy**.

## Bot commands

| Command | Example | Does |
|---|---|---|
| `/deposit [amount] [note]` | `/deposit 500 Capital` | Adds a deposit |
| `/transfer [amount] [name]` | `/transfer 100 Dara` | Adds a transfer + 5% fee |
| `/transfer [amount] [name] 0` | `/transfer 100 Dara 0` | Transfer with no fee |
| `/fee [amount]` | `/fee 150` | Sets this month's Fee (Expense) |
| `/fee [amount] [MM-yyyy]` | `/fee 180 10-2026` | Sets another month's Fee (Expense) |
| `/report` | `/report` | This month's report + today's transactions |
| `/report [MM-yyyy]` | `/report 10-2026` | Another month's report |
| `/app` | `/app` | Opens the dashboard |
| `/help` | `/help` | Shows commands and your chat ID |

## How the numbers work

- **Fee (Income)** = 5% of each transfer (0 if sent with `0`)
- **Net Profit** = Fee (Income) − Fee (Expense) for that month
- **Balance** = all deposits − all transfer totals (amount + fee), up to the end of that month
