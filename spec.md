---
title: "DRAFT — Spec"
version: "1.3"
status: "draft"
last_updated: "2026-03-27"
owner: "Aymeric"
---

# DRAFT

> **Your friend group has a personal journalist.** DRAFT talks to each of you, extracts what's going on, and publishes it as a shared daily feed — so you always know what's happening in your friends' lives, without anyone having to post.

> `[CLARIFY]` "DRAFT" is a codename. Final app name to be brainstormed before public launch.

---

## 1. Overview

**Problem**

Nobody shares what's actually going on in their life anymore. Social platforms became either performative (Instagram as personal brand) or algorithmically driven away from actual friends (TikTok). The real updates (what you're doing this weekend, that you're stressed at work, that you're going to a concert) stay trapped in your head because there's no low-friction way to share them with the people who care.

**Target users**

Primary: Urban adults 18–30 with an active close friend group (5–15 people). They use WhatsApp constantly but haven't posted on Instagram in months.

Secondary: People who feel out of touch with friends post-move, post-graduation, post-breakup.

Not for: couples, family groups, public creators, professional networks.

---

## 2. Success metrics


| Hypothesis                                                                 | Metric                                            | Target |
| -------------------------------------------------------------------------- | ------------------------------------------------- | ------ |
| Draft posts are interesting enough to make users engage daily with friends | Daily readers interacting with ≥ 1 card           | ≥ 40%  |
| Users answer when DRAFT asks                                               | Prompt Completed / Prompt Viewed per user per day | ≥ 60%  |
| Cards are worth sharing                                                    | Card Shared events / sessions                     | ≥ 0.5  |
| The product creates a daily habit                                          | Users with ≥ 1 session on day 7                   | ≥ 35%  |


---

## 3. Scope

**In MVP**

- No authentication: account created automatically on device at first launch
- Minimal onboarding: name, age, gender, push permission only
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
5. **Push permission**: Explain why notifications matter before triggering the native iOS prompt, so the user grants permission with context.
6. **Home feed**: The user lands on their feed. A welcome card is already present: "[Name] just joined DRAFT!" with a QR code and a "Share your link" CTA. DRAFT's first question opens immediately as a bottom sheet over the feed.

#### Business rules

- [account] No sign-up or authentication required. An account is created automatically on the device at first launch. `[CLARIFY]` Account persistence: if the user deletes and reinstalls the app or switches devices, they lose their account. What is the acceptable MVP strategy — accept the loss, or store a device token server-side? To validate with engineering.
- [welcome card] The welcome card "[Name] just joined DRAFT!" is generated immediately on account creation, before the user reaches the home feed. It contains a QR code linking to the user's personal invite link and a "Share your link" CTA. Its purpose is to seed the growth loop from the very first session.
- [welcome card] The welcome card persists permanently in the feed as a regular chronological post. It is not pinned and does not disappear when friends are added.
- [first question] DRAFT's first question appears as a bottom sheet over the home feed immediately on first arrival. It does not wait for the scheduled 9:47 or 18:12 prompt window.
- [first question] The first question is drawn from the theme library like any other daily prompt, but is chosen to be open-ended enough to work without any prior user context.
- [friend graph] Friends are not added during onboarding. They are added later via the share link, QR code, or contact discovery. The onboarding funnel does not include a friend discovery step.
- [growth] The primary acquisition mechanic for MVP is ambassador-driven: groups of friends are onboarded together via an external programme. The in-app share link and QR code support and reinforce this.

#### Error paths & edge cases

- User quits mid-onboarding → resume from the last completed step on next launch
- User skips push permission → they can still use the app but will not receive prompt notifications; an in-app nudge is shown later
- Welcome card QR code or share link fails to generate → show the card without the QR code with a retry button

---

### Flow: DRAFT asks a question

**Trigger**: Scheduled prompt at 9:47 or 18:12.

**End state**: User has completed the exchange. DRAFT decides independently whether to generate a card.

#### Steps

