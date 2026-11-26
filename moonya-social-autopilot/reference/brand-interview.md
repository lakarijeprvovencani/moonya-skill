# The brand interview

For a user with no website (or a website that says nothing). Eight questions, asked in
**two or three messages**, never one at a time — nobody finishes a twenty-question
form in a chat.

Lead with why: *"Five minutes here is the difference between posts that sound like your
business and posts that sound like every other AI account."*

## Round 1 — the three that matter most

1. **What exactly do you sell, and what does it cost?**
   Products or services, the actual range, who each one is for. Push for specifics:
   "handmade velour tracksuits and sets, 6,900–9,900 RSD, made to order in 7–10 days"
   beats "women's clothing".
   → `sells` (aim for 150+ characters)

2. **Who buys it? Describe your best customer like a person.**
   Age, situation, what they want, where they hang out.
   → `audience` (aim for 120+ characters)

3. **Why do people choose you over the alternative?**
   The honest answer, not a slogan.
   → `usp`, `differentiators`

## Round 2 — voice and guardrails

4. **How should the posts sound?**
   Warm or direct, expert or playful, formal or informal address, emoji or no emoji.
   → `tone`, `voice_guide.traits`

5. **What should the posts never say?**
   Claims they can't back, discounts they don't run, topics that are off limits.
   → `avoid`, `rules.never`

6. **What do customers worry about before buying, and what do you tell them?**
   → `pains`, `objections`, and the answers into `proof_points`

## Round 3 — proof and the ask

7. **What proof do you have?**
   Numbers, years, reviews, results, certificates, how many customers.
   → `proof_points`

8. **What should a post ask people to do?**
   Comment a keyword, DM, click a link, book a slot — and the link it points to.
   → `cta {type, text, goal, founder_url}`

## While you're there

Three more that cost one sentence each and visibly improve every post:

- **Brand colors** (hex, or "the colors on my logo") → `branding {primary, accent, bg}`
- **Logo** → upload to the Gallery, then `logo {media_id, mode}`
- **Language and city** → `language`, `timezone`

## Writing it back

One `moonya_brain_update` call per round, sending only the fields that round filled.
Then `moonya_brain_get` to confirm the strength gaps closed.

If `moonya_status` or `moonya_brain_get` lists open Brain questions, answer them with
`moonya_brain_update {answers:[{question, answer}]}` using the question text verbatim.

## Rules

- **Never invent a brand fact.** No made-up founding year, no invented customer count,
  no fictional testimonial. An empty field is better than a false one — with a thin
  Brain, Moonya writes useful tips in the brand's field instead of fabricating a story.
- Save every later correction with `moonya_remember` so it applies to all future posts,
  not just the draft in front of you.
- A photo of a price list, menu or catalog is faster than any interview:
  `moonya_brand_doc` (1 credit) reads a PDF or DOCX straight into the Brain.
- If they do have a site, `moonya_brain_analyze` does rounds 1–3 in one call — free
  during onboarding. Then only check and correct it.

Moonya AI: https://moonya.ai/en
