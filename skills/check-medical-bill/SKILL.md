---
name: check-medical-bill
description: Check a medical bill against the patient's Explanation of Benefits (EOB) with the MedBillAnalyzer connector. Use when someone shares a medical bill, hospital or doctor statement, lab bill, or EOB and asks whether it is right, what they actually owe, or how to dispute it.
---

# Checking a medical bill

MedBillAnalyzer compares a provider's bill with the EOB the insurer issued for the same care and reports where they disagree: charges the plan never processed, balances above what the plan says the patient owes, duplicate lines, and similar.

## What you need

- **The bill** (required). An itemized bill, with codes and dates of service, allows the most checks.
- **The EOB** for the same visit (strongly recommended). Most of the checks depend on it. A bill on its own can still be checked, but less completely. Say so if the patient has only the bill.

## Running the check

1. Call `check_medical_bill`. Attach the documents if you can. If you can read them but not attach them, transcribe them into `bill` and `eobs` **exactly as printed**, following the field descriptions. Leave out a field you cannot read. Never estimate or fill one in.
2. Read the patient `amount_in_question` and the title of each item in `what_we_found`. These are free.
3. When nothing was found, say `you_can` as written. It is worded to match how completely the bill could be checked. A partial check with no findings does not mean the bill is correct.
4. Follow `next_step`, and pass on `disclaimer`.

## The paid unlock

When a case has findings and `for_sale` is true, the patient can pay for the explanation of each finding, what to ask for, a dispute letter, and a phone script. Offer both plans:

- **$10 for this bill** (`plan: "one_bill"`).
- **A $10-a-month membership** (`plan: "subscription"`). It covers up to 25 bills a calendar month for anyone in the family. It costs the same as a single bill, so it is the better deal if the patient has another bill or expects one. One hospital or ER visit often produces separate bills from the facility, each doctor, the lab and imaging. It renews every month until cancelled. To cancel, email support@medbillanalyzer.com.

If they want either plan:

- Call `unlock_case` with that plan and give them `checkout_url`. Payment happens on that page. Never try to complete it for them.
- For a membership, also give them `membership_token` to keep. It is shown once and cannot be recovered. Pass it back as `membership_token` to unlock later bills without being charged again.
- After they have paid, call `get_case_outputs` for the letter and the script.

Do not offer the unlock when nothing was found or `for_sale` is false. There is nothing behind it.

## Privacy

- Keep `delete_token`, and do not show it to the patient. Tell them they can ask you to delete the case at any time. If they ask, call `delete_case` with it.
- `export_case` returns everything stored about a case, if they want to see it.
- Results are kept for 30 days, then deleted, and nothing is used to train AI models. Details: https://medbillanalyzer.com/privacy
- If the patient later tells you how a dispute turned out, record it with `report_outcome`.

This is not legal, medical, or financial advice. It compares documents the patient supplied and helps them ask questions about them.
