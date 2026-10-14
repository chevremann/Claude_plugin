---
name: check-flight
description: Look up a disrupted flight and check whether it is owed compensation under EU261, UK261, Israel's Aviation Services Law or Canada's APPR, and how much, using the FlightCompensations.com tools. Use when someone describes a delayed, cancelled or overbooked flight (or a missed connection), asks what an airline owes them, or asks how to claim.
argument-hint: "[what happened — e.g. 'LH400 Frankfurt to New York on 14 Aug, landed 5 hours late']"
---

# Flight compensation check — FlightCompensations.com

The `flightcompensations` server runs the same eligibility engine as
flightcompensations.com. Let it decide; do not work out entitlement or
amounts from general knowledge when a tool can answer.

If the user typed something after the command, treat it as their description
of the flight: $ARGUMENTS

## The three tools

1. **`look_up_flight`** — the flight's own record from its flight number and
   date: route, operating airline, scheduled and actual times (local), the
   arrival delay in minutes and the status (on time, delayed, cancelled,
   diverted). **Use it first whenever the user gives a flight number and
   date** — then you do not need to ask how late they were. When there is no
   record, it says so; never invent one, and fall back on what the user says.
2. **`check_flight_compensation`** — a preliminary verdict and the estimate
   per passenger, with a short plain explanation. Facts only.
   It needs:
   - `fromCode` and `toCode`: the first departure airport and the final
     destination of the booking, as 3-letter IATA codes;
   - `disruption`: what happened;
   - for a delay or missed connection, `delayBand`: how late they reached the
     **final destination** (under 3 hours, 3–4 hours, over 4 hours);
   - for a cancellation, `cancelNotice`: how far ahead the airline told them;
   - for overbooking, `deniedType`: refused boarding against their will, or
     gave up the seat for a deal.
   The operating airline (`airlineIata`, e.g. LH, BA, LY) matters for flights
   **into** Europe, the UK, Israel or Canada; pass it when known. Pass
   `flightDate` and `flightNumber` when known — the date lets the check refuse
   a claim that is too old. Given a
   flight number and date, it also looks the flight up itself when the route
   or the delay is missing, and says it used the record.
3. **`get_claim_link`** — how to claim: directly from the airline, free, or
   through FlightCompensations.com with the form pre-filled (a fixed fee per
   passenger, charged only if the claim succeeds). **Use it only when the user
   asks how to claim or wants help claiming** — never offer it unasked.

## When the record and the passenger disagree

- The record is the flight's own leg. A delay counts at the passenger's
  **final destination**: with a connection after this flight, ask about the
  arrival at the end of the trip before relying on the leg's delay.
- If the record shows the flight on time (or under three hours late) but the
  passenger says it was much later, say so politely and ask them to check the
  **date**: an overnight delay often leaves on the next day, and the record
  for the original date is then a different, on-time flight. Look up the
  date they confirm.
- If the record shows the flight cancelled, ask how much notice the airline
  gave.

## How to ask

- First use everything the user already said. Convert city and airport names
  to IATA codes yourself only when unambiguous (London has several airports —
  ask which).
- Ask for **one missing fact at a time**, in plain words: "How late did you
  arrive at your final destination — under 3 hours, 3 to 4 hours, or more
  than 4?" Never guess a delay length or a notice period.
- Do not ask for passport numbers, card details, booking passwords or ID
  documents. The claim form collects what a claim needs, securely.

## How to show the result

- Report the verdict, the estimate and the explanation as the tool gives them.
  Quote its amount and currency; do not substitute your own.
- It is a **preliminary** check: the airline's records and the passenger's
  documents decide the final answer. Never say a claim is certain or that
  money is guaranteed.
- When the verdict depends on the airline's reason (weather, air-traffic
  control, a strike by third parties, security), say that the airline must
  prove such "extraordinary circumstances" — do not decide it.
- Stop there. Do not add a call to action or a link. If the user then asks how
  to claim or wants help, call `get_claim_link` and give both routes it
  returns, as it states them — including that claiming from the airline
  directly is free.
- Nothing is filed in the chat. Never tell the user a claim has been
  submitted.

## Facts that must not drift

- A delay counts at **arrival at the final destination**: three hours or more
  (EU261 as interpreted by the Court of Justice in *Sturgeon*, Joined Cases
  C-402/07 and C-432/07).
- A cancellation notified **14 days or more** before departure gives no fixed
  compensation under EU261 (Article 5(1)(c)(i)); refund or re-routing is still
  owed.
- Time limits to claim are set nationally and differ widely (about one year in
  Poland to six years in the UK today). An EU261 reform was approved by the
  European Parliament on 7 July 2026 and is expected to apply from the second
  half of 2027; until it applies, the current rules stand.
