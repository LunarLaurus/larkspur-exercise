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

**1. agent.py seats_left() answers enough_seats yes or no from one count, and its schema says it never runs on search_alternatives options.**

The diff adds seats_left(pnr, last_name, flight_no, date), which compares seats_left_Y or seats_left_J against len(booking["passengers"]) for the disrupted segment's cabin. Its 365-character description tells Claude not to use it on options returned by search_alternatives, so the tool only fires when a customer names a flight. Nothing in the repository shows it firing, and the committed trace used lookup_booking, get_flight_status, check_policy only.

Run python3 run.py --all --trace and paste the lines showing which booking shape calls seats_left and what it returned.

**2. readout-trace.json shows 12884 tokens in on 4 API turns with 0 cache read and 0 written, and agent.py has no cache_control.**

Each turn in run_agent resends runtime_preamble() + SYSTEM_PROMPT + TONE_ADDENDUM plus the full tools list, which is now 10 schemas plus mcp_client.tools(). The trace reports hit ratio None. A bigger model would pay for the same 12884 input tokens at a higher rate per token.

Run python3 run.py --tool-tax and paste the output so the per-turn cost of the tool definitions is a number.

**3. The evidence for Priya's model question is one committed trace of 3 tool calls, and the repository has no evals/cases.json.**

The trace stops at check_policy: no search_alternatives, no hold_seat, no escalate_to_human. The 8 booking shapes the agent will meet are not covered by any saved case, so no pass rate exists for any model, theirs or a larger one. The agent's failures on those shapes would come from tool descriptions, the loop and the policy table, which a model swap leaves untouched.

Run python3 eval_harness.py and paste the pass and fail summary for the whole suite.

**4. agent.py run_agent() sets answer = text_of(response) after each create call, so a loop that hits MAX_TOOL_CALLS = 8 returns text from a tool_use turn.**

The diff moved the answer assignment below the create call. When the while loop exits on turns < 8 with stop_reason still tool_use, answer holds whatever text blocks sat beside the last tool call, which can be empty or a mid-task sentence. No handoff to a human happens at the cap in this file.

Run python3 verify.py 1.2 and paste the output, then run python3 run.py <PNR> --trace on a booking that needs more than 7 tool calls and paste the final answer.

**5. agent.py declares 10 tool schemas where the pack ships 9, and send_confirmation is still the 71-character description.**

send_confirmation reads "Send the customer a written confirmation of what was just done. Benign." and gives no condition for when to call it or what the message must contain, while the other nine descriptions run 216 to 445 characters. TONE_ADDENDUM is still 0 characters, so nothing in agent.py tells Claude how to word that message.

Run python3 run.py --show-tools and paste the send_confirmation entry, then run python3 run.py <PNR> --trace on a rebooked booking and paste the message argument it sent.

## Your four answers

Four of the lines in your PITCH.md are answers to me rather than to your
verifier, and somebody on your side wrote them between our sessions. I read
those beside the code, not instead of it. Where an answer is carrying a number,
I have said whether the repository backs it.

| My question | Your answer | My read |
| --- | --- | --- |
| **What it costs** | About $0.06 per conversation (model cost only), on claude-sonnet-5-5 | **Not supported by the repository.** "About $0.06 per conversation" has no artifact under it: readout-trace.json reports 12884 in and 1125 out for one run and no price, and no bench-after.json is in the material. The model name "claude-sonnet-5-5" also appears nowhere in agent.py, which imports MODEL from support. |
| **When it is wrong** | The agent is built such that the information it provides is always guaranteed to come from your source data, there will be no data fabrication or hallucination. | **Not supported by the repository.** "Always guaranteed" and "no data fabrication or hallucination" is a claim with no evals/cases.json behind it, and the committed trace covers 3 tool calls on one booking. The answer names no failure case, and seats_left returning "no" or an error dict is a wrong-answer path it does not mention. |
| **Who runs it** | We will provide tools to enable you to monitor it and carry out first and 2nd line support, we will provide 3rd line support to fix any issues and add extra features. | **Thin.** "We will provide tools to enable you to monitor it" names no owner, no tool and no on-call; nothing in the material (readout.py output, a runbook, TEAM.md contents) says what that monitoring would show. The 1st, 2nd and 3rd line split is stated without who staffs each. |
| **What you left out** | Havent investigated scaling, no idea if it could handle a million stranded customers at the same time | **Answered.** "Havent investigated scaling, no idea if it could handle a million stranded customers" is an honest statement of an unmeasured number, and nothing in the repository contradicts it. A bench.py run at higher concurrency would be the artifact that turns it into a figure. |

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
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
