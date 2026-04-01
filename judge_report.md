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


| #   | Urgency  | Question                                                                                                                     | Owner         | Action                                                                                                                                                                                       | Status |
| --- | -------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| 1   | Critical | How do we validate that AI-generated posts are compelling enough before committing to full build?                            | Margaux       | Build the 10-30 question library and run manual post generation tests with real answers before automating.                                                                                   | To do  |
| 2   | Critical | How do we guarantee that 5-6 friends from the same group onboard and stay active simultaneously?                             | Jodie          | Ask Jodie for operational detail on the ambassador program — who, how, when.                                                                                                                  | To do  |
| 6   | Critical | What is DRAFT's tone of voice when asking questions?                                                                         | Margaux       | Build DRAFT's conversational tone of voice (separate from post writing tone defined in §5).                                                                                                  | To do  |
| 7   | Critical | What is the push notification copy strategy?                                                                                 | Margaux       | Write push notification copy — must feel like a personal message, not an app alert. Define format + examples.                                                                                | To do  |
| 25  | Medium   | How labor-intensive is the ambassador program, and can it scale beyond the first 2-3 cohorts?                                | Jodie          | Ask Jodie.                                                                                                                                                                                    | To do  |


---

## Answered — decisions made


| #   | Urgency  | Question                                                                                                                  | Decision                                                                                                                                                                                                                                                                                                                          |
| --- | -------- | ------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 3   | Critical | What content filtering or sensitivity guardrail prevents DRAFT from publishing something embarrassing, private, or wrong? | The post generation prompt must include guardrails: (1) never publish anything that could embarrass the user, (2) if the user explicitly said they don't want something shared, respect it, (3) absolutely no hallucination — only write what was actually said. This is a prompt engineering requirement, not a product feature. |
| 4   | Critical | Should we test H1 in isolation before building the full social feed for H2?                                               | No — testing H1 without the feed creates too much friction. Users won't answer questions if they don't see the result. It's a false problem, but worth keeping in mind that H1 must be validated first even if both are shipped together.                                                                                         |
| 8   | Critical | What is the onboarding question list?                                                                                      | Onboarding questions are merged with the theme system — no separate list. Themes (~15) are hardcoded in the backend and passed as variables to prompts. **Work still to do:** define the ~15 themes. Owner: Margaux.                                                                                                              |
| 9   | Critical | What is the follow-up stopping trigger?                                                                                    | Exchanges 1–3: DRAFT asks direct questions (curiosity mode, question marks). From exchange 4: soft/curious mode, no direct questions ("tu me tiendras au courant" instead of "c'était avec qui ?"). If user re-engages after soft mode → back to question mode. **Work still to do:** implement in prompts. Owner: Aymeric + eng.  |
| 10  | Critical | What is the full prompt specification?                                                                                     | 3 prompts defined: (1) Check-in — sends notif + question, variables: theme + conversation history + post history. (2) Main/chat — follow-up conversation, curious but not a companion. (3) Post generation — writes the post, variables: conversation history + style. **Work still to do:** write actual prompts. Owner: Aymeric (post gen + styles) + Margaux (check-in + main). |
| 17  | High     | Why would a user choose DRAFT over just texting their group chat or posting an Instagram Close Friends story?             | Hypothesis: DRAFT removes the excuse barrier. You don't have to impose reactions on your group chat. You don't need photo support. You can't overthink it because DRAFT writes it for you. Instagram Close Friends requires a photo, self-curation, and effort — DRAFT removes all of that.                                       |
| 18  | High     | What structural reason prevents the BeReal/Locket novelty-then-crash curve?                                               | Hypothesis: the key difference is that DRAFT posts for you. BeReal requires the user to take a photo and post it themselves — that's the friction that causes drop-off. DRAFT only asks you to answer a question, then handles the rest. Removing the creation friction is the structural bet.                                    |
| 19  | High     | Should MVP ship with all 4 post styles or just 1 safe style (Hype)?                                                       | Keep 4 styles. Aymeric tested with 1 style only → feed was too boring and monotone. Styles are randomized by the backend (not the LLM). **Work still to do:** write elaborate style definitions. Owner: Aymeric.                                                                                                                  |
| 20  | High     | Should post generation without user input ("at least 1 post/day even if user hasn't responded") be cut?                   | Yes — removed from MVP. Post generation only happens 30 minutes after the user's last message. If the user doesn't respond, no post is generated.                                                                                                                                                                                 |
| 21  | High     | What will make users think of DRAFT without a push notification?                                                          | Two internal triggers: (1) curiosity — you know DRAFT might have posted something about you but you don't know what or when, (2) habit — checking what your friends are up to, like opening a group chat. Similar to BeReal's curiosity mechanic.                                                                                 |
| 23  | Medium   | Should onboarding cut age, gender, and profile photo to reduce friction?                                                  | **Updated:** Profile photo removed from MVP — using first letter of name as avatar instead. Age kept (Apple age restrictions). Gender kept (DRAFT writes in third person — he/she/they). Previous decision overridden after Aymeric+Margaux discussion.                                                                           |
| 24  | Medium   | What happens to users who deny push notifications or stop noticing them?                                                  | For users in the app: add a fullscreen skippable popup reminding them to accept push notifications (reuse Frank's implementation, already in spec). For users who don't open the app at all: no solution for now — accepted risk.                                                                                                 |
| 28  | Medium   | Is the bottom sheet really 4 distinct states or can MVP ship with 2 (open/closed)?                                        | Simplified to 2 states: badge (new message) / no badge. User can always open the bottom sheet. "Processing" state removed.                                                                                                                                                                                                        |
| 30  | Medium   | What is the revenue model if the concept works?                                                                            | IAP consumables. Trigger: implement when D7 retention ≥ 35%. No hard paywall (destroys friend density). Advertising is a secondary lever (text content = low revenue). Main model: unlock posts, secret posts with name reveal, weekly editions, longer versions, media unlock.                                                    |
| 31  | Medium   | Should voice and image input be cut from MVP?                                                                             | No — already developed (reusing Frank/Orai component), cutting it doesn't save any work.                                                                                                                                                                                                                                          |
| 33  | Low      | Is device-only account acceptable for test or will it cause real frustration?                                             | Fine for MVP. No concern.                                                                                                                                                                                                                                                                                                         |
| 34  | Low      | Should the 30-minute post generation delay be cut for MVP?                                                                | Keep it. The 30-minute delay is a product decision (gives the user time to keep talking). Not a significant implementation complexity.                                                                                                                                                                                            |


---

## Open questions — not MVP, revisit later


| #   | Urgency | Question                                                                                                   | Notes                                                                                                                                                             |
| --- | ------- | ---------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 11  | High    | What structural mechanic keeps users answering prompts after the first 2 weeks?                            | Not MVP. Test H1 first — do people answer at all? Retention mechanics come after validating the core hypothesis. Don't over-engineer before the first test.       |
| 13  | High    | What happens to the feed when 2 out of 6 friends stop responding?                                          | Not MVP. Post-MVP idea: if a user hasn't responded in 2 days, DRAFT publishes a post about them saying they haven't responded — creates social pressure to re-engage. |
| 14  | High    | Can text-only input generate posts that are genuinely interesting and varied over time?                    | Core hypothesis to test. Long-term, integrations (Spotify, Apple Health, Photos, Calendar) will enrich input signal. For now, test whether text-only is enough.   |
| 15  | High    | Will users accept that they can't share when something actually happens — they must wait for DRAFT to ask? | Open question, but a good problem to have — it means users have formed the habit of talking to DRAFT. Power users will surface this as a feature request.         |
| 16  | High    | Does the 48-hour content window punish inconsistency and kill re-engagement?                               | Known risk. But removing ephemeral content creates other problems (stale feed, content accumulation, pressure). Accepted trade-off for now.                       |
| 22  | High    | Is "not knowing what friends are up to" painful enough to make someone adopt a new social app?             | Core hypothesis. Open question by design — this is what we're testing.                                                                                            |
| 27  | Medium  | How do you re-engage a lapsed user who skipped 3 days and returns to an empty feed?                        | Not MVP. Open question to revisit post-launch.                                                                                                                    |


---

## Removed from this report

- **Feed density target** (question #12) — moved to `spec.md` §6 Data / Dashboard as a requirement.
