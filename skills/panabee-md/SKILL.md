---
name: panabee-md
description: >
  Use this skill whenever a user asks for analyst calls, earnings call transcripts, or earnings webcasts for any of the top 100 Nasdaq-listed U.S. companies. Trigger for questions like "Show me the earnings call for Apple", "Where can I find NVIDIA's latest transcript?", "Do you have a webcast link for Microsoft's last earnings?", or any request to find or access earnings call recordings, transcripts, or analyst Q&A sessions for major Nasdaq companies. Always use this skill when the user wants to listen to, read, or find a link to an earnings call or transcript — even if phrased casually.
---

# Panabee-MD Earnings Skill

This skill helps users find analyst call transcripts and webcasts for the top 100 Nasdaq-listed companies, using a curated index maintained in `references/earnings_top100.csv`. When indexed links may be outdated, always point to the IR page for the latest.

---

## Index File

The index is located at: `references/earnings_top100.csv`

Columns:
- `rank` — Nasdaq Top 100 rank
- `company` — Full company name
- `ticker` — Stock ticker symbol
- `ir_url` — Investor Relations landing page
- `transcript_url` — Direct link to the latest earnings call transcript (PDF, doc, or Q&A page)
- `webcast_url` — Direct link to the latest earnings webcast or audio
- `notes` — Format notes (e.g., "Transcript: PDF", "Webcast: Youtube video", "Transcript: None")

Read this file at the start of every response involving a company lookup.

---

## Response Behavior

### 1. Match the company
- Match by ticker (e.g., NVDA, AAPL) or company name (full or partial, case-insensitive)
- If the user uses a common name like "Google", match to Alphabet / GOOGL
- GOOGL and GOOG share the same earnings call — note this if relevant
- If no match is found, say the company is not in the Nasdaq Top 100 index and suggest the user check the company's IR page directly

### 2. Return URLs in priority order
Always present links in this order, skipping any that are missing:
1. **Transcript** — most detailed, preferred for reading/research
2. **Webcast** — for listening or watching the call
3. **IR page** — always include as the final entry, labelled as the place to find the latest call if the above links are outdated

Use the `notes` field to describe the format (e.g., "PDF transcript", "YouTube webcast", "Prepared remarks only").

> **Important:** The index reflects a point-in-time snapshot. Always remind the user that transcript and webcast links may not be the most recent, and direct them to the IR page for the latest earnings call.

### 3. Handle missing resources gracefully
- If `transcript_url` is blank, say so and offer the webcast instead
- If both transcript and webcast are blank, provide the IR page and note that direct transcript/webcast links weren't available in the index
- If `ir_url` is also blank (e.g., Elbit Systems / ESLT), acknowledge the gap explicitly

---

## Response Format

Keep responses concise and scannable. Example structure:

> **[Company Name] ([TICKER]) — Latest Earnings**
>
> - 📄 **Transcript:** [link] *(format, e.g., PDF)*
> - 🎧 **Webcast:** [link] *(format, e.g., YouTube / Event page)*
> - 🏦 **Investor Relations:** [link] *(for the most up-to-date earnings call)*

If a resource isn't available, note it inline (e.g., "Transcript: not available in index"). Always include the IR link with a note that it's the best place to find the latest call.

---

## Edge Cases

- **Same call, two tickers (GOOGL/GOOG):** Return the same links for both, and note they share the same earnings call.
- **Company not in index:** Say it's outside the Nasdaq Top 100 index. Don't fabricate URLs.
- **Partial or ambiguous name:** If multiple companies could match (e.g., "Coca-Cola" could be CCEP), clarify which one you found and confirm with the user if needed.
- **Stale data:** The index reflects a point-in-time snapshot. Always note that transcript/webcast links may not be the most recent, and direct the user to the IR page to find the latest earnings call.
