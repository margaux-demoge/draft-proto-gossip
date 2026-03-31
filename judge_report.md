# DRAFT — Judge Report

> Generated 2026-03-31 by running 5 product judges against `spec.md`.
>
> **Judges:** MVP Checker, Risk Checker, Retention Validator, User Interview Simulator, Amon Vision
>
> **Verdicts summary:**
>
> - Amon Vision: BUILD IT
> - Risk Checker: HIGH RISK (2 critical, 13 high)
> - MVP Checker: TOO BROAD — NEEDS SCOPE CUT
> - Retention Validator: RETENTION IS FRAGILE
> - User Interview Simulator: WEAK ON KEY ASSUMPTIONS (1/10 answered, 5 partial, 4 unanswered)

---

## Action items


| #   | Urgency  | Question                                                                                                                                                                                      | Owner         | Action                                                                                                                                                                                       | Status |
| --- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| 1   | Critical | How do we validate that AI-generated cards are compelling enough before committing to full build?                                                                                             | Margaux       | Build the 10-30 question library and run manual card generation tests with real answers before automating.                                                                                   | To do  |
| 2   | Critical | How do we guarantee that 5-6 friends from the same group onboard and stay active simultaneously?                                                                                              | Judy          | Ask Judy for operational detail on the ambassador program — who, how, when.                                                                                                                  | To do  |
| 6   | Critical | What is DRAFT's tone of voice when asking questions?                                                                                                                                          | Margaux       | Build DRAFT's conversational tone of voice (separate from card writing tone defined in §5).                                                                                                  | To do  |
| 7   | Critical | What is the push notification copy strategy?                                                                                                                                                  | Margaux       | Write push notification copy — must feel like a personal message, not an app alert. Define format + examples.                                                                                | To do  |
| 8   | Critical | What is the onboarding question list?                                                                                                                                                         | Aymeric       | Define the dedicated first-exchange questions. These are separate from the daily theme library (#5) — they must be specific enough that a single reply generates a card. Comes from spec §8. | To do  |
| 9   | Critical | What is the follow-up stopping trigger?                                                                                                                                                       | Aymeric + eng | Define the signal or rule that tells DRAFT to stop asking follow-ups and start generating the card.                                                                                          | To do  |
| 10  | Critical | What is the full prompt specification?                                                                                                                                                        | Margaux       | Write a dedicated section listing all prompt types (daily, onboarding, follow-ups), their exact behavior, and rules governing when each fires.                                               | To do  |
| 19  | High     | Should MVP ship with all 4 card styles or just 1 safe style (Hype)?                                                                                                                           | Margaux       | Ask Aymeric to challenge on this — good point from the judges, no strong opinion yet.                                                                                                        | To do  |
| 20  | High     | Should card generation without user input ("at least 1 card/day even if user hasn't responded") be cut? (Spec §4, line 137.)                                                                  | Margaux       | Decide: the judges argue it undermines H1 measurement since silence is the data you need.                                                                                                    | To do  |
| 25  | Medium   | How labor-intensive is the ambassador program, and can it scale beyond the first 2-3 cohorts?                                                                                                 | Judy          | Ask Judy.                                                                                                                                                                                    | To do  |
| 28  | Medium   | Is the bottom sheet really 4 distinct states or can MVP ship with 2 (open/closed)?                                                                                                            | Margaux       | Ask Aymeric — no strong opinion, but worth challenging.                                                                                                                                      | To do  |
| 30  | Medium   | What is the revenue model if the concept works?                                                                                                                                               | Aymeric       | Ask Aymeric.                                                                                                                                                                                 | To do  |


---

## Answered — decisions made


| #   | Urgency  | Question                                                                                                                  | Decision                                                                                                                                                                                                                                                                                                                          |
| --- | -------- | ------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 3   | Critical | What content filtering or sensitivity guardrail prevents DRAFT from publishing something embarrassing, private, or wrong? | The card generation prompt must include guardrails: (1) never publish anything that could embarrass the user, (2) if the user explicitly said they don't want something shared, respect it, (3) absolutely no hallucination — only write what was actually said. This is a prompt engineering requirement, not a product feature. |
| 4   | Critical | Should we test H1 in isolation before building the full social feed for H2?                                               | No — testing H1 without the feed creates too much friction. Users won't answer questions if they don't see the result. It's a false problem, but worth keeping in mind that H1 must be validated first even if both are shipped together.                                                                                         |
| 17  | High     | Why would a user choose DRAFT over just texting their group chat or posting an Instagram Close Friends story?             | Hypothesis: DRAFT removes the excuse barrier. You don't have to impose reactions on your group chat. You don't need photo support. You can't overthink it because DRAFT writes it for you. Instagram Close Friends requires a photo, self-curation, and effort — DRAFT removes all of that.                                       |
| 18  | High     | What structural reason prevents the BeReal/Locket novelty-then-crash curve?                                               | Hypothesis: the key difference is that DRAFT posts for you. BeReal requires the user to take a photo and post it themselves — that's the friction that causes drop-off. DRAFT only asks you to answer a question, then handles the rest. Removing the creation friction is the structural bet.                                    |
| 21  | High     | What will make users think of DRAFT without a push notification?                                                          | Two internal triggers: (1) curiosity — you know DRAFT might have posted something about you but you don't know what or when, (2) habit — checking what your friends are up to, like opening a group chat. Similar to BeReal's curiosity mechanic.                                                                                 |
| 23  | Medium   | Should onboarding cut age, gender, and profile photo to reduce friction?                                                  | No. Age is required for Apple age restrictions. Gender is required because DRAFT writes about you in third person (he/she/they). Profile photo is required because the feed displays profile pics on every card. All three are justified.                                                                                         |
| 24  | Medium   | What happens to users who deny push notifications or stop noticing them?                                                  | For users in the app: add a fullscreen skippable popup reminding them to accept push notifications (reuse Frank's implementation, already in spec). For users who don't open the app at all: no solution for now — accepted risk.                                                                                                 |
| 31  | Medium   | Should voice and image input be cut from MVP?                                                                             | No — already developed (reusing Frank/Orai component), cutting it doesn't save any work.                                                                                                                                                                                                                                          |
| 33  | Low      | Is device-only account acceptable for test or will it cause real frustration?                                             | Fine for MVP. No concern.                                                                                                                                                                                                                                                                                                         |
| 34  | Low      | Should the 30-minute card generation delay be cut for MVP?                                                                | Keep it. The 30-minute delay is a product decision (gives the user time to keep talking). Not a significant implementation complexity.                                                                                                                                                                                            |


---

## Open questions — not MVP, revisit later


| #   | Urgency | Question                                                                                                   | Notes                                                                                                                                                             |
| --- | ------- | ---------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 11  | High    | What structural mechanic keeps users answering prompts after the first 2 weeks?                            | Not MVP. Test H1 first — do people answer at all? Retention mechanics come after validating the core hypothesis. Don't over-engineer before the first test.       |
| 13  | High    | What happens to the feed when 2 out of 6 friends stop responding?                                          | Not MVP. Post-MVP idea: if a user hasn't responded in 2 days, DRAFT posts a card about them saying they haven't responded — creates social pressure to re-engage. |
| 14  | High    | Can text-only input generate cards that are genuinely interesting and varied over time?                    | Core hypothesis to test. Long-term, integrations (Spotify, Apple Health, Photos, Calendar) will enrich input signal. For now, test whether text-only is enough.   |
| 15  | High    | Will users accept that they can't share when something actually happens — they must wait for DRAFT to ask? | Open question, but a good problem to have — it means users have formed the habit of talking to DRAFT. Power users will surface this as a feature request.         |
| 16  | High    | Does the 48-hour content window punish inconsistency and kill re-engagement?                               | Known risk. But removing ephemeral content creates other problems (stale feed, content accumulation, pressure). Accepted trade-off for now.                       |
| 22  | High    | Is "not knowing what friends are up to" painful enough to make someone adopt a new social app?             | Core hypothesis. Open question by design — this is what we're testing.                                                                                            |
| 27  | Medium  | How do you re-engage a lapsed user who skipped 3 days and returns to an empty feed?                        | Not MVP. Open question to revisit post-launch.                                                                                                                    |


---

## Removed


| #   | Question                                                                                | Reason                                                   |
| --- | --------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| 32  | What stops Instagram or Snapchat from shipping "AI summary of your close friends' day"? | Not their core feature to push. Not actionable. Removed. |


---

## Feed density target

From question #12: **minimum viable feed density = 4 cards per day** for a 6-person group. Quality matters more than quantity — if cards are genuinely interesting, 4 is enough. If cards are mediocre, no amount compensates.

