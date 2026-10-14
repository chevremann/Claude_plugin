# FlightCompensations.com for Claude

Look up a delayed, cancelled or overbooked flight and check whether it is owed
compensation (EU261, UK261, Israel's Aviation Services Law, Canada's APPR) and
how much. Asked how to claim, it gives both routes: the airline directly
(free), or FlightCompensations.com (a fixed fee, only if the claim succeeds).

- **Server:** `https://flightcompensations.com/api/mcp/check` — read-only, no
  sign-in; it files nothing and stores nothing.
- **Tools:** `look_up_flight` (the flight's record: route, times, delay,
  status), `check_flight_compensation`, `get_claim_link` (how to claim — only when asked).
- **Skill:** `check-flight` — Claude uses it when you describe a disrupted
  flight, or run `/flight-compensations:check-flight` with what happened.

## Install

**Claude Code**

```
/plugin marketplace add chevremann/Claude_plugin
/plugin install flight-compensations@flightcompensations
```

**Claude (claude.ai) and Cowork:** Customize → Plugins → Add → Add marketplace,
enter `chevremann/Claude_plugin`, then install **FlightCompensations.com**.
The server appears on the plugin's Connectors tab.

## Try

- "My Lufthansa flight from Frankfurt to New York on 14 August landed 5 hours late. Am I owed anything?"
- "easyJet cancelled my Gatwick to Faro flight two days before. What can I claim?"
- "I was bumped off an overbooked El Al flight from Tel Aviv to London. How much?"

License: MIT.

Support: support@flightcompensations.com · Privacy: https://flightcompensations.com/legal/privacy · Terms: https://flightcompensations.com/legal/terms
