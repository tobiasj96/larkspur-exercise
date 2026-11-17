# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built:  The agent now reliably processes complex, multi-turn interactions by continuously calling tools until a definitive answer is achieved and delivered. It also accurately processes date formats and provides more comprehensive tool descriptions.
Does: When a flight is delayed or cancelled, this agent acts as a disruption-care specialist, automatically processing customer inquiries, verifying booking details against real-time policy, and presenting rebooking options to support passengers.
Number: 4 eval cases out of 6 passed and the hard evals are a must to qualify for a release however the 2 failed cases are a harh eval and hence the release is blocked.
Safety check: The lookup_booking tool requires that last name and PNR
Next: Fix the agent sometimes not calling the check_policy tool
Still broken: The agent sometimes does not call the check_policy and recalls polices from memory which can cause problems and sometimes offensive language is not properly addressed by the agent
Lever: intelligence

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