1. **Push notification**: DRAFT sends a notification that feels like a message from a friend, not a survey prompt. It builds curiosity or references something from a previous exchange before surfacing a question. The question is never stated directly in the notification. `[CLARIFY]` Exact push notification copy examples to be written before build — owner: Aymeric.
2. **DRAFT question sheet**: A bottom sheet opens over the feed showing DRAFT's question and her avatar. The user sees the question in full before deciding whether to answer.
3. **User reply**: The user types a free-text response.
4. **Follow-up** (optional): DRAFT may ask one follow-up question to get more detail. The user can answer or dismiss; both close the active exchange.
5. **Sheet closes**: The exchange is complete from the user's perspective. No loading, no indication of what happens next. DRAFT processes the response in the background.

#### Business rules

- [card generation] Card generation is fully decoupled from the exchange. The user never knows if their response will generate a card, when, or what it will say. The surprise is intentional and core to the product.
- [card generation] The generation process starts 30 minutes after the user's first reply, not 30 minutes after DRAFT sent the question. If the user never replies, no generation is triggered from that exchange.
- [card generation] DRAFT should try to generate at least 1 card per user per day using existing context, even if the user has not responded to any prompt that day.
- [card generation] If a card is generated: a push notification is sent to the user's followers, and the card appears in their feed.
- [card generation] If the input contains no signal (no named person, place, event, or emotional state with context): no card is generated and no feedback is given to the user.
- [card generation] For MVP, cards are generated at any time of day. Future behaviour: cards are only published between 7am and 10:30pm. Responses received after 10:30pm trigger generation the next morning at 7am.
- [card timestamp] A card's timestamp is always the moment it is created, regardless of when the user shared the underlying information.
- [question system] DRAFT selects a theme from a hardcoded library of 10–30 themes (e.g. weekend plans, current music, upcoming events). She then generates a unique question tailored to that specific user based on everything she knows about them: onboarding context, previous exchanges, and history of cards written about them and their friends.
- [question system] `[CLARIFY]` What is the full list of themes in the library? Must be written before build.
- [question system] `[CLARIFY]` What is the selection logic: random theme, or contextually chosen based on recency and user profile?
- [question system] `[CLARIFY]` Question generation timing: generated synchronously at send time, or pre-generated a few hours in advance? To validate with engineering.
- [second prompt] If the user did not respond to the 9:47 prompt, the 18:12 prompt is still sent. DRAFT either follows up on the unanswered question in an intelligent way or proposes a different question. It is never a verbatim repeat.
- [prompt continuity] DRAFT references previous exchanges when relevant. For example, the next day's prompt may open with a callback to what was discussed ("Yesterday you mentioned the Netflix thing — how did it go?") before moving to a new question. This builds trust and makes the interaction feel like a real conversation rather than a survey.
- [check-in limit] Max 2 prompts per day per user.
- [follow-up] Max 1 follow-up question from DRAFT. The exchange closes after that regardless.
- [offline] If the user submits while offline: the response is stored locally with a "pending" status visible to the user.
- [bottom sheet states] `[CLARIFY]` The DRAFT question sheet has at least three distinct states that need to be designed: (a) active — DRAFT is waiting for a response; (b) processing — the user has just replied and DRAFT is doing her thing (some equivalent of "DRAFT is writing…"); (c) idle — nothing to do right now, waiting for the next scheduled prompt. Each state needs a defined UI treatment before build. It is auto-sent when the connection is restored. If sending fails repeatedly, a red error indicator is shown with an option to retry. Reuse the same implementation as Orai and Frank.

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
5. **Share**: The user taps the share button on a card to open the iOS native share sheet. The card is exported as an image. `[CLARIFY]` Explore technical feasibility of a link with automatic card preview instead of or in addition to the image.
6. **3-dots menu**: The user taps the 3-dots button on any card to open a context menu. Options depend on whether the card is about themselves or someone else (see Card moderation flow).
7. **New activity bubble**: When new cards are published while the user is in the app, a floating bubble appears at the bottom. Tapping it refreshes the feed and scrolls to the top.

#### Business rules

