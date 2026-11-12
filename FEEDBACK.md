# Client review: Larkspur disruption-care agent

**To:** Room 11  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-12 09:45

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

**1. agent.py run_agent(): the loop appends response.content and now returns text_of(response) from inside the while body only.**

Your diff moved answer = text_of(response) below the second client.messages.create call and deleted the answer = "" initialiser. If the first response has stop_reason other than tool_use, the while body never runs and the return statement references a name that was never bound. The committed run took 4 API turns, so the path where Claude answers without calling a tool has not been exercised.

Run python3 run.py K7PQ2M --trace on a booking where the first reply is plain text, and paste the output or the traceback.

**2. agent.py build_tools(): send_confirmation is still the 71-character description, and confirm_rebooking sits beside it with a customer-click token.**

The send_confirmation description is "Send the customer a written confirmation of what was just done. Benign." and it carries no instruction on when to call it or what message must contain. You rewrote search_alternatives and hold_seat and left this one, so the final customer-facing write in the flow is the least specified.

Run python3 run.py --show-tools and paste the send_confirmation entry next to your intended calling rule for it.

**3. readout-trace.json: the committed wire run called lookup_booking, get_flight_status, check_policy, and stopped after 4 turns with 0 cache tokens.**

12884 tokens in, 1125 out, 10.8s wall clock, cache read 0, written 0, hit ratio None. It never reached search_alternatives, hold_seat, or your cause_in_plain_words, so none of the descriptions you rewrote in the diff were exercised on the wire. agent.py has no cache_control anywhere, so the 12884 input tokens are billed in full on every turn.

Run python3 run.py --all --trace and paste the totals footer, so one number covers every booking shape and not one PNR.

**4. agent.py EXTRA_TOOLS: cause_in_plain_words reads disruption_policy.json from disk on every call, and nothing in the repo shows it was ever called.**

The function opens data/americas/disruption_policy.json each invocation and returns an error dict for any code outside WX, ATC, MX, CREW, SEC. Its 428-character description tells the model to call it whenever it names a cause, and readout-trace.json lists three tool calls, none of them this one. A model that skips an optional tool and states the cause itself is not a failure a larger model removes, because the instruction is already in the description.

Run python3 run.py K7PQ2M --trace and paste the line showing whether cause_in_plain_words was called.

**5. Repository: no evals/cases.json, banked gates are 1.2, 1.3, 1.4 only, and PITCH.md is blank including the Lever line.**

The static scan finds no eval cases, readout.html banks three gates, and TONE_ADDENDUM is 0 characters. No before-and-after number exists for any lever, so Priya's model question has no data under it yet. You also have 22 given files that differ from the shipped pack, including verify.py, eval_harness.py and bench.py, which changes what any number from them means.

Run python3 bench.py --label before --stage 1 --runs 3 and paste the output, and list which of the 22 modified given files you edited on purpose.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (288 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
