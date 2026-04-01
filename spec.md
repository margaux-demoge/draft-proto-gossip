# DRAFT

> **Your friend group has a personal journalist.** DRAFT talks to each of you, extracts what's going on, and publishes it as a shared daily feed — so you always know what's happening in your friends' lives, without anyone having to post.

---

## 1. Overview

**Problem**

There are two reasons people have stopped sharing what's actually going on in their lives. First, creating content has become high-friction and high-pressure - you need to think of something worth posting, frame it, and accept that it will be judged. Most people don't bother. Second, the content they do consume is algorithmic and performative - platforms optimise for engagement, not relevance, so your feed is full of strangers and influencers, not the real daily updates from the people you care about. The result: you have no idea what's going on in your friends' lives, and neither do they.

**Target users**

Primary: 16–25 year-olds with an active close friend group (5–15 people). They live on WhatsApp and Snapchat but haven't posted on Instagram in months. Post-graduation, post-breakup, post-move — the moments when you lose touch with what your friends are actually doing.

Secondary: Anyone who feels out of the loop with their close friends and doesn't have the energy or motivation to post about their own life.

Not for: couples, family groups, public creators, professional networks.

---

## 2. Product experience

Instead of posting on social media, you answer questions from an AI (Draft) — and it turns your answers into short posts published in a shared feed with your friends.

**Here's what happens:**

You install the app, enter your name, age and gender, and DRAFT immediately asks you a first question. No account creation needed, everything is device-based.

Twice a day (morning and evening), DRAFT sends you a push notification and asks you something about your life, like a curious friend checking in. You reply via text, voice or photo. DRAFT may follow up with more questions to dig deeper. Then, without asking your permission, it decides whether there's enough signal to write a post, and publishes it in your friends' feed (and yours too obviously).

The feed is chronological, on a rolling 48-hour window. You see posts about your friends, you can like or share them. If a post is about you and you want it gone, you can remove it.

Friends are added exclusively via invite link or QR code, no search, no contact sync. Clicking someone's link makes you friends immediately, no confirmation needed. You can always remove a friendship later.

In short: you talk, DRAFT writes, your friends read.

---

## 3. Success metrics


| #   | Hypothesis                                                                                                                | Metric | Target |
| --- | ------------------------------------------------------------------------------------------------------------------------- | ------ | ------ |
| H1  | People are willing to share small moments of their life if an AI asks them — rather than having to initiate it themselves | TBD    | ≥ 60%  |
| H2  | Daily updates from friends are interesting enough to make users engage with the content                                   | TBD    | ≥ 40%  |


---

## 4. Scope

**In MVP**