- [social graph] Friendship is mutual and symmetric. Both users must accept the connection before either can see the other's cards. If either user removes the friendship, both lose access to each other's content immediately.
- [visibility] A card is visible to all confirmed friends of its subject.
- [visibility] If a card mentions multiple people, all mentioned users also see it in their feed, regardless of whether they follow each other.
- [own cards] The user sees their own cards in the feed exactly as others do. They will recognise their own photo and name. No special layout or pinning.
- [own cards] If the user has zero friends, their own cards are visible only to themselves. Empty state prompts them to invite people.
- [content window] The feed shows the last 48 hours of cards. Not a calendar day. Rolling 48-hour window.
- [data retention] Card data is never deleted from the server. Cards are only hidden from the feed after 48h.
- [likes] Like count is visible to all users. Who liked is not shown in MVP.

#### Error paths & edge cases

- Feed empty, no follows → empty state with invite nudge
- Feed empty, follows present but no cards yet → "DRAFT is still gathering intel. Check back soon."
- Offline → cached feed shown with an "Offline" banner, like and share buttons disabled

---

### Flow: Card moderation

**Trigger**: User taps the 3-dots button on a card.

**End state for own card**: The card is either corrected (regenerated by DRAFT) or removed from all followers' feeds.

**End state for another person's card**: The report is submitted.

#### Steps (own card — user is the subject)

1. **3-dots menu**: Tapping the 3-dots button opens a context menu with: "This is wrong", "Remove this card", and Cancel.

2a. **"This is wrong"**: A text field opens pre-filled with the card's current content. The user edits the inaccurate part and submits the correction to DRAFT.
2b. **"Remove this card"**: A confirmation dialog asks the user to confirm removal. On confirm, the card is immediately deleted from all followers' feeds.

#### Steps (another person's card)

1. **3-dots menu**: Tapping the 3-dots button opens a context menu with: "Report" and Cancel.
2. **Report**: `[CLARIFY]` Report categories and handling process to be defined. Required by Apple App Store guidelines for UGC apps.

#### Business rules

- [correction] DRAFT regenerates the card using the corrected input. The updated card replaces the original in all followers' feeds.
- [removal] Deletion is silent. Friends see the card disappear with no explanation or notification.
- [permissions] Users can correct or remove only cards where they are the subject. They cannot modify cards about other people.
- [report] All users can report any card via the 3-dots menu. Required by Apple for UGC apps.
- `[CLARIFY]` How does the user indicate what is wrong? Do they edit the full card as free text, or is there a more structured correction flow? Must be defined before building the moderation UI.

#### Error paths & edge cases

- Correction submitted but DRAFT fails to regenerate → remove the card silently and notify the user: "DRAFT couldn't rewrite this one. The card has been removed."
- User taps Cancel → menu closes, no action taken, no event fired

---

### Flow: Friend management

**Trigger**: User taps the friends icon or navigates to the friend management screen.

**End state**: User has reviewed pending friend requests and their current friend list.

#### Steps

1. **Friend management screen**: A single screen with two sections. At the top: pending friend requests with Accept and Reject actions. Below: the user's current friend list with a Remove option per friend.
2. **At the top of the screen**: A persistent CTA to share the user's invite link or display their QR code, so they can grow their friend list at any time.
3. **Accept request**: The requesting user is added as a mutual friend immediately. Both users now see each other's cards.
4. **Reject request**: The request is removed silently. The requesting user is not notified.
5. **Remove friend**: A confirmation prompt before removal. Both users immediately lose access to each other's cards.

#### Business rules

- [friend requests] There is no in-app search for users. Friends are added exclusively via invite link, QR code, or friend suggestions.
- [friend suggestions] Once a user has at least one confirmed friend, they see suggestions based on mutual friends. `[CLARIFY]` Friend suggestion friction: in a party setting, each person still needs to individually send and accept requests, even with suggestions visible. This adds significant friction when trying to connect a large group quickly. A solution (e.g. group QR code, batch add) needs to be explored before the first test cohort.
- [share CTA] The top of the friend management screen always shows a CTA to share the user's personal invite link or display their QR code. This mirrors the welcome card and keeps the growth loop accessible at all times.
- [removal] Friendship removal is symmetric and immediate. Neither user sees the other's cards after removal.

#### Error paths & edge cases

- User rejects a request → request disappears, no notification sent to requester
- User removes a friend → both lose access to each other's cards immediately; cards already in the feed disappear on next refresh

---

### Flow: Settings

**Trigger**: User navigates to Settings from the Profile tab.

**End state**: User has updated a setting or taken an account action.

#### Steps

