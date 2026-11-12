# Client review: Larkspur disruption-care agent

**To:** Room 11  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-12 09:55

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py send_confirmation description is 71 characters and your diff left it as shipped.**

The scan puts send_confirmation at 71 characters: "Send the customer a written confirmation of what was just done. Benign." It names no trigger and no ordering against confirm_rebooking or hold_seat. Your diff rewrote search_alternatives and hold_seat and touched nothing else on this tool, so a bigger model reads the same 71 characters.

Run python3 run.py --show-tools and paste the send_confirmation entry as the model sees it.

**2. No evals/cases.json exists, so nothing in the repo shows a case where the agent was wrong.**

The scan reports no eval cases. Your seats_left tool answers "yes" only if the flight has seats for the whole party, and nothing in the repo exercises the "no" branch, the "Name does not match this PNR." branch, or the "No seat data" branch. The only run on record is readout-trace.json, which shows one conversation: lookup_booking, get_flight_status, check_policy.

Write evals/cases.json with at least one case per branch of seats_left, then run python3 eval_harness.py and paste the summary.

**3. readout-trace.json shows 0 cache read, 0 written, and 12884 tokens in over 4 API turns.**

Every turn in run_agent resends runtime_preamble() + SYSTEM_PROMPT + TONE_ADDENDUM plus a tool list of 10 schemas, and the scan finds no cache_control anywhere in agent.py. The trace reports "hit ratio None". 12884 input tokens for 3 tool calls is mostly repeated prefix, and a different model name does not change that count.

Run python3 run.py --tool-tax and paste the output, then python3 run.py --all --trace and paste the totals footer.

**4. agent.py seats_left reads cabin from get_disrupted_segment but sends the party size from the booking, one flight, one date.**

The tool compares "seats_left_J" or "seats_left_Y" against len(booking["passengers"]) and returns only {"enough_seats": ..., "party_size": ...}. Your description tells the model not to call it on search_alternatives results because "those already fit the whole party". Nothing in the repo shows that claim holding on any booking shape.

Run python3 verify.py 2.1 and paste the output, then python3 run.py --all --trace and paste which PNRs called seats_left.

**5. PITCH.md Lever is still the placeholder "<cost | speed | intelligence>" and Number counts tokens only.**

The Number line reads "Counts tokens in and out for the customer query", a quantity with no before, no after and no denominator. No bench output is in the material, so no pair of runs backs any lever. Priya's question is about the model, and the repo has one run on one model with 1125 tokens out.

Run python3 bench.py --label before --stage 1 --runs 3, make one change, run it again as after, then paste python3 bench.py --compare before after.

## Your four answers

Four of the lines in your PITCH.md are answers to me rather than to your
verifier, and somebody on your side wrote them between our sessions. I read
those beside the code, not instead of it. Where an answer is carrying a number,
I have said whether the repository backs it.

| My question | Your answer | My read |
| --- | --- | --- |
| **What it costs** | About $0.06 per conversation (model cost only), on claude-sonnet-5-5 | **Not supported by the repository.** "About $0.06 per conversation" has no artifact under it: readout-trace.json shows 12884 in and 1125 out for one run and no price per token or multi-run average. Paste the bench-after.json compare output with the rate you used. |
| **When it is wrong** | The agent is built such that the information it provides is always guaranteed to come from your source data, there will be no data fabrication or hallucination. | **Not supported by the repository.** "Always guaranteed ... no data fabrication or hallucination" has no evals/cases.json behind it, and nothing in agent.py enforces it. The loop sets answer = text_of(response) from the model's free text on the last turn. |
| **Who runs it** | We will provide tools to enable you to monitor it and carry out first and 2nd line support, we will provide 3rd line support to fix any issues and add extra features. | **Thin.** "We will provide tools to enable you to monitor it" names no tool, and the repo contains none beyond run.py --trace and readout.py. Name the owner and the artifact for first, second and third line. |
| **What you left out** | Havent investigated scaling, no idea if it could handle a million stranded customers at the same time | **Answered.** "Havent investigated scaling, no idea if it could handle a million stranded customers" is an honest statement of an unmeasured thing, and nothing in the repo contradicts it. It would be stronger with a rate or a concurrency figure from bench.py. |

All four answered. Bring the artifact behind each one to our next meeting, not the sentence.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (277 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `PITCH.md (4 of 4 answers to Priya)`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
