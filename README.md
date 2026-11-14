# TrueCoach MCP server

Connect TrueCoach to ChatGPT, Claude and any AI agent. Read clients, workouts, programs, compliance and messages, then approve writes before they reach your clients.

## What you can ask

> List clients whose 7-day compliance is below 80%

> Draft next week workouts for my group and ask before writing them

> Show workouts missed this week and message those clients after my approval

> Compare body metrics for a client over the last 90 days

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

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

## Questions from TrueCoach users

**Can I connect TrueCoach to ChatGPT or Claude?**

Yes. Add the Cotlet MCP connector and enable TrueCoach in your Cloud Browser.

**Does TrueCoach have an official MCP server or public API?**

TrueCoach has no official MCP server or public API. Cotlet provides hosted MCP and REST access to your coach account.

**What can the TrueCoach MCP server do?**

It can read clients, workouts, programs, compliance, metrics and messages, and can perform approved writes such as creating workouts and assigning programs.

**Are writes approved before they reach clients?**

Writes that affect clients ask for your approval first.

## Find it on

- [TrueCoach on Cotlet.dev](https://cotlet.dev/integrations/truecoach-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=truecoach&utm_content=find)
- [PyPI package truecoach-mcp](https://pypi.org/project/truecoach-mcp/)
- [Odel](https://odel.app/modules/cotlet/truecoach)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/truecoach/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-truecoach-to-ai-agents-guide)

## Also works with

- [Trainerize MCP](https://github.com/cotlet-connect/trainerize-mcp)
- [everfit MCP](https://github.com/cotlet-connect/everfit-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Boulevard MCP](https://github.com/cotlet-connect/boulevard-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [Meevo MCP](https://github.com/cotlet-connect/meevo-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [AestheticsPro MCP](https://github.com/cotlet-connect/aestheticspro-mcp)
- [Club OS MCP](https://github.com/cotlet-connect/club-os-mcp)
- [EZ Lawn Biller MCP](https://github.com/cotlet-connect/ez-lawn-biller-mcp)
- [Eversports Studio Manager MCP](https://github.com/cotlet-connect/eversports-studio-manager-mcp)
- [Exercise.com MCP](https://github.com/cotlet-connect/exercise-com-mcp)
- [Gymdesk MCP](https://github.com/cotlet-connect/gymdesk-mcp)
- [Iris Works MCP](https://github.com/cotlet-connect/iris-works-mcp)
- [CoachingPortal MCP](https://github.com/cotlet-connect/coachingportal-mcp)
- [CrewNest MCP](https://github.com/cotlet-connect/crewnest-mcp)
- [Paired Plus MCP](https://github.com/cotlet-connect/paired-plus-mcp)
- [ClubReady MCP](https://github.com/cotlet-connect/clubready-mcp)
- [Joist MCP](https://github.com/cotlet-connect/joist-mcp)
- [Substack MCP](https://github.com/cotlet-connect/substack-mcp)
- [AnyList MCP](https://github.com/cotlet-connect/anylist-mcp)
- [Midjourney MCP](https://github.com/cotlet-connect/midjourney-mcp)
- [Systeme.io MCP](https://github.com/cotlet-connect/systeme-io-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/truecoach-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=truecoach

Start free: https://cotlet.dev/signup?app=truecoach&utm_source=github&utm_medium=readme&utm_campaign=truecoach