1. **Settings screen**: The user sees a list of options grouped by category (see content below).
2. **Permission settings**: Tapping Notifications or Contacts deep-links to the relevant iOS system settings screen.
3. **Account actions**: Sign Out logs the user out and returns to the splash screen. Delete Account triggers a confirmation dialog before permanent deletion.

#### Business rules

- [settings content] Access: Notifications, Contacts. Community: Help, Submit Feature Request, Give a Review. Legal: Privacy Policy, Terms of Service. Account: Sign Out. Danger Zone: Delete Account. Footer: app version, build number, User ID.
- [component] Reuse the generic Amon settings component already used in Orai and Frank.
- [delete account] Account deletion is permanent and irreversible. All cards authored by the user are removed from all followers' feeds immediately. The user's friends are not notified.
- [sign out] In MVP with no authentication, Sign Out clears the local session. The user loses access to their account if they cannot restore it. `[CLARIFY]` Sign out behaviour must be validated with engineering given the no-auth MVP strategy.

#### Error paths & edge cases

- Deep link to iOS settings fails → show a manual instruction ("Go to Settings > DRAFT > Notifications")
- Delete account fails on backend → show error, do not clear local session

---

## 5. Data / Dashboard

> This section defines the Amplitude charts to build before launch. Each chart maps directly to a success metric or a key funnel. Charts are built from the events defined in §6.

---

### North star charts


| Chart                   | Hypothesis validated                   | Calculation                                                 | Events                                                            |
| ----------------------- | -------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------- |
| Daily Reader Rate       | Users engage daily with friends' cards | `unique(Card Liked OR Card Shared OR Card Moderated) / MAU` | Card Liked, Card Shared, Card Moderated                           |
| Prompt Response Rate    | Users answer when DRAFT asks           | `unique(Prompt Completed) / unique(Prompt Viewed)` per day  | Prompt Viewed, Prompt Completed                                   |
| Card Shares per Session | Cards are worth sharing                | `count(Card Shared) / count(Application Opened)`            | Card Shared, [Amplitude] Application Opened                       |
| D7 Retention            | The product creates a daily habit      | `users with ≥1 session on day 7 / users installed on day 0` | [Amplitude] Application Installed, [Amplitude] Application Opened |


---

### Supporting funnel charts


| Chart                      | Purpose                                | Calculation                                                                       | Events                                                                                        |
| -------------------------- | -------------------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Onboarding Funnel          | Identify drop-off steps                | Step conversion: Splash → Name → Age → Gender → Push Permission → Account Created | Onboarding Step Viewed, Onboarding Step Completed, Push Permission Completed, Account Created |
| Push Opt-in Rate           | Measure notification permission rate   | `Push Permission Completed (successful=true) / Push Permission Viewed`            | Push Permission Viewed, Push Permission Completed                                             |
| New Users (Last 7 Days)    | Track acquisition pace                 | `count(Account Created)` rolling 7-day                                            | Account Created                                                                               |
| Prompt Follow-up Rate      | Measure depth of exchange              | `Prompt Completed (is_followup=true) / Prompt Completed (is_followup=false)`      | Prompt Completed                                                                              |
| Card Interaction Breakdown | Understand how users engage with cards | `count(Card Liked) + count(Card Shared) + count(Card Moderated)` split by type    | Card Liked, Card Shared, Card Moderated                                                       |
| Invite Link Share Rate     | Measure growth loop activation         | `count(Invite Link Shared) / DAU`                                                 | Invite Link Shared                                                                            |


> `[CLARIFY]` A "Card Generated" backend event is needed to measure card generation rate (cards generated / Prompt Completed). Must be added to the analytics plan before build. — owner: engineering

---

## 6. Analytics event definitions

> Naming convention: Title Case, Object followed by past-tense verb (e.g. "Card Viewed", "Prompt Dismissed"). Aligns with Amon standard.
> All iOS events include `current_view` (required) and `previous_view` (optional) as standard properties on every event.

### Common events

Events shared with Frank and Orai. Reuse the existing Amplitude schema as-is — do not duplicate or rename.


