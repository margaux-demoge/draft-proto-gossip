# DRAFT

> **Your friend group has a personal journalist.** DRAFT talks to each of you, extracts what's going on, and publishes it as a shared daily feed — so you always know what's happening in your friends' lives, without anyone having to post.

---

## 1. Overview

**Problem**

There are two reasons people have stopped sharing what's actually going on in their lives. First, creating content has become high-friction and high-pressure - you need to think of something worth posting, frame it, and accept that it will be judged. Most people don't bother. Second, the content they do consume is algorithmic and performative - platforms optimise for engagement, not relevance, so your feed is full of strangers and influencers, not the real daily updates from the people you care about. The result: you have no idea what's going on in your friends' lives, and neither do they.

**Target users**

Primary: Urban adults 18–30 with an active close friend group (5–15 people). They use WhatsApp constantly but haven't posted on Instagram in months.

Secondary: People who feel out of touch with friends post-move, post-graduation, post-breakup.

Not for: couples, family groups, public creators, professional networks.

---

## 2. Success metrics


| #   | Hypothesis                                                                                                                | Metric | Target |
| --- | ------------------------------------------------------------------------------------------------------------------------- | ------ | ------ |
| H1  | People are willing to share small moments of their life if an AI asks them — rather than having to initiate it themselves | TBD    | ≥ 60%  |
| H2  | Daily updates from friends are interesting enough to make users engage with the content                                   | TBD    | ≥ 40%  |


---

## 3. Scope

**In MVP**

- No authentication: account created automatically on device at first launch
- Minimal onboarding: name, age, gender, profile photo (camera roll upload), push permission
- Welcome card on first session: "[Name] just joined DRAFT!" with QR code and share link
- DRAFT's first question opens as a bottom sheet immediately on home feed arrival
- AI-generated cards published throughout the day, based on conversations with DRAFT
- Conversational input loop: DRAFT prompts users 2× daily (9:47 and 18:12)
- Like button on each card with visible like count
- Share button on each card: fixed label "Share", opens iOS native share sheet (image output). CTA is never AI-generated in MVP.
- Card moderation via 3-dots menu: remove card (if tagged) or report
- Chronological feed with relative timestamps, pull-to-refresh
- Push notifications for new cards and new prompts from DRAFT
- iOS only, English, US App Store

**Out of MVP**

- User-initiated input (DRAFT always initiates)
- Data integrations (Spotify, Apple Health, Photos, Calendar)
- In-app messaging
- Friend search, contact scan, and algorithmic friend suggestions (e.g. based on contacts or location). For MVP, groups are onboarded together via the ambassador programme. Basic mutual-friend suggestions (friends of friends) are included in MVP.
- Sub-group scoping
- Weekly digest or newspaper format
- Public profiles or discovery
- Card detail screen
- Quick-tap reply options on prompts
- Duplicate card detection when two friends mention the same event
- Card generation restricted to daytime hours (7am–10:30pm); MVP generates at any time
- Question sheet "closed" state: MVP always shows the last exchange, overwritten by next question
- Android, web, non-US markets

---

## 4. Flows

---

### Flow: Onboarding

**Trigger**: First app launch with no stored session.

**End state**: User is on the home feed with their welcome card visible and DRAFT's first question open in a bottom sheet.

#### Steps

1. **Splash**: Show the app logo and name. Single screen, no value prop copy needed for MVP. Auto-advances after a brief moment or on tap.
2. **Name**: The user enters their first name so DRAFT can personalise cards and questions from day one.
3. **Age**: The user enters their age. Used by DRAFT to calibrate question tone and relevance.
4. **Gender**: The user selects their gender. Used by DRAFT to personalise questions and card writing.
5. **Profile photo**: The user uploads a photo from their camera roll. Used to attribute cards in the feed and friend lists.
6. **Push permission**: Explain why notifications matter before triggering the native iOS prompt, so the user grants permission with context.
7. **Home feed**: The user lands on their feed. A welcome card is already present: "[Name] just joined DRAFT!" with a QR code and a "Share your link" CTA. DRAFT's first question opens immediately as a bottom sheet over the feed.

