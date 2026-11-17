# Client review: Larkspur disruption-care agent

**To:** Room 4  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-17 10:15

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

**1. readout-trace.json shows 2 tool calls, lookup_booking and get_flight_status, and the run ends before check_policy is called.**

The committed wire run made 3 API turns and called lookup_booking then get_flight_status. check_policy, search_alternatives and hold_seat never appear in the tool order. PITCH.md names the same gap under Still broken: "the agent sometimes does not call the check_policy and recalls polices from memory". The committed run is that gap, and nothing in it shows a model setting changing it.

Run python3 run.py --all --trace and paste the totals footer, so the check_policy call count across every booking shape is on the page.

**2. The run_agent loop in agent.py now appends response.content and takes answer = text_of(response) after the while loop.**

The diff replaces the plain-text assistant message with the full response.content block list, and the final answer now comes from the last response instead of the previous turn. The thinking parameter is on in all create() calls, and readout-trace.json says thinking was seen on the wire. That fix lets a multi-turn run complete, and it is the only evidence you have for the "reliably" in your Built line.

Run python3 verify.py 1.2 and paste the output that shows the loop continuing past the first tool_use.

**3. Your search_alternatives description in build_tools() is now a long single sentence, and it points at check_policy's output as the trigger.**

It went from "search" to a sentence that tells the model to search when check_policy returns rebooking, and to pass the IDs into hold_seat. That makes the call order depend on check_policy having run first. The committed trace never reaches check_policy, so the new text has not been exercised on the wire. The description also carries typos, "customer's source" is never defined as origin or destination airport codes, and the sentence has no instruction about what to do with an empty result.

Run python3 run.py --show-tools and paste the search_alternatives entry next to a trace from python3 run.py <PNR> --trace where search_alternatives is called.

**4. hold_seat is 73 characters and send_confirmation is 71 characters in build_tools(), the two shortest of the 9 descriptions.**

Every other description is between 216 and 445 characters. hold_seat reads "Place a 15-minute hold on one alternative. Reversible. It simply expires." and says nothing about when to call it or what option_id it expects. Your search_alternatives text tells the model its IDs feed hold_seat, but hold_seat's own description does not say where option_id comes from.

Run python3 run.py --tool-tax and paste the per-tool token cost, then add the hold_seat row to your readout.

**5. There is no evals/cases.json, no bench-before or bench-after, and the 9-tool Number in PITCH.md is a count, not a measurement.**

PITCH.md gives Number: "9 Tools offered every turn". Nothing in the repository gives a pass rate, a token cost, or a before and after for the check_policy fix you list under Next. The static scan finds no cache_control and readout-trace.json shows 0 prompt tokens read from cache, written, or hit, against 8933 in. TONE_ADDENDUM is 0 characters, so the lever you picked, intelligence, has no code change behind it yet.

Run python3 bench.py --label before --stage 1 --runs 3, make your change, run it again as after, and paste the python3 bench.py --compare before after output.

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

- `agent.py (231 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
