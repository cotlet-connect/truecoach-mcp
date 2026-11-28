# TrueCoach API integration and MCP server: connect TrueCoach to ChatGPT, Claude and any AI agent

TrueCoach API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in TrueCoach account: clients, programs, workouts, results, messages, check-ins and payments, on the same screens your login opens. No developer and no TrueCoach API key needed. Cotlet is independent and is not affiliated with TrueCoach.

**Does TrueCoach have an API or an MCP server?** TrueCoach has no public API (checked 23 Nov 2026); the API key in Settings, Integration only connects its Zapier app. The Zapier app has four triggers (Added to Client Group, New Client, Workout Created, Workout Completed) and one action, Create Client. It has no Make or n8n app, and TrueCoach has no MCP server and no ChatGPT or Claude app. Payments through TrueCoach cost a flat 5% per transaction from 7 Jan 2026.

## What personal trainers and coaches ask their AI to do in TrueCoach

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Load a program | "Load this 12-week program from my spreadsheet into TrueCoach for Sarah, starting Monday. Show me before you save." | When you write a program |
| Copy a program to many clients | "Copy the Spring Strength program to these 30 clients, starting next Monday." | When a group starts |
| Shift a week | "Sam is traveling. Move all his workouts this week to next week." | When plans change |
| Who is falling behind | "Read this week's workout results and list clients who missed sessions or stopped logging." | Weekly |
| Messages you missed | "List client messages and workout notes from the last 3 days I have not answered, and draft a reply for each." | Daily |
| Weekly check-ins | "Send this week's check-in questions to my clients and put their answers in one table. Show me the message first." | Weekly |
| What the 5% fee costs | "Total last month's payments through TrueCoach and the 5% fee, and compare it with Stripe's standard fee." | Monthly |
| Workout history out | "Export Sarah's workouts with the results she entered to a spreadsheet." | When a client asks or leaves |
| Weekly backup | "Download my programs, client list and check-ins as a backup." | Weekly |
| New clients from your form | "Add the people from my Instagram bio form as TrueCoach clients with the starter program." | Your own endpoint, called from your form tool or Zapier |
| Moving to another app | "Export all my programs and client history before I move to Everfit or Trainerize." | Once |

Your AI asks you before it sends, books or changes anything. Payments stay in your hands: Cotlet refuses payments.

## Your own TrueCoach API, built with Cotlet

