# MedBillAnalyzer plugin

Check a medical bill against your insurer's Explanation of Benefits (EOB) and see where the bill asks for more than your plan says you owe.

This plugin connects Claude Code, Cowork, Cursor and Gemini CLI to MedBillAnalyzer's remote MCP server, and adds a skill that tells the agent how to use it.

## What's included

| Component | What it does |
|---|---|
| `.mcp.json` | Claude Code and Cowork: connects to `https://app.medbillanalyzer.com/mcp` (Streamable HTTP, no sign-in) |
| `.cursor-plugin/plugin.json` + `mcp.json` | Cursor: connects to `https://app.medbillanalyzer.com/mcp/apps` (Streamable HTTP, no sign-in) |
| `gemini-extension.json` + `GEMINI.md` | Gemini CLI: `gemini extensions install https://github.com/RMKCapital/medbillanalyzer-plugin` |
| `skills/check-medical-bill` | How to run a check, read the results, offer the paid unlock, and handle the patient's privacy |
| `commands/check-bill` | `/medbillanalyzer:check-bill` starts a check |

## Tools

| Tool | Read-only | What it does |
|---|---|---|
| `check_medical_bill` | no | Compares the bill with the EOB and creates a case. Free |
| `get_case` | yes | Re-reads a case |
| `unlock_case` | no | Returns a Stripe Checkout link. It never charges anything itself |
| `get_case_outputs` | yes | The dispute letter and phone script, after payment |
| `export_case` | yes | Everything stored about a case |
| `report_outcome` | no | Records how a dispute turned out, as an aggregate count |
| `delete_case` | no (destructive) | Permanently deletes a case |

## Examples

- "Here's my hospital bill and the EOB from my insurer. Do I really owe this?"
- "Check this lab bill against my EOB. It looks like they charged me twice."
- "My plan says I owe $24 but the doctor's office billed me $250. What's going on?"

## Pricing

The scan is free and shows how much is in question and the title of each finding. The explanation of each finding, what to ask for, a dispute letter and a phone script cost **$10 for the bill**, or come with a **$10-a-month membership** that covers up to 25 bills a month for the whole family. The membership renews monthly until cancelled; email support@medbillanalyzer.com to cancel. **If we find nothing, you pay nothing.**

## Privacy Policy

What reaches MedBillAnalyzer is what Claude sends: the figures read off your documents, or the documents themselves if Claude attaches them. We keep results for 30 days and then delete them, you can delete a case at any time, and nothing is used to train AI models. Full policy: https://medbillanalyzer.com/privacy

## Support

support@medbillanalyzer.com · https://medbillanalyzer.com/claude

This is not legal, medical, or financial advice. It compares documents you supplied and helps you ask questions about them.