#### Business rules

**Account**

- No sign-up or authentication required. An account is created automatically on the device at first launch.
- If the user deletes and reinstalls the app, their account is preserved (same strategy as Orai — device token stored server-side).
- If the user switches devices, their account is lost. This is acceptable for MVP.

**Welcome card**

- The welcome card "[Name] just joined DRAFT!" is generated immediately on account creation, before the user reaches the home feed. It contains a QR code linking to the user's personal invite link and a "Share your link" CTA. Its purpose is to seed the growth loop from the very first session.
- The welcome card is visible only to the user themselves — friends do not see it. It disappears from the feed after 48h, like any other card.

**First question**

- DRAFT's first question appears as a bottom sheet over the home feed immediately on first arrival. It does not wait for the scheduled 9:47 or 18:12 prompt window.
- The first question is drawn from a dedicated onboarding question list (not the regular theme library). See `[CLARIFY]` in §8.

**Friend graph**
Friends are not added during onboarding. They are added later via the share link or QR code. The onboarding funnel does not include a friend discovery step.

**Growth**
The primary acquisition mechanic for MVP is ambassador-driven: groups of friends are onboarded together via an external programme. The in-app share link and QR code support and reinforce this.

#### Error paths & edge cases

- User quits mid-onboarding → resume from the last completed step on next launch
- User skips push permission → they can still use the app but will not receive prompt notifications. Every 24h, a full-screen skippable screen (reusing Frank's implementation) explains why notifications matter, with a CTA that redirects to iOS Settings > DRAFT > Notifications.
- User creates account and receives the onboarding first question → no scheduled prompt (9:47 / 18:12) is sent if the user already received a prompt in the last 4 hours

---

### Flow: DRAFT asks a question

**Trigger**: Scheduled prompt at 9:47 or 18:12.

**End state**: User has completed the exchange. DRAFT decides independently whether to generate a card.

#### Steps

1. **Push notification**: DRAFT sends a notification that feels like a message from a friend, not a survey prompt. It builds curiosity or references something from a previous exchange before surfacing a question. The question is never stated directly in the notification. Tapping the notification opens the app and immediately opens the chat bottom sheet in its active state. `[CLARIFY]` Exact push notification copy examples to be written before build — owner: Margaux.
2. **DRAFT question sheet**: A bottom sheet opens over the feed showing DRAFT's question. The user sees the question in full before deciding whether to answer.
3. **User reply**: The user replies via text, voice, or image (reusing the Frank/Orai chat component).
4. **Follow-up**: DRAFT continues asking follow-up questions to gather more context. The user can stop replying at any time. There is no artificial limit on the number of follow-ups. See `[CLARIFY]` in §8 for the stopping trigger.
5. **Sheet closes**: The user stops responding and dismisses the sheet. No loading, no indication of what happens next. DRAFT processes the exchange in the background and decides independently whether to generate a card.

#### Business rules

**Card generation**

- Card generation is fully decoupled from the exchange. The user never knows if their response will generate a card, when, or what it will say. The surprise is intentional and core to the product.
- The generation process starts 30 minutes after the user's first reply, not 30 minutes after DRAFT sent the question. If the user never replies, no generation is triggered from that exchange.
- DRAFT should try to generate at least 1 card per user per day using existing context, even if the user has not responded to any prompt that day.
- If a card is generated: a push notification is sent to the user's friends, and the card appears in their feed.
- If the input contains no signal (no named person, place, event, or emotional state with context): no card is generated and no feedback is given to the user.
- For MVP, cards are generated at any time of day. Future behaviour: cards are only published between 7am and 10:30pm. Responses received after 10:30pm trigger generation the next morning at 7am.

**Card timestamp**

- A card's timestamp is always the moment it is created, regardless of when the user shared the underlying information.

**Question system**

- For MVP, DRAFT selects a theme at random from the library. Each theme has an equal chance of being selected.
- `[CLARIFY]` Question generation timing: generated synchronously at send time, or pre-generated a few hours in advance? To validate with engineering.

**Prompts**

- If the user did not respond to the 9:47 prompt, the 18:12 prompt is still sent. DRAFT either follows up on the unanswered question in an intelligent way or proposes a different question. It is never a verbatim repeat.
- DRAFT references previous exchanges when relevant. For example, the next day's prompt may open with a callback to what was discussed ("Yesterday you mentioned the Netflix thing — how did it go?") before moving to a new question. This builds trust and makes the interaction feel like a real conversation rather than a survey.
- Max 2 prompts per day per user.

**Follow-up**

- There is no maximum number of follow-ups. The exchange stays open until the user stops responding. See `[CLARIFY]` in §8 for the trigger that tells DRAFT to stop and generate.

**Offline**

- If the user submits while offline: the response is stored locally with a "pending" status visible to the user.

**Bottom sheet states**

- The DRAFT question sheet has four distinct states:
(a) Minimized / active — DRAFT has a question waiting. Small bar with a glow or badge effect. Label: "DRAFT has a question for you." No message preview shown.
(b) Open — Full bottom sheet with the chat interface. Reuse the Frank/Orai chat component (text, voice, image input supported).
(c) Minimized / processing — User replied and dismissed the sheet while DRAFT is generating. Small bar shows "DRAFT is writing…". Transitions back to (a) active once ready.
(d) Minimized / idle — Nothing to do right now. Small bar in a subdued/disabled state, waiting for the next scheduled prompt.

#### Error paths & edge cases

- User dismisses without answering → no card generated from that exchange, prompt not repeated today
- User has already received 2 prompts today → no further prompts until the next day
- Network failure on submit → response stored as "pending", auto-sent on reconnect with retry logic
- User responds at 11:55pm → card generated at 12:25am, timestamped at creation time

---

### Flow: Browsing the feed

**Trigger**: User opens the app after completing onboarding.

**End state**: User has consumed at least one card and optionally taken an action.

#### Steps

1. **Home feed**: The user sees a chronological list of cards, newest first. Timestamps are displayed as relative labels ("1 hour ago", "yesterday"). No date dividers.
2. **Pull-to-refresh**: The user pulls down to refresh the feed and load new cards published since last open.
3. **Scrolling**: As the user scrolls, cards enter the viewport. A card is marked as viewed after 1 continuous second of at least 50% visibility.
4. **Like**: The user taps the like button on a card. The like count is visible to all users.
5. **Share**: The user taps the share button on a card to open the iOS native share sheet. The card is exported as an image.
6. **3-dots menu**: The user taps the 3-dots button on any card to open a context menu. Options depend on whether the card is about themselves or someone else (see Card moderation flow).
7. **New activity bubble**: When new cards are published while the user is in the app, a floating bubble appears at the bottom. Tapping it refreshes the feed and scrolls to the top.

#### Business rules

**Social graph**

- Friendship is mutual and symmetric. Both users must accept the connection before either can see the other's cards. If either user removes the friendship, both lose access to each other's content immediately.

**Visibility**

- A card is visible to all confirmed friends of its subject.
- For MVP, a card always has exactly one subject. Other first names may appear in the body text, but only the subject's profile photo and name are displayed on the card.

**Own cards**

- The user sees their own cards in the feed exactly as others do. They will recognise their own photo and name. No special layout or pinning.
- If the user has zero friends, their own cards are visible only to themselves. Empty state prompts them to invite people.

**Content window**

- The feed shows the last 48 hours of cards. Not a calendar day. Rolling 48-hour window.
- Card data is never deleted from the server. Cards are only hidden from the feed after 48h.

**Likes**

- Like count is visible to all users. Who liked is not shown in MVP.
- Tapping like on a card that the user has already liked removes the like (toggle behaviour).
- When a friend likes a card the user is mentioned in, a push notification is sent: "[Friend name] liked a post you are mentioned in."

#### Error paths & edge cases

- Feed empty, no friends → empty state with invite nudge
- Feed empty, follows present but no cards yet → "DRAFT is still gathering intel. Check back soon."
- Offline → cached feed shown with an "Offline" banner, like and share buttons disabled

---

### Flow: Card moderation

**Trigger**: User taps the 3-dots button on a card.

**End state for own card**: The card is removed from all feeds where it was visible (the user's own feed and all their friends' feeds).

**End state for another person's card**: The report is submitted.

#### Steps (own card — user is the subject)

1. **3-dots menu**: Tapping the 3-dots button opens a context menu with: "Remove this card" and Cancel.
2. **"Remove this card"**: A confirmation dialog asks the user to confirm removal. On confirm, the card is immediately deleted from all friends' feeds.

#### Steps (another person's card)

1. **3-dots menu**: Tapping the 3-dots button opens a context menu with: "Report" and Cancel.
2. **Report**: The report is submitted immediately — no category selection required. A `Content Reported` analytics event is fired (card_id + associated data) for manual team review. A confirmation modal is shown: "Thanks, our team will review this quickly."

#### Business rules

**Removal**

- Deletion is silent. Friends see the card disappear with no explanation or notification.
- The card_removed rate is tracked in analytics as a proxy for content quality.

**Permissions**

- Users can remove only cards where they are the subject. They cannot modify cards about other people.

**Report**

- All users can report any card via the 3-dots menu. Required by Apple for UGC apps.

#### Error paths & edge cases

- User taps Cancel → menu closes, no action taken, no event fired

---

### Flow: Friend management

**Trigger**: User taps the friends icon or navigates to the friend management screen.

**End state**: User has reviewed pending friend requests, their current friend list, and friend suggestions.

#### Steps

1. **Friend management screen**: A single screen with three sections: (1) pending friend requests, (2) current friend list, (3) friend suggestions.
2. **At the top of the screen**: A persistent CTA to share the user's invite link or display their QR code, so they can grow their friend list at any time.
3. **Accept request**: The requesting user is added as a mutual friend immediately. Both users now see each other's cards.
4. **Reject request**: The request is removed silently. The requesting user is not notified.
5. **Remove friend**: A confirmation prompt before removal. Both users immediately lose access to each other's cards.
6. **Friend suggestions**: Shown below the friend list once the user has ≥ 1 confirmed friend. Displays friends of friends (deduped). Tapping "Add as friend" sends a request and the button switches to a "Pending" state.

#### Business rules

**Friend requests**

- There is no in-app search for users. Friends are added exclusively via invite link, QR code, or friend suggestions.

**Friend suggestions**

- The friend suggestions section appears once the user has at least one confirmed friend. It shows friends of friends only — no other discovery logic for MVP. Results are deduped. Ordering is by simplest available signal (e.g. account creation date). Tapping "Add as friend" sends a friend request and the button enters a "Pending" state.

**Share CTA**

- The top of the friend management screen always shows a CTA to share the user's personal invite link or display their QR code. This mirrors the welcome card and keeps the growth loop accessible at all times.

**Removal**

- Friendship removal is symmetric and immediate. Neither user sees the other's cards after removal.

#### Error paths & edge cases

- User rejects a request → request disappears, no notification sent to requester
- User removes a friend → both lose access to each other's cards immediately; cards already in the feed disappear on next refresh

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

- Profile: Name (editable), Profile photo (editable). Access: Notifications. Community: Help, Submit Feature Request, Give a Review. Legal: Privacy Policy, Terms of Service. Danger Zone: Delete Account. Footer: app version, build number, User ID.
- There is no separate profile tab in MVP. Name and profile photo are editable directly from Settings.
- Reuse the generic Amon settings component already used in Orai and Frank.

**Delete account**

- Account deletion is permanent and irreversible. All cards authored by the user are removed from all friends' feeds immediately. The user's friends are not notified.

#### Error paths & edge cases

- Deep link to iOS settings fails → show a manual instruction ("Go to Settings > DRAFT > Notifications")
- Delete account fails on backend → show error, do not clear local session

---

## 5. Card styles

> DRAFT writes every card in one of four styles. The style shapes the tone and framing — not the facts. Every card is warm, personal, and always written in the third person about the subject.

### Writing rules

- **Person:** Always 3rd person. Never "you" or "I".
- **Headline:** 4–8 words. Max 55 characters. No period.
- **Body:** 1–2 sentences. Max 160 characters total.
- **Names:** Use first name only, exactly as entered at onboarding.
- **Tone baseline:** Warm across all styles. Roast is never mean.
- **Facts:** Never fabricate. Only write what the user actually shared.

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
- Leave the gap intentional — the card should make friends reach out.

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

DRAFT picks a style at random — each of the four styles has an equal 25% chance of being selected each time a card is generated. This ensures variety in the feed and provides an even distribution to measure engagement per style from the first cohort.

---

## 6. Data / Dashboard

> This section defines the Amplitude charts to build before launch. Each chart maps directly to a success metric or a key funnel. Charts are built from the events defined in §7.

---

### North star charts


| Chart                  | Hypothesis | Calculation                                                 | Events                                                            |
| ---------------------- | ---------- | ----------------------------------------------------------- | ----------------------------------------------------------------- |
| Daily Engagement Rate  | H2         | `% of DAU who liked or shared ≥ 1 card` (rolling 7-day)     | Card Liked, Card Shared                                           |
| Prompt Completion Rate | H1         | `count(Prompt Completed) / count(Prompt Sent)` per day      | Prompt Completed, Push Sent                                       |
| D7 Retention           | —          | `users with ≥1 session on day 7 / users installed on day 0` | [Amplitude] Application Installed, [Amplitude] Application Opened |


---

### Supporting funnel charts


| Chart                      | Purpose                                      | Calculation                                                                                                             | Events                                                                                        |
| -------------------------- | -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Onboarding Funnel          | Identify drop-off steps                      | Step conversion: Splash → Name → Age → Gender → Profile Photo → Push Permission → Account Created → First card received | Onboarding Step Viewed, Onboarding Step Completed, Push Permission Completed, Account Created |
| Friends Added (First 24h)  | Measure activation — is the graph seeded?    | `avg(Friend Request Accepted) per user in first 24h after Account Created`                                              | Friend Request Accepted, Account Created                                                      |
| Average Cards Available    | Is there enough content in the feed?         | `count(distinct card_id seen) / unique active users` per day                                                            | Card Viewed                                                                                   |
| Question → Card Conversion | How much signal turns into a published card? | `count(Card Generated) / count(Prompt Completed)`                                                                       | Prompt Completed, Card Generated (backend — to add)                                           |
| Push Opt-in Rate           | Measure notification permission rate         | `Push Permission Completed (successful=true) / Push Permission Viewed`                                                  | Push Permission Viewed, Push Permission Completed                                             |
| New Users (Last 7 Days)    | Track acquisition pace                       | `count(Account Created)` rolling 7-day                                                                                  | Account Created                                                                               |
| Prompt Follow-up Rate      | Measure depth of exchange                    | `Prompt Completed (is_followup=true) / Prompt Completed (is_followup=false)`                                            | Prompt Completed                                                                              |
| Card Interaction Breakdown | Understand split of like vs share            | `count(Card Liked)` vs `count(Card Shared)` split by type                                                               | Card Liked, Card Shared                                                                       |
| Invite Link Share Rate     | Measure growth loop activation               | `count(Invite Link Shared) / DAU`                                                                                       | Invite Link Shared                                                                            |


> `[CLARIFY]` A "Card Generated" backend event is needed to measure card generation rate (cards generated / Prompt Completed). Must be added to the analytics plan before build. — owner: engineering

---

## 7. Analytics event definitions

> Naming convention: Title Case, Object followed by past-tense verb (e.g. "Card Viewed", "Prompt Dismissed"). Aligns with Amon standard.
> All iOS events include `current_view` (required) and `previous_view` (optional) as standard properties on every event.

### Common events

Events shared with Frank and Orai. Reuse the existing Amplitude schema as-is — do not duplicate or rename.


| Event                                | Category     | Source  | Notes                                                                                                         |
| ------------------------------------ | ------------ | ------- | ------------------------------------------------------------------------------------------------------------- |
| [Amplitude] Application Installed    | —            | ios     | Auto-tracked by Amplitude SDK                                                                                 |
| [Amplitude] Application Opened       | —            | ios     | Auto-tracked by Amplitude SDK                                                                                 |
| [Amplitude] Application Backgrounded | —            | ios     | Auto-tracked by Amplitude SDK                                                                                 |
| [Amplitude] Application Updated      | —            | ios     | Auto-tracked by Amplitude SDK                                                                                 |
| Account Created                      | Amon Default | backend | Same schema as Frank. `account_id` required.                                                                  |
| Account Deleted                      | Amon Default | backend | Same schema as Frank. `account_id` required.                                                                  |
| Onboarding Step Viewed               | Amon Default | ios     | `step_name` enum must be updated for DRAFT steps: `splash, name, age, gender, profile_photo, push_permission` |
| Onboarding Step Completed            | Amon Default | ios     | Same as above for `step_name`                                                                                 |
| Push Permission Viewed               | Amon Default | ios     | Identical to Frank                                                                                            |
| Push Permission Completed            | Amon Default | ios     | Identical to Frank                                                                                            |
| Push Clicked                         | Amon Default | ios     | `type` enum must be updated: `card, prompt` (replaces Frank's `message, checkin, broadcast, insight`)         |
| Setting Clicked                      | Amon Default | ios     | `setting_name` values to define for DRAFT settings screen                                                     |


---

### App-specific events

Events new to DRAFT. Full property definitions below.

```yaml
TBD when flows are ready.
```

---

## 8. Open questions

**Must be resolved before build**

- `[CLARIFY]` Theme library: the full list of 10–30 themes DRAFT uses to generate daily questions (e.g. weekend plans, current music, upcoming events). Must be written before build. — owner: Margaux
- `[CLARIFY]` Push notification copy: first notification should feel like a personal message from DRAFT (not a direct question), referencing past context. Exact copy and format to be defined. — owner: Margaux
- `[CLARIFY]` Tone of voice when asking questions: DRAFT's register, level of warmth, and conversational style during exchanges. (Tone when writing cards is already defined in §5.) Must be defined before front-end build. — owner: Margaux
- `[CLARIFY]` Onboarding question list: a dedicated set of questions for the first onboarding exchange must be written. Each question should be specific enough that a single reply generates a card. Must include party-context variants (e.g. "How are you planning to end the night?"). — owner: Margaux
- `[CLARIFY]` Follow-up stopping trigger: define the signal or rule that tells DRAFT to stop asking follow-ups and begin generating the card. — owner: Aymeric + engineering
- `[CLARIFY]` Prompt specification: create a dedicated section in the spec to list and describe all prompt types (daily prompts, onboarding question, follow-ups), their exact behaviour, and the rules governing when each fires. — owner: Margaux

**Engineering validation required**

- `[CLARIFY]` Question generation timing: generated synchronously at send time, or pre-generated a few hours in advance? To validate with engineering. — owner: engineering
- `[CLARIFY]` Deferred deep linking: technical complexity to validate with Florian and Romain before committing to MVP. — owner: engineering

**Risks**

- `[RISK]` Card quality is the core bet. If DRAFT's output feels generic or inaccurate, the input loop breaks. Mitigation: run manual prompt tests with real users before automating card generation.
- `[RISK]` Cold start: a user with zero friends sees very little. For the test phase, cohorts must include 5–6 people who know each other and onboard together. This is a hard requirement for the first test, not a nice-to-have.

**Assumptions**

- `[ASSUMPTION]` Text-only conversation with no integrations generates cards worth reading. Validation: test with real users before the full build.
- `[ASSUMPTION]` Native card sharing via iOS share sheet (image format) is sufficient for MVP virality. Validation: track Card Shared rate and qualitative feedback from the first cohort.

---

*This document is the source of truth for product intent. Any implementation decision that diverges from this spec must be reflected here before the feature is marked done.*