TrueCoach gives you no public API; its Zapier app only creates clients. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, Gmail, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `workout-results` | completed workouts with results per client for a period |
| `missed-workouts` | clients who missed scheduled workouts in a period |
| `unanswered-messages` | client messages and notes with no reply, with time |
| `payments-for-period` | payments with the processing fee for a period |
| `clients-export` | clients with contact details, group and program |
| `assign-program` | assigns a program to the clients you name from a start date (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/workout-results \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=truecoach

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to TrueCoach once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its TrueCoach tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"truecoach":{"command":"uvx","args":["truecoach-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t truecoach-mcp .
docker run -i --rm -e COTLET_API_KEY truecoach-mcp
```

Tools: `truecoach_guide`, `truecoach_status`, `truecoach_open`, `truecoach_read`, `truecoach_click`, `truecoach_type`, `truecoach_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install truecoach-mcp        # or run it once: uvx truecoach-mcp connect
truecoach-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 truecoach_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 truecoach_agent.py status    # is TrueCoach ready in your Cotlet.dev account
python3 truecoach_agent.py guide     # TrueCoach screens and sign-in link
python3 truecoach_agent.py read      # open TrueCoach, read the page, close the browser
python3 truecoach_agent.py tools     # list Cotlet.dev tools
python3 truecoach_agent.py serve     # start the stdio MCP server (same as no argument)
```

## What you can ask your AI to do in TrueCoach

These are real jobs TrueCoach users asked about. Cotlet lets ChatGPT, Claude or any MCP client do them in your own signed-in TrueCoach account.

### Programs and workouts

#### Can ChatGPT write a 12-week program for a client and load it into TrueCoach for me?

Yes. TrueCoach has a Programs tab and a workout calendar, and you fill them by hand. With Cotlet, ChatGPT or Claude writes the program, or takes yours, and loads it into the client's calendar in your signed-in TrueCoach account. Nothing is saved until you say yes.

Ask your agent: "Write a 12-week strength program for this client and load each workout into her TrueCoach calendar. Show me week one first."

#### Can an AI move or shift a whole week of workouts, or copy a program to 30 clients, in TrueCoach?

Yes. TrueCoach lets you copy, paste and drag workouts and sync program changes, one selection at a time. With Cotlet, your agent repeats the same move or copy for each client you name. Nothing changes until you say yes to the list.

Ask your agent: "Copy this week's workouts for Jess into next week and move her rest day to Sunday. Show me the changes first."

#### How do I bulk import clients and their starting programs into TrueCoach from a spreadsheet?

Partly. TrueCoach's bulk import is done by its support from your CSV, and the programs must exist first. With Cotlet, your agent can add clients one by one in your TrueCoach account from your spreadsheet and assign each a program. Nothing is created until you say yes.

Ask your agent: "Take the clients in this spreadsheet and add each to TrueCoach with email and group. Show me the first five before you start."

### Client progress and messages

#### Can an AI read my clients' workout results and tell me who is falling behind this week in TrueCoach?

Yes. TrueCoach shows a compliance score on the client list. With Cotlet, your agent reads this week's workouts per client, lists who missed sessions or stopped logging and says which ones need a call.

Ask your agent: "List my TrueCoach clients who completed fewer than half of this week's workouts, with their program and how many they did."

As an API endpoint: Yes. Save the list as an endpoint in Cotlet and call it every Friday from n8n, Make or Zapier to get it in Slack or email.

#### Can an AI draft replies to client check-in messages and notes inside TrueCoach?

Yes. With Cotlet, your agent reads a client's new messages and workout notes in TrueCoach's messenger and drafts a reply for each in your voice. You send each reply yourself.

Ask your agent: "Read my unanswered TrueCoach client messages from this week and draft a short, friendly reply for each."

#### Can an AI send weekly check-in forms to my clients and collect the answers from TrueCoach?

Partly. TrueCoach delivers check-ins as workouts or documents and no native form builder was found. With Cotlet, your agent reads each client's check-in answers in TrueCoach and puts them in one summary. Anything sent to clients waits until you say yes.

Ask your agent: "Collect this week's check-in answers from my TrueCoach clients and put them in one summary, one row per client."

As an API endpoint: Yes. Save the summary as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier to fill a Google Sheet.

#### Can an AI review my clients' MyFitnessPal logs and macros in TrueCoach and flag problems?

Yes. TrueCoach shows macro goals per client and food logs that arrive through the client's MyFitnessPal link. With Cotlet, your agent reads them per client and flags days far from the goal.

Ask your agent: "Check last week's MyFitnessPal logs in TrueCoach for each client and flag anyone far from their macro goals."

As an API endpoint: Yes. Save the check as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier.

#### How do I sync my client's Apple Health weight, Garmin or Apple Watch data into TrueCoach?

Partly. TrueCoach has its own wearables pages for Apple, Garmin, WHOOP and OURA, and the client connects the device. With Cotlet, your agent reads each client's wearables data in TrueCoach and sums up the week for you.

Ask your agent: "Read this week's wearables data for each of my TrueCoach clients and give me one line per client on sleep, steps and activity."

### New clients and email

#### How do I get new clients from my Instagram bio form or website form added to TrueCoach automatically?

Yes. TrueCoach's Zapier app has a Create Client action. With Cotlet, your agent reads each new form answer and adds the client in TrueCoach with name, email and group. Nothing is created until you say yes.

Ask your agent: "Take this week's new sign-ups from my form and add each to TrueCoach with email and group. Show me the list first."

As an API endpoint: Yes. Save the add-client step as an endpoint in Cotlet and call it from n8n, Make or Zapier whenever your form gets a new answer.

#### How do I get TrueCoach client messages forwarded to my Gmail so I stop missing them?

Yes. With Cotlet, your agent reads new client messages in your signed-in TrueCoach account and puts them in one email to your Gmail, or in a spreadsheet. Nothing is sent until you say yes.

Ask your agent: "Read my new TrueCoach client messages from today and email them to me in one summary, one line per client."

As an API endpoint: Yes. Save the message read as an endpoint in Cotlet and call it every few hours from n8n, Make or Zapier to email you the new ones.

#### How do I send a welcome email in Gmail every time a new client joins TrueCoach?

Partly. TrueCoach's help centre describes onboarding emails through Zapier and Gmail. With Cotlet, your agent lists the newest clients in TrueCoach and drafts a welcome email for each in your Gmail, signed in in the same Cloud Browser. Nothing is sent until you say yes.

Ask your agent: "List the clients who joined TrueCoach this week and draft a welcome email for each in Gmail. Do not send."

As an API endpoint: Yes. Save the new-client list as an endpoint in Cotlet and call it daily from n8n, Make or Zapier.

#### Can I connect TrueCoach to Google Calendar or Calendly so in-person sessions and bookings show up in one place?

Partly. Coaches report no booking for in-person sessions in TrueCoach. With Cotlet, your agent reads your clients' scheduled sessions and adds them as events in Google Calendar, signed in in the same Cloud Browser. Nothing is added until you say yes.

Ask your agent: "Add next week's TrueCoach sessions to my Google Calendar, one event per client session with the client name."

As an API endpoint: Yes. Save the week read as an endpoint in Cotlet and call it daily from n8n or Make to update Google Calendar.

### Payments and fees

#### Can an AI work out what the 5% processing fee costs me against using Stripe directly in TrueCoach?

Partly. TrueCoach Payments runs on Stripe with a flat 5% per transaction from 7 Jan 2026, for coaches in the US, UK, Australia and Canada. With Cotlet, your agent reads your invoices and totals the fee. Changing payment settings stays your own step.

Ask your agent: "Read my TrueCoach invoices for the last three months and total the 5% processing fee I paid, month by month."

As an API endpoint: Yes. Save the fee total as an endpoint in Cotlet and call it from n8n, Make or a script at each month's end.

### Exports, backups and moving

#### How do I export all my clients and their workout history from TrueCoach to Excel before I switch apps?

Yes. TrueCoach exports the client list as a CSV by email and workouts as text files per client and date range. With Cotlet, your agent goes through every client in your signed-in account and saves clients and workout history as files you keep.

Ask your agent: "Export every TrueCoach client's workouts for the last year into one file per client, and the client list as a CSV."

#### How do I export a client's TrueCoach workouts together with the results they entered?

Yes. TrueCoach's workout export is a text file per client for a date range. With Cotlet, your agent reads each workout and the results the client entered and gives you one table with exercise, sets, reps and weight.

Ask your agent: "Export Ria's TrueCoach workouts from the last six months with the results she entered into one spreadsheet."

#### How do I move my programs and client history from TrueCoach to Everfit, CoachRx or Trainerize without retyping everything?

Partly. Coaches report TrueCoach has no export another app can import. With Cotlet, your agent reads your programs and client history in TrueCoach and enters them in the new app, both signed in in your Cloud Browser. Nothing is created until you say yes.

Ask your agent: "Read my three TrueCoach programs and rebuild each one in Trainerize with the same workouts. Show me the first program before you save."

#### How do I back up my programs, check-ins and client list every week in case TrueCoach goes down again?

Yes. With Cotlet, your agent opens your signed-in TrueCoach account each week and saves your client list, programs and check-in answers as files you keep outside TrueCoach.

Ask your agent: "Save my TrueCoach client list, programs and this week's check-in answers as separate files I can keep as a backup."

As an API endpoint: Yes. Save the backup as an endpoint in Cotlet and call it every Sunday from n8n, Make or Zapier to store the files in Google Drive.

## Questions from TrueCoach coaches
**Does TrueCoach have a public API?**

No. The API key in Settings only connects TrueCoach's Zapier app (checked 23 Nov 2026). With Cotlet you build your own endpoints in your signed-in TrueCoach account and call them from Google Sheets, Zapier, Make or n8n.

**What can the TrueCoach Zapier app do?**

It has four triggers, such as New Client and Workout Completed, and one action, Create Client. To load workouts, read results or reply to messages from another tool, build your own endpoints with Cotlet.

**Can ChatGPT write a 12-week program and load it into TrueCoach?**

Yes. Your AI writes or takes your program and loads it into the client's calendar in TrueCoach, and shows you before it saves.

**Can AI tell me which TrueCoach clients are falling behind?**

Yes. It reads the week's workout results and lists clients who missed sessions or stopped logging.

**How do I stop missing TrueCoach client messages?**

Your AI lists messages and workout notes you have not answered and drafts a reply for each. You can also build an endpoint that sends them to your email or a sheet.

**How do I export clients and workout history from TrueCoach?**

Your AI exports clients, programs and workouts with results from the screens your login opens, so you keep everything if TrueCoach is down or you switch apps.

**Is there an official TrueCoach MCP server or ChatGPT app?**

No. Cotlet is an unofficial MCP server for the ChatGPT and Claude apps and any MCP client.

## Find it on

- [TrueCoach MCP server on Cotlet.dev](https://cotlet.dev/integrations/truecoach-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=truecoach&utm_content=find)
- [TrueCoach API with Cotlet](https://cotlet.dev/integrations/api/truecoach?utm_source=github&utm_medium=readme&utm_campaign=truecoach&utm_content=find)
- [Does TrueCoach have an API or MCP server?](https://cotlet.dev/api-mcp-status/truecoach?utm_source=github&utm_medium=readme&utm_campaign=truecoach&utm_content=find)
- [PyPI package truecoach-mcp](https://pypi.org/project/truecoach-mcp/)

## More fitness coaching apps on Cotlet

- [Trainerize MCP](https://github.com/cotlet-connect/trainerize-mcp)
- [Everfit MCP](https://github.com/cotlet-connect/everfit-mcp)
- [TrainHeroic MCP](https://github.com/cotlet-connect/trainheroic-mcp)
- [Exercise.com MCP](https://github.com/cotlet-connect/exercise-com-mcp)
- [PushPress MCP](https://github.com/cotlet-connect/pushpress-mcp)