| Event                                | Category     | Source  | Notes                                                                                                 |
| ------------------------------------ | ------------ | ------- | ----------------------------------------------------------------------------------------------------- |
| [Amplitude] Application Installed    | —            | ios     | Auto-tracked by Amplitude SDK                                                                         |
| [Amplitude] Application Opened       | —            | ios     | Auto-tracked by Amplitude SDK                                                                         |
| [Amplitude] Application Backgrounded | —            | ios     | Auto-tracked by Amplitude SDK                                                                         |
| [Amplitude] Application Updated      | —            | ios     | Auto-tracked by Amplitude SDK                                                                         |
| Account Created                      | Amon Default | backend | Same schema as Frank. `account_id` required.                                                          |
| Account Deleted                      | Amon Default | backend | Same schema as Frank. `account_id` required.                                                          |
| Onboarding Step Viewed               | Amon Default | ios     | `step_name` enum must be updated for DRAFT steps: `splash, name, age, gender, push_permission`        |
| Onboarding Step Completed            | Amon Default | ios     | Same as above for `step_name`                                                                         |
| Push Permission Viewed               | Amon Default | ios     | Identical to Frank                                                                                    |
| Push Permission Completed            | Amon Default | ios     | Identical to Frank                                                                                    |
| Push Clicked                         | Amon Default | ios     | `type` enum must be updated: `card, prompt` (replaces Frank's `message, checkin, broadcast, insight`) |
| Setting Clicked                      | Amon Default | ios     | `setting_name` values to define for DRAFT settings screen                                             |


---

### App-specific events

Events new to DRAFT. Full property definitions below.

```yaml
events:
  - name: Friend Request Accepted
    description: "Tracks when a user accepts a friend request. Fires on tap of Accept button, after mutual friendship is established."
    source: ios
    properties:
      - name: current_view
        type: string
        required: true

  - name: Friend Request Rejected
    description: "Tracks when a user rejects a friend request. Fires on tap of Reject button."
    source: ios
    properties:
      - name: current_view
        type: string
        required: true

  - name: Friend Removed
    description: "Tracks when a user removes an existing friend. Fires on confirmation of removal."
    source: ios
    properties:
      - name: current_view
        type: string
        required: true

  - name: Invite Link Shared
    description: "Tracks when a user opens the share sheet from the friend management screen or welcome card. Fires before the share sheet opens."
    source: ios
    properties:
      - name: source
        type: enum
        required: true
        values: ["friend_management", "welcome_card"]
      - name: current_view
        type: string
        required: true

  - name: Prompt Viewed
    description: "Tracks when DRAFT's question sheet becomes visible to the user. Fires on .onAppear of the question sheet."
    source: ios
    properties:
      - name: prompt_id
        type: string
        required: true
      - name: prompt_type
        type: enum
        required: true
        values: ["scheduled"]
      - name: current_view
        type: string
        required: true
      - name: previous_view
        type: string
        required: false

  - name: Prompt Completed
    description: "Tracks when a user submits a response to DRAFT's question. Fires on submit tap, not on typing. Fires separately for the initial question and any follow-up."
    source: ios
    properties:
      - name: prompt_id
        type: string
        required: true
      - name: is_followup
        type: boolean
        required: true
        description: "True if this response is to DRAFT's follow-up question"
      - name: response_length_chars
        type: integer
        required: true
        description: "Character count of the text response"
      - name: current_view
        type: string
        required: true

  - name: Prompt Dismissed
    description: "Tracks when a user closes DRAFT's question sheet without answering. Fires when the user swipes down or taps outside the sheet without submitting."
    source: ios
    properties:
      - name: prompt_id
        type: string
        required: true
      - name: current_view
        type: string
        required: true

  - name: Card Viewed
    description: "Tracks when a card becomes visible in the user's feed. Fires when the card is at least 50% visible for at least 1 continuous second. Fires once per card per session."
    source: ios
    properties:
      - name: card_id
        type: string
        required: true
      - name: card_subject_is_self
        type: boolean
        required: true
        description: "True if the card is about the user who is viewing it"
      - name: current_view
        type: string
        required: true

  - name: Card Liked
    description: "Tracks when a user taps the like button on a card. Like count is visible to all users; who liked is not shown in MVP."
    source: ios
    properties:
      - name: card_id
        type: string
        required: true
      - name: card_subject_is_self
        type: boolean
        required: true
      - name: current_view
        type: string
        required: true

  - name: Card Shared
    description: "Tracks when a user taps the Share button on a card. Fires before the iOS native share sheet opens."
    source: ios
    properties:
      - name: card_id
        type: string
        required: true
      - name: card_subject_is_self
        type: boolean
        required: true
      - name: current_view
        type: string
        required: true

  - name: Card Moderated
    description: "Tracks when a user takes a moderation action on a card about themselves. Fires on confirmation of the action — not on menu open and not on Cancel."
    source: ios
    properties:
      - name: card_id
        type: string
        required: true
      - name: action
        type: enum
        required: true
        values: ["correction_submitted", "card_removed"]
      - name: current_view
        type: string
        required: true

  - name: Card Reported
    description: "Tracks when a user submits a report on a card. Fires on report submission."
    source: ios
    properties:
      - name: card_id
        type: string
        required: true
      - name: current_view
        type: string
        required: true

  - name: Feed Bubble Tapped
    description: "Tracks when a user taps the new card activity bubble in the feed. Fires on tap, only when the bubble is visible."
    source: ios
    properties:
      - name: current_view
        type: string
        required: true
```

---

## 7. Open questions

**Must be resolved before build**

- `[CLARIFY]` Final app name: "DRAFT" is a codename. Brainstorm needed before public launch. — owner: Aymeric
- `[CLARIFY]` Theme library: the full list of 10–30 themes DRAFT uses to generate daily questions (e.g. weekend plans, current music, upcoming events). Must be written before build. — owner: Aymeric
- `[CLARIFY]` Theme selection logic: random, or contextually chosen based on recency and user profile? — owner: Aymeric + engineering
- `[CLARIFY]` Question generation timing: generated synchronously at send time, or pre-generated a few hours in advance? To validate with engineering. — owner: engineering
- `[CLARIFY]` Push notification copy strategy: first notification should feel like a personal message from DRAFT (not a direct question), referencing past context. Exact copy and format to be defined. — owner: Aymeric
- `[CLARIFY]` Card data structure: headline + body text? Multiple card types? Author display format ("DRAFT" vs "DRAFT on [Name]")? Must be defined before back-end build. — owner: Aymeric + engineering
- `[CLARIFY]` Tone of voice: DRAFT's copy register, level of humour, and how she writes about people. Must be defined before front-end build. — owner: Aymeric
- `[CLARIFY]` Card moderation UI: does the user edit the full card as free text, or is there a more structured correction flow? — owner: Aymeric
- `[CLARIFY]` Report flow: categories and handling process. Required by Apple App Store guidelines for UGC apps. — owner: Aymeric + engineering
- `[CLARIFY]` Card share format: MVP is an image via iOS share sheet. Explore whether a link with automatic card preview is technically feasible. — owner: engineering

**Engineering validation required**

- `[CLARIFY]` Account persistence: no auth in MVP means if user deletes and reinstalls or switches devices, they lose their account. What is the acceptable strategy for MVP? (Accept the loss, or store a device token server-side?) — owner: engineering
- `[CLARIFY]` Deferred deep linking: technical complexity to validate with Florian and Romain before committing to MVP. — owner: engineering
- `[CLARIFY]` Friend discovery MVP implementation: contacts scan + username search + share link confirmed in principle, but simplest MVP approach needs an engineering brainstorm. — owner: Aymeric + engineering

**Risks**

- `[RISK]` Card quality is the core bet. If DRAFT's output feels generic or inaccurate, the input loop breaks. Mitigation: run manual prompt tests with real users before automating card generation.
- `[RISK]` Cold start: a user with zero followers sees very little. For the test phase, cohorts must include 5–6 people who know each other and onboard together. This is a hard requirement for the first test, not a nice-to-have.

**Assumptions**

- `[ASSUMPTION]` Text-only conversation with no integrations generates cards worth reading. Validation: test with real users before the full build.
- `[ASSUMPTION]` Native card sharing via iOS share sheet (image format) is sufficient for MVP virality. Validation: track Card Shared rate and qualitative feedback from the first cohort.

---

*This document is the source of truth for product intent. Any implementation decision that diverges from this spec must be reflected here before the feature is marked done.*