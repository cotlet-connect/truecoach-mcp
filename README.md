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
