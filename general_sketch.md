I want to build a crutch for Slack and email. Slack is unreasonable and can cause mental health issues. At least I've gotten some after using it. You know what they say, it's the people that do the deeds but the institutions behind them that permit it.

So the solution is to first categorize. For example, incoming message appears. First block it from the user's vision (e.g. redact it). On top put a label from A+ to D-, D- is extremely easy to deal with and A+ is the most Machiavellian dilemma crap you can come up with.

Once you get the early warning, there are a few steps: you can choose to have the AI directly respond for you, or you can choose to have AI provide a suggestion which you can choose to follow or not, or you can choose to write your own response.

The same thing goes for email. So I won't be shocked when an unreasonable email comes, because it will be blocked by the AI first, so I can at least calibrate expectations.

The AI suggested messages and the grading can both be done by calling an api, like gemini 2.5 api

the app can also have a bunch of features:
- [Important!] With user's permission, rewrite incoming messages to separate tone from content, separate opinion from fact, and separate feelings from other information
- (Moat: customer obsession) Personalization. User gives AI feedback on their call (Good call / bad call) and AI begins to understand user's preferences and needs
- (Moat: customer obsession) Service Memory: The service could start to recognize communication patterns with specific people.
- (Moat: platform) data flywheel: More users -> more feedback -> a smarter, more personalized system -> happier users -> more new users
- (Moat: engineering effort) Deep App Integration
- (Moat: customer obsession) "Response Playbooks": Allow users to save their own successful responses to certain types of messages. When a similar message comes in, the AI can suggest their own proven playbook. This becomes a valuable, user-generated library that is unique to your service.
- (Moat: branding) Wellness company instead of technology company
It also has to have very easy to use UI without getting in the way of the user

(Important: since this is effectively a wrapper around an llm api, that is not the moat. The real moat is the above)

Monetization

P1: B2B 
- Companies buy seats in bulk (SaaS, /user /month)
- Employee wellness and productivity
- team dashboard for managers (with anonymized data) showing communication hotspots, team stress levels, and overall communication health (note this should be done with care to avoid manager and employee mutual suspicion)

P2: Consumer freemium
- Free tier (20 email limit per day, less suggested responses)
- Paid tier ($5 to $10 per month), unlimited analysis, Slack integration, FB messenger integration (if possible?), personalization, workflow integrations, access to "response playbooks"

Also perhaps open source the mvp (this): e.g. The basic browser extension for Gmail, the redaction logic, and the basic API connector. builds trust, transparency, and a community of developers who may be able to help
(this is following the unsloth business model: P1 is b2b (selling large orders), P2 is small consumers, and a lite version is open sourced. similar also at gitlab, docker, elastic)








Current plans:
- 1. MVP
- 2. Slide deck (SWOT analysis, market analysis, etc)
- 3. Match with cofounders
