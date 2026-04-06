# DRAFT

> **Your friend group has a personal journalist.** DRAFT talks to each of you, extracts what's going on, and publishes it as a shared daily feed - so you always know what's happening in your friends' lives, without anyone having to post.

---

## 1. Overview

**Problem**

There are two reasons people have stopped sharing what's actually going on in their lives. First, creating content has become high-friction and high-pressure - you need to think of something worth posting, frame it, and accept that it will be judged. Most people don't bother. Second, the content they do consume is algorithmic and performative - platforms optimise for engagement, not relevance, so your feed is full of strangers and influencers, not the real daily updates from the people you care about. The result: you have no idea what's going on in your friends' lives, and neither do they.

**Target users**

Primary: 18–25 year-olds with an active close friend group (5–15 people). They live on WhatsApp and Snapchat but haven't posted on Instagram in months. Post-graduation, post-breakup, post-move - the moments when you lose touch with what your friends are actually doing.

Secondary: Anyone who feels out of the loop with their close friends and doesn't have the energy or motivation to post about their own life.

Not for: couples, family groups, public creators, professional networks.

---

## 2. Product experience

Instead of posting on social media, DRAFT messages you twice a day and turns your replies into short posts published in a shared feed with your friends.

**Here's what happens:**

You install the app, enter your name, age and gender, and DRAFT immediately asks you a first question. No account creation needed, everything is device-based.

Twice a day (morning and evening), DRAFT sends you a push notification and asks you something about your life, like a curious friend checking in. You reply via text, voice or photo. DRAFT may follow up with more questions to dig deeper. Then DRAFT decides on its own whether to write a post. You find out when it's published.

The feed is chronological, on a rolling 48-hour window. You see posts about your friends, you can like or share them. If a post is about you and you want it gone, you can remove it.

Friends are added exclusively via invite link or QR code, no search, no contact sync. Clicking someone's link makes you friends immediately, no confirmation needed. You can always remove a friendship later.

In short: you talk, DRAFT writes, your friends read.

---

## 3. Success metrics


| #   | Hypothesis                                                                                                                | Metric | Target |
| --- | ------------------------------------------------------------------------------------------------------------------------- | ------ | ------ |
| H1  | People are willing to share small moments of their life if an AI asks them - rather than having to initiate it themselves | TBD    | ≥ 60%  |
| H2  | Daily updates from friends are interesting enough to make users engage with the content                                   | TBD    | ≥ 40%  |


---

## 4. First cohort strategy

> Owned by Jodie. This section defines how the first real users are acquired, what "activated" means, and the budget envelope for the test phase. It also justifies product decisions like the QR code and the instant friendship model.

---

### North star

Get real friend groups into Draft. A user only counts if they have downloaded the app, shared their personal link, and added at least 1 friend inside the app.

**Validation target**: 10 activated users meeting the full condition, across organizers and ambassadors combined.

---

### Payout structure

| Condition | Payout |
|---|---|
| Download + share link + 1 friend added in app | $5 |
| +$1 if that friend has themselves added at least 1 friend (via suggestions) | $1 |
| Cap per organizer or ambassador | $150 |

*Example: organizer downloads, shares their link, adds 1 friend, and that friend adds 1 more friend = $5 + $1 = $6.*

---

### Lever 1 — Party organizers

Organizers host a real social event and get their guests to install Draft and add each other on the spot. Recruited via Instagram lead gen campaigns.

**Funnel (conservative)**

| Step | Conversion | Output |
|---|---|---|
| Instagram leads generated | — | ~100–250 leads at $200–500 spend |
| Lead → confirmed organizer | ~10–15% | 15–20 organizers |
| Organizer → meets full payout condition | ~30–40% | 5–8 organizers paid |
| Cascade bonus triggered (friend adds a friend) | ~30–40% of paid organizers | 2–3 bonus payouts |

**Cost estimate**

| | Low | High |
|---|---|---|
| Instagram lead gen | $200 | $500 |
| Organizer payouts (5–8 × $5) | $25 | $40 |
| Cascade bonuses (2–3 × $1) | $2 | $3 |
| **Total lever 1** | **$227** | **$543** |

---

### Lever 2 — Ambassadors

Ambassadors share their personal Draft link inside real friend group chats (WhatsApp, Snapchat, iMessage). Same payout conditions as organizers. Recruited via direct outreach to students on Sideshift.

