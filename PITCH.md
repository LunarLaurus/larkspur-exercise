# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A support agent for disrupted flights, with Larkspur's nine tools (plus ours).
Does: It finds the booking, looks for flights with free seats, holds one, and explains the fare rules in plain words.
Number: Counts tokens in and out for the customer query
Safety check: It holds a seat but never books it, only the customer's own click does.
Next: Let the agent answer customers in different languages
Still broken: Havent investigated scaling, no idea if it could handle a million stranded customers at the same time
Lever: <cost | speed | intelligence>

## Priya asked

Costs: About $0.06 per conversation (model cost only), on claude-sonnet-5-5
Wrong: The agent is built such that the information it provides is always guaranteed to come from your source data, there will be no data fabrication or hallucination.
Runs it: We will provide tools to enable you to monitor it and carry out first and 2nd line support, we will provide 3rd line support to fix any issues and add extra features.
Left out: Havent investigated scaling, no idea if it could handle a million stranded customers at the same time