- No authentication: account created automatically on device at first launch
- Minimal onboarding: name, age, gender, push permission. No profile photo — first letter of name used as avatar.
- Welcome post on first session: "[Name] just joined DRAFT!" with QR code and share link. Persists indefinitely (does not expire after 48h). Accessible from the friends tab at all times.
- At the end of onboarding, user lands on the friends tab (if they came from a link) or the home feed (if they didn't)
- AI-generated posts published throughout the day, based on conversations with DRAFT
- Conversational input loop: DRAFT prompts users 2× daily (9:47 and 18:12)
- Post generation: 30 minutes after the user's last message in the exchange. No generation without user input.
- Like button on each post with visible like count
- Share button on each post: fixed label "Share", opens iOS native share sheet (image output). CTA is never AI-generated in MVP.
- Post moderation via 3-dots menu: remove post (if tagged) or report
- Block/report: Amplitude event log only, manual team review. No complex moderation UI.
- Chronological feed with relative timestamps, pull-to-refresh
- Push notifications: 4 types (friend added, post about you, post in your feed, DRAFT check-in as chat preview)
- Friend system: click on link = direct friendship, no confirmation needed. No pending requests. User can unfriend.
- Friend suggestions: friends of your direct friends (cascading — if you add someone, you see their friends too)
- iOS only, English, US App Store

**Out of MVP**

- Profile photo upload
- Waiting list
- Auto-generation without user input (generating posts from stored facts even if user hasn't responded)
- User-initiated input (DRAFT always initiates)
- Data integrations (Spotify, Apple Health, Photos, Calendar)
- In-app messaging
- Friend search, contact scan
- Sub-group scoping
- Weekly digest or newspaper format
- Public profiles or discovery
- Post detail screen
- Quick-tap reply options on prompts
- Duplicate post detection when two friends mention the same event
- Post generation restricted to daytime hours (7am–10:30pm); MVP generates at any time
- Question sheet "closed" state: MVP always shows the last exchange, overwritten by next question
- Android, web, non-US markets

---

## 5. Flows

---

### Flow: Onboarding

**Trigger**: First app launch with no stored session.

**End state**: If the user came from a friend's link: user is on the friends tab, sees their new friend and "people you may know" suggestions. If the user didn't come from a link: user is on the home feed with the welcome post visible and DRAFT's first question open in the bottom sheet.

#### Steps

1. **Splash**: Show the app logo and name. Single screen, no value prop copy needed for MVP. Auto-advances after a brief moment or on tap.
2. **Name**: The user enters their first name so DRAFT can personalise posts and questions from day one.
3. **Age**: The user enters their age. Used by DRAFT to calibrate question tone and relevance. Required for Apple age restrictions.
4. **Gender**: The user selects their gender. Used by DRAFT to personalise questions and post writing (he/she/they).
5. **Push permission**: Explain why notifications matter before triggering the native iOS prompt, so the user grants permission with context.
6. **Friends tab or Home feed**: See end state above. If coming from a link, the friend is already added and the friends tab opens with suggestions. If not from a link, the home feed opens with the welcome post and DRAFT's first question in the bottom sheet.

#### Business rules

**Account**

- No sign-up or authentication required. An account is created automatically on the device at first launch.
- If the user deletes and reinstalls the app, their account is preserved (same strategy as Orai — device token stored server-side).
- If the user switches devices, their account is lost. This is acceptable for MVP.

**Welcome post**

- The welcome post "[Name] just joined DRAFT!" is generated immediately on account creation. It contains a QR code linking to the user's personal invite link and a "Share your link" CTA. Its purpose is to seed the growth loop from the very first session.
- The welcome post is visible only to the user themselves — friends do not see it. It does **not** expire — it persists indefinitely and remains accessible from the friends tab so the user can always share their link.

**First question**

- DRAFT's first question appears as a bottom sheet over the home feed immediately on first arrival (for users who didn't come from a link). It does not wait for the scheduled 9:47 or 18:12 prompt window.
- The first question uses a theme from the regular theme library (themes are randomized by the backend — see §9 Prompt architecture).

**Friend graph**

- If the user came from a link: they are automatically added as friends with the person who shared the link (no confirmation needed). The friends tab opens showing this friend and "people you may know" suggestions (friends of that friend).
- If the user didn't come from a link: they arrive on the home feed. Friends are added later via the share link or QR code.

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

1. **Push notification**: DRAFT sends a notification displayed as a **chat message preview** — showing the actual question text, not a generic label like "DRAFT has a question." This makes it feel like a personal message. Tapping the notification opens the app and immediately opens the chat bottom sheet.
2. **DRAFT question sheet**: A bottom sheet opens over the feed showing DRAFT's question. The user sees the question in full before deciding whether to answer.
3. **User reply**: The user replies via text, voice, or image (reusing the Frank/Orai chat component).
4. **Follow-up**: DRAFT continues the conversation based on the follow-up trigger rules (see below). The user can stop replying at any time.
5. **Sheet closes**: The user stops responding and dismisses the sheet. No loading, no indication of what happens next. DRAFT processes the exchange in the background and decides independently whether to generate a post.

#### Business rules

**Post generation**

- Post generation is fully decoupled from the exchange. The user never knows if their response will generate a post, when, or what it will say. The surprise is intentional and core to the product.
- The generation process starts **30 minutes after the user's last message** in the exchange. If the user never replies, no generation is triggered from that exchange.
- If the user doesn't respond to a prompt, no post is generated. There is no automatic generation without user input.
- If a post is generated: a push notification is sent to the user's friends, and the post appears in their feed.
- If the input contains no signal (no named person, place, event, or emotional state with context): no post is generated and no feedback is given to the user.
- For MVP, posts are generated at any time of day. Future behaviour: posts are only published between 7am and 10:30pm. Responses received after 10:30pm trigger generation the next morning at 7am.

**Post timestamp**

- A post's timestamp is always the moment it is created, regardless of when the user shared the underlying information.

**Question system**

- DRAFT selects a theme at random from the library. Themes (~15) are hardcoded in the backend and passed as variables to the check-in prompt. The LLM does not choose themes — the backend randomizes.

**Prompts**

- If the user did not respond to the 9:47 prompt, the 18:12 prompt is still sent. DRAFT either follows up on the unanswered question in an intelligent way or proposes a different question. It is never a verbatim repeat.
- DRAFT references previous exchanges when relevant. For example, the next day's prompt may open with a callback to what was discussed ("Yesterday you mentioned the Netflix thing — how did it go?") before moving to a new question. This builds trust and makes the interaction feel like a real conversation rather than a survey.
- Max 2 prompts per day per user.

**Follow-up trigger**

- **Exchanges 1–3**: DRAFT pushes in curiosity mode — direct questions with question marks. The goal is to extract maximum context. ("Ah nice, who was there?" / "Wait, what happened after that?")
- **From exchange 4**: DRAFT switches to soft/curious mode — no direct questions, but still shows interest. ("Sounds like a great weekend. You'll have to tell me how it goes." — period, not question mark.)
- If the user re-engages after the soft mode → DRAFT goes back to question mode.
- The user who returns 2 hours later keeps the conversation history — it's only overwritten by the next check-in prompt.

**Offline**

- If the user submits while offline: the response is stored locally with a "pending" status visible to the user.

**Bottom sheet**

- The bottom sheet has two states:
(a) **Badge** — DRAFT has a new message the user hasn't read. Small indicator (badge/dot) on the bottom sheet bar.
(b) **No badge** — No new messages. The user can still open the bottom sheet at any time to continue a conversation or re-read the last exchange.
- The user can **always** open the bottom sheet, regardless of state.

#### Error paths & edge cases

- User dismisses without answering → no post generated from that exchange, prompt not repeated today
- User has already received 2 prompts today → no further prompts until the next day
- Network failure on submit → response stored as "pending", auto-sent on reconnect with retry logic
- User responds at 11:55pm → post generated at 12:25am, timestamped at creation time

---

### Flow: Browsing the feed

**Trigger**: User opens the app after completing onboarding.

**End state**: User has consumed at least one post and optionally taken an action.

#### Steps

1. **Home feed**: The user sees a chronological list of posts, newest first. Timestamps are displayed as relative labels ("1 hour ago", "yesterday"). No date dividers.
2. **Pull-to-refresh**: The user pulls down to refresh the feed and load new posts published since last open.
3. **Scrolling**: As the user scrolls, posts enter the viewport. A post is marked as viewed after 1 continuous second of at least 50% visibility.
4. **Like**: The user taps the like button on a post. The like count is visible to all users.
5. **Share**: The user taps the share button on a post to open the iOS native share sheet. The post is exported as an image.
6. **3-dots menu**: The user taps the 3-dots button on any post to open a context menu. Options depend on whether the post is about themselves or someone else (see Post moderation flow).
7. **New activity bubble**: When new posts are published while the user is in the app, a floating bubble appears at the bottom. Tapping it refreshes the feed and scrolls to the top.

#### Business rules

**Social graph**

- Friendship is created instantly when someone clicks a friend's invite link — no confirmation required. Either user can remove the friendship at any time. Removal is symmetric: both lose access to each other's content immediately.

**Visibility**

- A post is visible to all friends of its subject.
- For MVP, a post always has exactly one subject. Other first names may appear in the body text, but only the subject's avatar (first letter) and name are displayed on the post.

**Own posts**

- The user sees their own posts in the feed exactly as others do. They will recognise their own name and avatar. No special layout or pinning.
- If the user has zero friends, their own posts are visible only to themselves. Empty state prompts them to invite people.

**Content window**

- The feed shows the last 48 hours of posts. Not a calendar day. Rolling 48-hour window.
- Post data is never deleted from the server. Posts are only hidden from the feed after 48h.

**Likes**

- Like count is visible to all users. Who liked is not shown in MVP.
- Tapping like on a post that the user has already liked removes the like (toggle behaviour).
- When a friend likes a post the user is mentioned in, a push notification is sent: "[Friend name] liked a post you are mentioned in."

#### Error paths & edge cases

- Feed empty, no friends → empty state with invite nudge
- Feed empty, friends present but no posts yet → "DRAFT is still gathering intel. Check back soon."
- Offline → cached feed shown with an "Offline" banner, like and share buttons disabled

---

### Flow: Post moderation

**Trigger**: User taps the 3-dots button on a post.

**End state for own post**: The post is removed from all feeds where it was visible (the user's own feed and all their friends' feeds).

**End state for another person's post**: The report is submitted.

#### Steps (own post — user is the subject)

1. **3-dots menu**: Tapping the 3-dots button opens a context menu with: "Remove this post" and Cancel.
2. **"Remove this post"**: A confirmation dialog asks the user to confirm removal. On confirm, the post is immediately deleted from all friends' feeds.

#### Steps (another person's post)

1. **3-dots menu**: Tapping the 3-dots button opens a context menu with: "Report" and Cancel.
2. **Report**: The report is submitted immediately — no category selection required. A `Content Reported` analytics event is fired (post_id + associated data) for manual team review. A confirmation modal is shown: "Thanks, our team will review this quickly."

#### Business rules

**Removal**

- Deletion is silent. Friends see the post disappear with no explanation or notification.
- The post_removed rate is tracked in analytics as a proxy for content quality.

**Permissions**

- Users can remove only posts where they are the subject. They cannot modify posts about other people.

**Report**

- All users can report any post via the 3-dots menu. Required by Apple for UGC apps. MVP implementation: Amplitude event log only, manual team review. No complex moderation UI.

#### Error paths & edge cases

- User taps Cancel → menu closes, no action taken, no event fired

---

### Flow: Friend management

**Trigger**: User taps the friends icon or navigates to the friends tab.

**End state**: User has reviewed their current friend list and friend suggestions.

#### Steps

1. **Friends tab**: A single screen with two sections: (1) current friend list, (2) friend suggestions ("people you may know").
2. **At the top of the screen**: A persistent CTA to share the user's invite link or display their QR code, so they can grow their friend list at any time. The welcome post's share CTA also links here.
3. **Add friend via link**: When someone clicks the user's invite link, they become friends immediately — no confirmation needed. The user receives a push notification: "[Name] just joined Draft" (or "[Name] is now your friend" if the person already had an account).
4. **Remove friend**: A confirmation prompt before removal. Both users immediately lose access to each other's posts. Removal also removes the person from each other's friend suggestions cascade.
5. **Friend suggestions**: Shown below the friend list once the user has ≥ 1 friend. Displays friends of the user's direct friends. Tapping "Add" makes them friends immediately (no request, no pending state). Suggestions cascade: adding someone reveals their friends as new suggestions.

#### Business rules

**Friendship model**

- No friend requests, no pending states. Clicking a link = instant friendship. Adding a suggestion = instant friendship.
- There is no in-app search for users. Friends are added exclusively via invite link, QR code, or friend suggestions.
- **Risk accepted**: anyone with a link can become friends and see content. Suggestions cascade, creating potential chain propagation. Decision: test in MVP, monitor via analytics, kill the cascade if it causes problems.

**Share CTA**

- The top of the friends tab always shows a CTA to share the user's personal invite link or display their QR code. This mirrors the welcome post and keeps the growth loop accessible at all times.

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

- Profile: Name (editable). Access: Notifications. Community: Help, Submit Feature Request, Give a Review. Legal: Privacy Policy, Terms of Service. Danger Zone: Delete Account. Footer: app version, build number, User ID.
- There is no separate profile tab in MVP. Name is editable directly from Settings.
- Reuse the generic Amon settings component already used in Orai and Frank.

**Delete account**

- Account deletion is permanent and irreversible. All posts authored by the user are removed from all friends' feeds immediately. The user's friends are not notified.

#### Error paths & edge cases

- Deep link to iOS settings fails → show a manual instruction ("Go to Settings > DRAFT > Notifications")
- Delete account fails on backend → show error, do not clear local session

---

## 6. Push notifications

DRAFT uses 4 types of push notifications:


| Type              | Content                                                                                           | Tap action                               |
| ----------------- | ------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| Friend added      | "[Name] just joined Draft" or "[Name] is now your friend"                                         | Opens friends tab                        |
| Post about you    | "New post about you in your feed"                                                                 | Opens home feed, scrolls to post         |
| Post in your feed | "New post from [Friend name]"                                                                     | Opens home feed                          |
| DRAFT check-in    | Chat message preview showing the actual question text (e.g. "Hey, how's your week going so far?") | Opens bottom sheet with the conversation |


The check-in notification is designed to feel like a personal message, not an app alert. It displays the question text as a preview, like a chat notification.

---

## 7. Post styles

> DRAFT writes every post in one of four styles. The style shapes the tone and framing — not the facts. Every post is warm, personal, and always written in the third person about the subject.

### Writing rules

- **Person:** Always 3rd person. Never "you" or "I".
- **Headline:** 4–8 words. Max 55 characters. No period.
- **Body:** 1–2 sentences. Max 160 characters total.
- **Names:** Use first name only, exactly as entered at onboarding.
- **Tone baseline:** Warm across all styles. Roast is never mean.
- **Facts:** Never fabricate. Only write what the user actually shared.
- **Privacy:** Never publish anything that could embarrass the user. If the user explicitly said they don't want something shared, respect it — do not include it in any post.
- **Consent:** When in doubt, omit. The guardrail is a prompt engineering requirement, not a product feature.

---

### Styles

#### Hype

Headline frames the situation as something worth celebrating — even when it's mundane. Body adds genuine warmth. Reads like the most supportive person in the group chat.

- Lead with what went right, even if small.
- Body adds genuine warmth, not hype for its own sake.
- Don't oversell — understatement with warmth lands better than enthusiasm.

**Headline:** A simple weekend done right
**Body:** He went home, slowed down, and kept things easy. No chaos, no pressure, just a clean reset with family.

---

#### Roast

Headline names the slightly ridiculous thing. Body lands the punchline with one extra detail. Warm, not mean — the kind of thing a close friend would say to your face.

- Headline calls out the absurdity with precision.
- Body lands the punchline with one extra detail.
- Never punch down — the joke is about the situation, not the person.

**Headline:** Maximum driving, minimum actual weekend
**Body:** Spent more time on the road than doing anything else. Saturday there, Sunday back, that's the whole story.

---

#### Teaser

Headline drops just enough to make you curious. Body adds one detail but leaves a gap — the kind that makes you want to message them. DRAFT as conversation starter.

- Headline hints without explaining.
- Body adds one detail but withholds the conclusion.
- Leave the gap intentional — the post should make friends reach out.

**Headline:** His "chill" weekend sounds… interesting
**Body:** Just a quick trip to his parents, nothing special he said. But it felt like he skipped over something.

---

#### Gossip

Headline sounds like something whispered across a table. Body leans in with one more detail. Conspiratorial and warm — makes the mundane feel like actual news.

- Headline frames it as something worth leaning in for.
- Body adds one more detail with a knowing tone.
- Keep it warm — conspiratorial, never malicious.

**Headline:** He quietly went home this weekend
**Body:** Didn't make a thing of it, just slipped it in casually. But the quick in-and-out feels a bit telling.

---

### Style selection

DRAFT picks a style at random — the style is randomized by the backend and passed as a variable to the post generation prompt. The LLM does not choose styles. Each of the four styles has an equal 25% chance of being selected. This ensures variety in the feed and provides an even distribution to measure engagement per style from the first cohort.

---

## 8. Data / Dashboard

> This section defines the Amplitude charts to build before launch. Each chart maps directly to a success metric or a key funnel. Charts are built from the events defined in §10.

---

### North star charts


| Chart                  | Hypothesis | Calculation                                                 | Events                                                            |
| ---------------------- | ---------- | ----------------------------------------------------------- | ----------------------------------------------------------------- |
| Daily Engagement Rate  | H2         | `% of DAU who liked or shared ≥ 1 post` (rolling 7-day)     | Post Liked, Post Shared                                           |
| Prompt Completion Rate | H1         | `count(Prompt Completed) / count(Prompt Sent)` per day      | Prompt Completed, Push Sent                                       |
| D7 Retention           | —          | `users with ≥1 session on day 7 / users installed on day 0` | [Amplitude] Application Installed, [Amplitude] Application Opened |


---

### Supporting funnel charts


| Chart                      | Purpose                                      | Calculation                                                                                             | Events                                                                                        |
| -------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Onboarding Funnel          | Identify drop-off steps                      | Step conversion: Splash → Name → Age → Gender → Push Permission → Account Created → First post received | Onboarding Step Viewed, Onboarding Step Completed, Push Permission Completed, Account Created |
| Friends Added (First 24h)  | Measure activation — is the graph seeded?    | `avg(Friend Added) per user in first 24h after Account Created`                                         | Friend Added, Account Created                                                                 |
| Average Posts Available    | Is there enough content in the feed?         | `count(distinct post_id seen) / unique active users` per day                                            | Post Viewed                                                                                   |
| Question → Post Conversion | How much signal turns into a published post? | `count(Post Generated) / count(Prompt Completed)`                                                       | Prompt Completed, Post Generated (backend — to add)                                           |
| Push Opt-in Rate           | Measure notification permission rate         | `Push Permission Completed (successful=true) / Push Permission Viewed`                                  | Push Permission Viewed, Push Permission Completed                                             |
| New Users (Last 7 Days)    | Track acquisition pace                       | `count(Account Created)` rolling 7-day                                                                  | Account Created                                                                               |
| Prompt Follow-up Rate      | Measure depth of exchange                    | `Prompt Completed (is_followup=true) / Prompt Completed (is_followup=false)`                            | Prompt Completed                                                                              |
| Post Interaction Breakdown | Understand split of like vs share            | `count(Post Liked)` vs `count(Post Shared)` split by type                                               | Post Liked, Post Shared                                                                       |
| Invite Link Share Rate     | Measure growth loop activation               | `count(Invite Link Shared) / DAU`                                                                       | Invite Link Shared                                                                            |


> `[CLARIFY]` A "Post Generated" backend event is needed to measure post generation rate (posts generated / Prompt Completed). Must be added to the analytics plan before build. — owner: engineering

### Feed density target

Minimum viable feed density = **4 posts per day** for a 6-person group. Quality matters more than quantity — if posts are genuinely interesting, 4 is enough. If posts are mediocre, no amount compensates. The "Average Posts Available" chart above should be monitored against this threshold.

---

## 9. Prompt architecture

DRAFT uses three distinct prompts, each with a specific role. Themes and styles are **hardcoded in the backend** and passed as variables to the prompts, the LLM does not choose them.

### Check-in prompt

**Purpose**: Send the push notification and opening question.
**Variables**: theme (randomized by backend from ~15 themes) + full conversation history + history of posts already written about the user.
**Behaviour**: Sends a question oriented toward the selected theme, referencing past exchanges when relevant. The question appears as a chat message preview in the push notification.

### Main prompt

**Purpose**: Handle the follow-up conversation after the user responds.
**Variables**: current conversation history.
**Behaviour**: Curious but not a companion. Asks direct follow-up questions for the first 3 exchanges (curiosity mode). From exchange 4, switches to soft/curious mode without direct questions. Stops naturally — doesn't try to keep the conversation going indefinitely.

### Post generation prompt

**Purpose**: Generate the post from the conversation.
**Variables**: conversation history from the current exchange + style (randomized by backend from 4 styles).
**Behaviour**: Triggered 30 minutes after the user's last message. Takes the conversation and writes a post in the assigned style. Output structure: headline + body, following the writing rules defined in §7.

### Themes

~15 themes to be defined. Examples discussed: future plans, weekend, music, travel, relationships. Themes are passed as variables to the check-in prompt — one theme per question.

**Owner**: Margaux. To do.

### Styles

4 styles defined in §7 (Hype, Roast, Teaser, Gossip). Elaborate definitions to be written.

**Owner**: Aymeric. To do.

### Prompt writing

- Check-in + main prompts — **owner: Margaux**
- Post generation prompt + style integration — **owner: Aymeric**

---

## 10. Analytics event definitions

> Naming convention: Title Case, Object followed by past-tense verb (e.g. "Post Viewed", "Prompt Dismissed"). Aligns with Amon standard.
> All iOS events include `current_view` (required) and `previous_view` (optional) as standard properties on every event.

### Common events

Events shared with Frank and Orai. Reuse the existing Amplitude schema as-is — do not duplicate or rename.


| Event                                | Category     | Source  | Notes                                                                                                               |
| ------------------------------------ | ------------ | ------- | ------------------------------------------------------------------------------------------------------------------- |
| [Amplitude] Application Installed    | —            | ios     | Auto-tracked by Amplitude SDK                                                                                       |
| [Amplitude] Application Opened       | —            | ios     | Auto-tracked by Amplitude SDK                                                                                       |
| [Amplitude] Application Backgrounded | —            | ios     | Auto-tracked by Amplitude SDK                                                                                       |
| [Amplitude] Application Updated      | —            | ios     | Auto-tracked by Amplitude SDK                                                                                       |
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

## 11. Monetization

> Not implemented in MVP. Trigger: begin monetization work when D7 retention ≥ 35%.

**Hard paywall** → No. Destroys the friend density required for a social app to work. Creates churn that leaves remaining users with too few friends on the platform.

**Advertising** → Possible as a secondary lever, but not central. Text-based content in a free-scroll feed generates low ad revenue compared to video formats. Can be explored for optimisation, not as the primary model.

**Main model: IAP consumables**

- Unlock specific posts (you see the headline but pay to read the body)
- Secret posts with name hidden — pay to reveal who it's about
- Weekly editions (curated digest of the best posts)
- Longer post versions (extended body text)
- Media unlock (photos mentioned in the conversation)

---

## 12. Open questions

**Must be resolved before build**

- `[CLARIFY]` Theme library: the full list of ~15 themes DRAFT uses to generate daily questions (e.g. weekend plans, current music, upcoming events). Must be written before build. — owner: Margaux
- `[CLARIFY]` Push notification copy: check-in notification should display the question as a chat message preview. Exact copy and format to be defined. — owner: Margaux
- `[CLARIFY]` Tone of voice when asking questions: DRAFT's register, level of warmth, and conversational style during exchanges. (Tone when writing posts is already defined in §7.) Must be defined before front-end build. — owner: Margaux
- `[CLARIFY]` Style definitions: elaborate the 4 post styles beyond the current examples. Must be detailed enough for prompt engineering. — owner: Aymeric
- `[CLARIFY]` Prompt writing: write the 3 prompts (check-in, main/chat, post generation). Check-in + main — owner: Margaux. Post generation + styles — owner: Aymeric.

**Engineering validation required**

- `[CLARIFY]` Question generation timing: generated synchronously at send time, or pre-generated a few hours in advance? To validate with engineering. — owner: engineering
- `[CLARIFY]` Deferred deep linking: technical complexity to validate with Florian and Romain before committing to MVP. — owner: engineering

**Risks**

- `[RISK]` Post quality is the core bet. If DRAFT's output feels generic or inaccurate, the input loop breaks. Mitigation: run manual prompt tests with real users before automating post generation.
- `[RISK]` Cold start: a user with zero friends sees very little. For the test phase, cohorts must include 5–6 people who know each other and onboard together. This is a hard requirement for the first test, not a nice-to-have.
- `[RISK]` Friend chain propagation: because friendship is instant (no confirmation) and suggestions cascade, a single shared link can lead to rapid, uncontrolled friend graph expansion. Anyone in the chain can see content from people they don't know. Mitigation: monitor via analytics, kill the cascade feature if it causes problems.

**Assumptions**

- `[ASSUMPTION]` Text-only conversation with no integrations generates posts worth reading. Validation: test with real users before the full build.
- `[ASSUMPTION]` Native post sharing via iOS share sheet (image format) is sufficient for MVP virality. Validation: track Post Shared rate and qualitative feedback from the first cohort.

---

*This document is the source of truth for product intent. Any implementation decision that diverges from this spec must be reflected here before the feature is marked done.*