**Funnel (conservative)**

| Step | Conversion | Output |
|---|---|---|
| Students reached on Sideshift | — | ~50–100 outreach |
| Reach → confirmed ambassador | ~20–30% | 10–20 ambassadors |
| Ambassador → meets full payout condition | ~20–30% | 2–5 ambassadors paid |
| Cascade bonus triggered | ~30–40% of paid ambassadors | 1–2 bonus payouts |

**Cost estimate**

| | Low | High |
|---|---|---|
| Sideshift outreach (time cost, no media spend) | $0 | $0 |
| Ambassador payouts (2–5 × $5) | $10 | $25 |
| Cascade bonuses (1–2 × $1) | $1 | $2 |
| **Total lever 2** | **$11** | **$27** |

---

### Total budget estimate

| Lever | Low | High |
|---|---|---|
| Party organizers (incl. lead gen) | $227 | $543 |
| Ambassadors | $11 | $27 |
| **Total** | **$238** | **$570** |

---

### Open questions

- How do we track friend additions reliably in app — is that data accessible today?
- Does the cascade bonus (+$1) require product instrumentation to verify?
- Are guests actually adding each other at events, or just installing?

---

## 5. Flows

---

### Flow: Onboarding

**Trigger**: First app launch with no stored session.

**End state**: User has landed on the home feed with DRAFT's first question open in the bottom sheet.

#### Steps

1. **Splash**: Show the app logo and name. Single screen, no value prop copy needed for MVP. Auto-advances after a brief moment or on tap.
2. **Name**: The user enters their first name so DRAFT can personalise posts and questions from day one.
3. **Age**: The user enters their age. Used by DRAFT to calibrate question tone and relevance. Required for Apple age restrictions.
4. **Gender**: The user selects their gender. Used by DRAFT to personalise questions and post writing (he/she/they).
5. **Push permission**: Explain why notifications matter before triggering the native iOS prompt, so the user grants permission with context.
6. **Friends screen**: A dedicated screen shown to every user before landing on the feed. Two states:
  - If the user came from a link: shows the friend they just added + "people you may know" suggestions (friends of that friend). The user can add them directly from this screen. CTA: "Continue".
  - If the user didn't come from a link: shows the invite card with QR code and Copy link CTA. CTA: "Continue".
7. **Home feed**: User lands on the feed with the invite card visible and DRAFT's first question open in the bottom sheet. If the user came from a link and added friends who have already posted, those posts are already visible in the feed.

#### Business rules

**Account**

- No sign-up or authentication required. An account is created automatically on the device at first launch.
- If the user deletes and reinstalls the app, their account is preserved (same strategy as Orai - device token stored server-side).
- If the user switches devices, their account is lost. This is acceptable for MVP.

**Invite card**

- The invite card is a persistent card always visible in the feed, regardless of how many posts are available. It is not a post written by DRAFT - it is a fixed UI element.
- Content: "DRAFT works better with more friends." + QR code + "Copy link" CTA.
- Visible only to the user themselves - friends do not see it in their feed.
- Never disappears - it persists whether the feed has 0 or 50 posts.

**First question**

- DRAFT's first question appears as a bottom sheet over the home feed immediately on first arrival (for users who didn't come from a link). It does not wait for the scheduled 9:47 or 18:12 prompt window.
- The first question uses a theme from the regular theme library (themes are randomized by the backend - see §9 Prompt architecture).

#### Error paths & edge cases

- User quits mid-onboarding → resume from the last completed step on next launch
- User skips push permission → they can still use the app but will not receive prompt notifications. Every 24h, a full-screen skippable screen (reusing Frank's implementation) explains why notifications matter, with a CTA that redirects to iOS Settings > DRAFT > Notifications.
- User creates account and receives the onboarding first question → no scheduled prompt (9:47 / 18:12) is sent if the user already received a prompt in the last 4 hours
- Deep link doesn't resolve (user came from a link but deferred deep link fails) → user lands on home feed, can re-click the link later to open the friends tab and add the friend

---

### Flow: DRAFT asks a question

**Trigger**: Scheduled prompt at 9:47 or 18:12.

**End state**: User has completed the exchange. DRAFT decides independently whether to generate a post.

#### Steps

1. **Push notification**: DRAFT sends a notification displayed as a **chat message preview** - showing the actual question text, not a generic label like "DRAFT has a question." This makes it feel like a personal message. Tapping the notification opens the app and immediately opens the chat bottom sheet.
2. **DRAFT question sheet**: A bottom sheet opens over the feed showing DRAFT's question. The user sees the question in full before deciding whether to answer.
3. **User reply**: The user replies via text, voice, or image (reusing the Frank/Orai chat component).
4. **Follow-up**: DRAFT continues the conversation based on the follow-up trigger rules (see below). The user can stop replying at any time.
5. **Sheet closes**: The user stops responding and dismisses the sheet. No loading, no indication of what happens next. DRAFT processes the exchange in the background and decides independently whether to generate a post.

#### Business rules

**Post generation**

- Post generation is fully decoupled from the exchange. The user never knows if their response will generate a post, when, or what it will say. The surprise is intentional and core to the product.
- The generation process starts 30 minutes after the user's last message. No user response = no post generated.
- If a post is generated: a push notification is sent to the user's friends, and the post appears in their feed.
- If the input contains no signal (no named person, place, event, or emotional state with context): no post is generated and no feedback is given to the user.
- For MVP, posts are generated at any time of day. Future behaviour: posts are only published between 7am and 10:30pm. Responses received after 10:30pm trigger generation the next morning at 7am.

**Post timestamp**

- A post's timestamp is always the moment it is created, regardless of when the user shared the underlying information.

**Question system**

- DRAFT selects a theme at random from the library. Themes (~15) are hardcoded in the backend and passed as variables to the check-in prompt. The LLM does not choose themes - the backend randomizes.

**Check-ins**

- If the user did not respond to the 9:47 check-in, the 18:12 check-in is still sent. DRAFT either follows up on the unanswered question in an intelligent way or proposes a different question. It is never a verbatim repeat.
- DRAFT references previous exchanges when relevant. For example, the next day's prompt may open with a callback to what was discussed ("Yesterday you mentioned the Netflix thing - how did it go?") before moving to a new question. This builds trust and makes the interaction feel like a real conversation rather than a survey.
- Max 2 prompts per day per user.

**Follow-up**

- DRAFT continues the conversation naturally, showing curiosity about what the user shares. It asks follow-up questions to dig deeper but never feels like an interrogation. One consistent mode throughout.
- The user who returns 2 hours later keeps the conversation history - it's only overwritten by the next check-in.

**Offline**

- If the user submits while offline: the response is stored locally with a "pending" status visible to the user.

**Bottom sheet**

- The bottom sheet has two states:
(a) **New message** - DRAFT has a message the user hasn't read. A small indicator is shown on the bottom sheet bar.
(b) **No new message** - Nothing waiting. The user can still open the bottom sheet at any time to continue a conversation or re-read the last exchange.
- The user can **always** open the bottom sheet, regardless of state.

#### Error paths & edge cases

- User has already received 2 prompts today → no further prompts until the next day
- Network failure on submit → response stored as "pending", auto-sent on reconnect with retry logic
- User responds at 11:55pm → post generated at 12:25am, timestamped at creation time

---

### Flow: Browsing the feed

**Trigger**: User opens the app after completing onboarding.

**End state**: User has consumed at least one post and optionally taken an action.

#### Steps

1. **Home feed**: The user sees a chronological list of posts, newest first. No date dividers.
2. **Pull-to-refresh**: The user pulls down to refresh the feed and load new posts published since last open.
3. **Scrolling**: As the user scrolls, posts enter the viewport. A post is marked as viewed after 1 continuous second of at least 50% visibility.
4. **Like**: The user taps the like button on a post. The like count is visible to all users.
5. **Share**: The user taps the share button on a post to open the iOS native share sheet. The post is exported as an image.
6. **3-dots menu**: The user taps the 3-dots button on any post to open a context menu. Options depend on whether the post is about themselves or someone else (see Post moderation flow).
7. **New activity bubble**: When new posts are published while the user is in the app, a floating bubble appears at the bottom. Tapping it refreshes the feed and scrolls to the top.

#### Business rules

**Social graph**

- Friendship is created instantly - either by clicking an invite link or by tapping "Add" on a friend suggestion. No confirmation required in either case. Either user can remove the friendship at any time. Removal is symmetric: both lose access to each other's content immediately.

**Visibility**

- A post is visible to all friends of the person it's about.
- For MVP, a post is always about exactly one person. Other first names may appear in the body text, but only that person's avatar (first letter) and name are displayed on the post.

**Own posts**

- The user sees their own posts in the feed exactly as others do. They will recognise their own name and avatar. No special layout or pinning.
- If the user has zero friends, their own posts are visible only to themselves. The invite card is always visible to prompt them to add friends.

**Content window**

- The feed shows the last 48 hours of posts. Not a calendar day. Rolling 48-hour window.
- Post data is never deleted from the server. Posts are only hidden from the feed after 48h.

**Timestamps in the post**

- < 24h → relative label ("2min ago", "1h ago", "3h ago")
- 24h–48h → "yesterday"
- Posts older than 48h are no longer shown in the feed — "2 days ago" never appears.

**Likes**

- Like count is visible to all users. Who liked is not shown in MVP.
- Tapping like on a post that the user has already liked removes the like (toggle behaviour).
- When a friend likes a post the user is mentioned in, a push notification is sent: "[Friend name] liked a post you are mentioned in."

#### Error paths & edge cases

- Feed empty, no friends → only the invite card is visible
- Feed empty, friends present but no posts yet →only the invite card is visible
- Offline → cached feed shown with an "Offline" banner. Actions (like, share) are queued locally and synced when connection is restored. To confirm with engineering.

---

### Flow: Post moderation

**Trigger**: User taps the 3-dots button on a post.

**End state for own post**: The post is removed from all feeds where it was visible (the user's own feed and all their friends' feeds).

**End state for another person's post**: The report is submitted.

#### Steps (own post - user is the person it's about)

1. **3-dots menu**: Tapping the 3-dots button opens a context menu with: "Remove this post" and Cancel.
2. **"Remove this post"**: A confirmation dialog asks the user to confirm removal. On confirm, the post is immediately deleted from all friends' feeds.

#### Steps (another person's post)

1. **3-dots menu**: Tapping the 3-dots button opens a context menu with: "Report" and Cancel.
2. **Report**: The report is submitted immediately - no category selection required. A `Content Reported` analytics event is fired (post_id + associated data) for manual team review. A confirmation modal is shown: "Thanks, our team will review this quickly."

#### Business rules

**Removal**

- Deletion is silent. Friends see the post disappear with no explanation or notification.
- The post_removed rate is tracked in analytics as a proxy for content quality.

**Permissions**

- Users can remove only posts where they are the person it's about. They cannot modify posts about other people.

**Report**

- All users can report any post via the 3-dots menu. Required by Apple for UGC apps. MVP implementation: Amplitude event log only, manual team review. No complex moderation UI.

#### Error paths & edge cases

- User taps Cancel → menu closes, no action taken, no event fired

---

### Flow: Friend management

**Trigger**: User taps the friends icon.

**End state**: User has reviewed their current friend list and friend suggestions.

#### Steps

1. **Friends screen**: A single screen with two sections: (1) current friend list, (2) friend suggestions ("people you may know").
2. **At the top of the screen**: A persistent CTA to share the user's invite link or display their QR code, so they can grow their friend list at any time. The invite card's CTA also links here.
3. **Remove friend**: A confirmation prompt before removal. Both users immediately lose access to each other's posts. Removal also removes the person from each other's friend suggestions cascade.
4. **Friend suggestions**: Shown below the friend list once the user has ≥ 1 friend. Displays friends of the user's direct friends. Tapping "Add" makes them friends immediately (no request, no pending state). Suggestions cascade: adding someone reveals their friends as new suggestions.

#### Business rules

**Friendship model**

- No friend requests, no pending states. Clicking a link = instant friendship. Adding a suggestion = instant friendship.
- When someone clicks the user's invite link, they become friends immediately. The user receives a push notification: "[Name] is now your friend".
- There is no in-app search for users. Friends are added exclusively via invite link, QR code, or friend suggestions.
- **Risk accepted**: anyone with a link can become friends and see content. Suggestions cascade, creating potential chain propagation. Decision: test in MVP, monitor via analytics, kill the cascade if it causes problems.

**Friends list order**

- Friends are displayed in reverse chronological order of when they were added — most recently added first.

**"Add" button behaviour on suggestions**

- Tapping "Add" on a suggestion creates the friendship instantly at the data level.
- The button toggles to "Added ✓" (greyed out) in place — the person does not move from the suggestions section immediately.
- The friends count at the top updates immediately (+1).
- New suggestions from that person's network appear at the bottom of the "People you may know" section (cascade).
- On the next time the user opens the Friends screen, the added person appears in the friends list and is no longer shown in suggestions.

**Share CTA**

- The top of the friends tab always shows a CTA to share the user's personal invite link or display their QR code. This mirrors the invite card and keeps the growth loop accessible at all times.

**Removal**

- Friendship removal is symmetric and immediate. Neither user sees the other's posts after removal.

#### Error paths & edge cases

- User removes a friend → both lose access to each other's posts immediately; posts already in the feed disappear on next refresh

---

### Flow: Settings

**Trigger**: User navigates to Settings from the Home page

**End state**: User has updated a setting or taken an account action.

#### Steps

1. **Settings screen**: The user sees a list of options grouped by category (see content below).
2. **Permission settings**: Tapping Notifications deep-links to the relevant iOS system settings screen.
3. **Account actions**: Delete Account triggers a confirmation dialog before permanent deletion.

#### Business rules

**Settings content**

- Access: Notifications. Community: Help, Submit Feature Request. Legal: Privacy Policy, Terms of Service. Danger Zone: Delete Account. Footer: app version, build number, User ID.
- Reuse the generic Amon settings component already used in Orai and Frank.

**Delete account**

- Account deletion is permanent and irreversible. All posts authored by the user are removed from all friends' feeds immediately. The user's friends are not notified.

#### Error paths & edge cases

- Deep link to iOS settings fails → show a manual instruction ("Go to Settings > DRAFT > Notifications")
- Delete account fails on backend → show error, do not clear local session

---

## 6. Push notifications

DRAFT uses 4 types of push notifications:


| Type              | Title | Body                                                                      | Icon                         | Tap action                               |
| ----------------- | ----- | ------------------------------------------------------------------------- | ---------------------------- | ---------------------------------------- |
| Friend added      | DRAFT | "[Name] is now your friend"                                               | Standard DRAFT app icon      | Opens friends screen                     |
| Post about you    | DRAFT | "You are mentioned in a new Draft"                                        | Standard DRAFT app icon      | Opens home feed, scrolls to post         |
| Post in your feed | DRAFT | "New Draft about [Friend name]"                                           | Standard DRAFT app icon      | Opens home feed                          |
| DRAFT check-in    | DRAFT | Question text as chat preview (e.g. "Hey, how's your week going so far?") | Custom icon (to be designed) | Opens bottom sheet with the conversation |


---

## 7. Post styles

> **Naming note:** Posts are called "Drafts" in user-facing copy (notifications, UI labels). "Post" is the internal term used throughout this spec.

> DRAFT writes every post in one of four styles. The style shapes the tone and framing - not the facts. Every post is warm, personal, and always written in the third person about the person it's about.

### Writing rules

- **Person:** Always 3rd person. Never "you" or "I".
- **Headline:** 4–8 words. Max 55 characters. No period.
- **Body:** 1–2 sentences. Max 160 characters total.
- **Names:** Use first name only, exactly as entered at onboarding.
- **Tone baseline:** Warm across all styles. Roast is never mean.
- **Facts:** Never fabricate. Only write what the user actually shared.
- **Privacy:** Never publish anything that could embarrass the user. If the user explicitly said they don't want something shared, respect it - do not include it in any post.
- **Consent:** When in doubt, omit. The guardrail is a prompt engineering requirement, not a product feature.

---

### Styles

#### Hype

Headline frames the situation as something worth celebrating - even when it's mundane. Body adds genuine warmth. Reads like the most supportive person in the group chat.

- Lead with what went right, even if small.
- Body adds genuine warmth, not hype for its own sake.
- Don't oversell - understatement with warmth lands better than enthusiasm.

**Headline:** A simple weekend done right
**Body:** He went home, slowed down, and kept things easy. No chaos, no pressure, just a clean reset with family.

---

#### Roast

Headline names the slightly ridiculous thing. Body lands the punchline with one extra detail. Warm, not mean - the kind of thing a close friend would say to your face.

- Headline calls out the absurdity with precision.
- Body lands the punchline with one extra detail.
- Never punch down - the joke is about the situation, not the person.

**Headline:** Maximum driving, minimum actual weekend
**Body:** Spent more time on the road than doing anything else. Saturday there, Sunday back, that's the whole story.

---

#### Teaser

Headline drops just enough to make you curious. Body adds one detail but leaves a gap - the kind that makes you want to message them. DRAFT as conversation starter.

- Headline hints without explaining.
- Body adds one detail but withholds the conclusion.
- Leave the gap intentional - the post should make friends reach out.

**Headline:** His "chill" weekend sounds… interesting
**Body:** Just a quick trip to his parents, nothing special he said. But it felt like he skipped over something.

---

#### Gossip

Headline sounds like something whispered across a table. Body leans in with one more detail. Conspiratorial and warm - makes the mundane feel like actual news.

- Headline frames it as something worth leaning in for.
- Body adds one more detail with a knowing tone.
- Keep it warm - conspiratorial, never malicious.

**Headline:** He quietly went home this weekend
**Body:** Didn't make a thing of it, just slipped it in casually. But the quick in-and-out feels a bit telling.

---

### Style selection

DRAFT picks a style at random - the style is randomized by the backend and passed as a variable to the post generation prompt. The LLM does not choose styles. Each of the four styles has an equal 25% chance of being selected. This ensures variety in the feed and provides an even distribution to measure engagement per style from the first cohort.

---

## 8. Data / Dashboard

> This section defines the Amplitude charts to build before launch. Each chart maps directly to a success metric or a key funnel. Charts are built from the events defined in §10.

---

### North star charts


| Chart                  | Hypothesis | Calculation                                                 | Events                                                            |
| ---------------------- | ---------- | ----------------------------------------------------------- | ----------------------------------------------------------------- |
| Daily Engagement Rate  | H2         | `% of DAU who liked or shared ≥ 1 post` (rolling 7-day)     | Post Liked, Post Shared                                           |
| Prompt Completion Rate | H1         | `count(Prompt Completed) / count(Prompt Sent)` per day      | Prompt Completed, Push Sent                                       |
| D7 Retention           | -          | `users with ≥1 session on day 7 / users installed on day 0` | [Amplitude] Application Installed, [Amplitude] Application Opened |


---

### Supporting funnel charts


| Chart                      | Purpose                                      | Calculation                                                                                             | Events                                                                                        |
| -------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Onboarding Funnel          | Identify drop-off steps                      | Step conversion: Splash → Name → Age → Gender → Push Permission → Account Created → First post received | Onboarding Step Viewed, Onboarding Step Completed, Push Permission Completed, Account Created |
| Friends Added (First 24h)  | Measure activation - is the graph seeded?    | `avg(Friend Added) per user in first 24h after Account Created`                                         | Friend Added, Account Created                                                                 |
| Average Posts Available    | Is there enough content in the feed?         | `count(distinct post_id seen) / unique active users` per day                                            | Post Viewed                                                                                   |
| Question → Post Conversion | How much signal turns into a published post? | `count(Post Generated) / count(Prompt Completed)`                                                       | Prompt Completed, Post Generated (backend - to add)                                           |
| Push Opt-in Rate           | Measure notification permission rate         | `Push Permission Completed (successful=true) / Push Permission Viewed`                                  | Push Permission Viewed, Push Permission Completed                                             |
| New Users (Last 7 Days)    | Track acquisition pace                       | `count(Account Created)` rolling 7-day                                                                  | Account Created                                                                               |
| Prompt Follow-up Rate      | Measure depth of exchange                    | `Prompt Completed (is_followup=true) / Prompt Completed (is_followup=false)`                            | Prompt Completed                                                                              |
| Post Interaction Breakdown | Understand split of like vs share            | `count(Post Liked)` vs `count(Post Shared)` split by type                                               | Post Liked, Post Shared                                                                       |
| Invite Link Share Rate     | Measure growth loop activation               | `count(Invite Link Shared) / DAU`                                                                       | Invite Link Shared                                                                            |


> `[CLARIFY]` A "Post Generated" backend event is needed to measure post generation rate (posts generated / Prompt Completed). Must be added to the analytics plan before build. - owner: engineering

---

## 9. Prompt architecture

DRAFT uses three distinct prompts, each with a specific role. Themes and styles are **hardcoded in the backend** and passed as variables to the prompts, the LLM does not choose them.

### Check-in prompt

**Purpose**: Maximize open rate from the push notification and drive the user to start an exchange. Key metric: push sent to first reply rate.  
**Variables**: theme (randomized by backend from ~15 themes) + full conversation history + history of posts already written about the user.  
**Behaviour**: Sends a question oriented toward the selected theme, referencing past exchanges when relevant. The question appears as a chat message preview in the push notification.

**Three criteria for a valid check-in question,**  every question generated must satisfy all three:

1. **Answerable without thinking** — the user should be able to reply immediately, without having to craft an answer or weigh what to share.
2. **Shareable with everyone** — the answer must be comfortable to share with the whole friend group, not just a select few. Questions that touch on intimate or sensitive territory (e.g. love life, mental health) fail this criterion.
3. **Post-ready from a single reply** — one answer must contain enough signal (a named person, place, event, or situation with context) for DRAFT to write a post. If a topic consistently requires multiple follow-ups to yield anything publishable, it is too abstract.

A fourth implicit criterion: **the story should be news to friends**, if it's something the user has already shared widely, the post adds no value.

### Main prompt

**Purpose**: Handle the follow-up conversation after the user responds.
**Variables**: current conversation history + web search.
**Behaviour**: Curious but not a companion. Asks follow-up questions naturally to show interest, without feeling like an interrogation. One consistent mode throughout. Stops naturally when the user disengages. When the user mentions a specific cultural reference (film, documentary, artist, place), search the web to understand it before responding,so follow-up questions are informed rather than generic.

### Post generation prompt

**Purpose**: Generate the post from the conversation.
**Variables**: conversation history from the current exchange + style (randomized by backend from 4 styles).
**Behaviour**: Triggered 30 minutes after the user's last message. Takes the conversation and writes a post in the assigned style. Output structure: headline + body, following the writing rules defined in §7.

### Themes

Each theme is a topic direction passed as a variable to the check-in prompt. It orients the question without dictating exact wording — the LLM crafts the question from the theme, the user's conversation history, and their post history. The backend randomizes theme selection; the LLM does not choose themes.

**Live ops** — the theme list is a starting base, not a fixed set. Themes will be added, adjusted, or retired based on test results.

#### Theme list


| #   | Name                | Description                                                                                                      |
| --- | ------------------- | ---------------------------------------------------------------------------------------------------------------- |
| 1   | `weekend`           | What the user has planned or just experienced for the weekend — trips, plans, or low-key moments.                |
| 2   | `future_plans`      | Something upcoming the user is anticipating — a trip, event, decision, or life change.                           |
| 3   | `music`             | What they've been listening to lately. An album, artist, or song on rotation.                                    |
| 4   | `travel`            | A recent trip or upcoming travel. A quick weekend away counts.                                                   |
| 5   | `food`              | A recent meal, craving, restaurant discovery, or first attempt at cooking something.                             |
| 6   | `hobbies`           | Something the user is actively into or learning. A recurring activity or new skill.                              |
| 7   | `current_mood`      | How the user is genuinely feeling this week, anchored in a specific recent context (see note below).             |
| 8   | `small_win`         | Something the user quietly accomplished — even if it seems minor to others.                                      |
| 9   | `current_obsession` | A show, book, game, podcast, or creator they've been consuming non-stop.                                         |
| 10  | `recent_discovery`  | A new place, person, thing, or idea they just encountered for the first time.                                    |
| 11  | `social_moment`     | Something that happened between the user and specific people recently — a gathering, a conversation, a surprise. |
| 12  | `random_moment`     | A specific small moment or unusual detail from their day or week. The more concrete, the better.                 |


### Styles

4 styles defined in §7 (Hype, Roast, Teaser, Gossip). Elaborate definitions to be written.

**Owner**: Aymeric. To do.

### Prompt writing

- Check-in + main prompts - **owner: Margaux**
- Post generation prompt + style integration - **owner: Aymeric**

---

## 10. Analytics event definitions

> Naming convention: Title Case, Object followed by past-tense verb (e.g. "Post Viewed", "Prompt Dismissed"). Aligns with Amon standard.
> All iOS events include `current_view` (required) and `previous_view` (optional) as standard properties on every event.

### Common events

Events shared with Frank and Orai. Reuse the existing Amplitude schema as-is - do not duplicate or rename.


| Event                                | Category     | Source  | Notes                                                                                                               |
| ------------------------------------ | ------------ | ------- | ------------------------------------------------------------------------------------------------------------------- |
| [Amplitude] Application Installed    | -            | ios     | Auto-tracked by Amplitude SDK                                                                                       |
| [Amplitude] Application Opened       | -            | ios     | Auto-tracked by Amplitude SDK                                                                                       |
| [Amplitude] Application Backgrounded | -            | ios     | Auto-tracked by Amplitude SDK                                                                                       |
| [Amplitude] Application Updated      | -            | ios     | Auto-tracked by Amplitude SDK                                                                                       |
| Account Created                      | Amon Default | backend | Same schema as Frank. `account_id` required.                                                                        |
| Account Deleted                      | Amon Default | backend | Same schema as Frank. `account_id` required.                                                                        |
| Onboarding Step Viewed               | Amon Default | ios     | `step_name` enum must be updated for DRAFT steps: `splash, name, age, gender, push_permission`                      |
| Onboarding Step Completed            | Amon Default | ios     | Same as above for `step_name`                                                                                       |
| Push Permission Viewed               | Amon Default | ios     | Identical to Frank                                                                                                  |
| Push Permission Completed            | Amon Default | ios     | Identical to Frank                                                                                                  |
| Push Clicked                         | Amon Default | ios     | `type` enum must be updated: `post, prompt, friend_added` (replaces Frank's `message, checkin, broadcast, insight`) |
| Setting Clicked                      | Amon Default | ios     | `setting_name` values to define for DRAFT settings screen                                                           |


---

### App-specific events

Events new to DRAFT. Full property definitions below.

```yaml
TBD when flows are ready.
```

---

## 11. Open questions

**Must be resolved before build**

- `[DONE]` Theme library: 15 themes written, tested against post-ready constraint, and documented in §9. Pending Aymeric validation before build. - owner: Margaux
- `[CLARIFY]` Push notification copy: check-in notification should display the question as a chat message preview. Exact copy and format to be defined. - owner: Margaux
- `[CLARIFY]` Tone of voice when asking questions: DRAFT's register, level of warmth, and conversational style during exchanges. (Tone when writing posts is already defined in §7.) Must be defined before front-end build. - owner: Margaux
- `[CLARIFY]` Style definitions: elaborate the 4 post styles beyond the current examples. Must be detailed enough for prompt engineering. - owner: Aymeric
- `[CLARIFY]` Prompt writing: write the 3 prompts (check-in, main/chat, post generation). Check-in + main - owner: Margaux. Post generation + styles - owner: Aymeric.

**Engineering validation required**

- `[CLARIFY]` Question generation timing: generated synchronously at send time, or pre-generated a few hours in advance? To validate with engineering. - owner: engineering
- `[CLARIFY]` Deferred deep linking: technical complexity to validate with Florian and Romain before committing to MVP. - owner: engineering

**Risks**

- `[RISK]` Post quality is the core bet. If DRAFT's output feels generic or inaccurate, the input loop breaks. Mitigation: run manual prompt tests with real users before automating post generation.
- `[RISK]` Cold start: a user with zero friends sees very little. For the test phase, cohorts must include 5–6 people who know each other and onboard together. This is a hard requirement for the first test, not a nice-to-have.
- `[RISK]` Friend chain propagation: because friendship is instant (no confirmation) and suggestions cascade, a single shared link can lead to rapid, uncontrolled friend graph expansion. Anyone in the chain can see content from people they don't know. Mitigation: monitor via analytics, kill the cascade feature if it causes problems.

**Assumptions**

- `[ASSUMPTION]` Text-only conversation with no integrations generates posts worth reading. Validation: test with real users before the full build.
- `[ASSUMPTION]` Native post sharing via iOS share sheet (image format) is sufficient for MVP virality. Validation: track Post Shared rate and qualitative feedback from the first cohort.

---

*This document is the source of truth for product intent. Any implementation decision that diverges from this spec must be reflected here before the feature is marked done.*