# DRAFT — Design Brief

## What is DRAFT?

DRAFT is a social app for close friend groups. An AI journalist talks to each person in the group, asks them questions about their life, and publishes AI-written cards in a shared feed — so everyone always knows what's going on with their friends, without anyone having to post.

Two things happen in the app:

1. **Consumption** — you scroll a feed of cards written about your friends (and yourself). Third-person, journalistic tone. Cards are short, punchy, and feel like headlines from a newspaper about your group.
2. **Creation** — DRAFT asks you a question (twice a day, at 9:47 and 18:12). You reply in a chat-like interface. DRAFT decides whether to turn your answer into a card. You never know what she'll write or when — the surprise is part of the product.

The key insight: people stop sharing because creating content is high-friction and high-pressure. If an AI writes about you in third person, you can share things you'd never post yourself. It removes the social pressure entirely.

---

## The product in one sentence

Every day, DRAFT writes a card about each person in your group — you just answer her questions.

---

## Target user

Urban adults 18–30 with an active close friend group (5–15 people). They use WhatsApp constantly but haven't posted on Instagram in months.

---

## Screens to design

Priority order for the 3-day sprint:

### 1. Design exploration + visual direction (Monday morning)
Before diving into screens, we need a visual direction. The reference we've been working with is a **newspaper / magazine aesthetic** — big typography, strong headlines, clean layout. Cards should feel like articles that happen to be about your friends.

Think: editorial, not social media. The feed is a daily edition. Each card is a story.

A few things to explore:
- How does the card format feel? (headline, body, subject attribution at the bottom — profile photo + first name, small, not dominant)
- Typography-driven layouts — bold headlines are the hero
- Cards should be simple and stylized enough to be shareable outside the app (Instagram, WhatsApp, etc.)
- The chat interface (DRAFT asking questions) lives **over** the feed, not as a separate tab. Think bottom sheet / expandable panel — secondary to the feed, but accessible at any time

We have some Pinterest refs and early Figma explorations to share — we'll send those along with this brief.

### 2. Home feed + DRAFT question sheet (Monday → Wednesday)
The core of the app. Two things coexist on this screen:

**The feed:**
- Chronological list of cards, newest first
- Relative timestamps ("1 hour ago", "yesterday")
- Pull-to-refresh
- Like button (count visible) + Share button (fixed label, opens iOS share sheet)
- 3-dots menu per card (remove / report)
- Floating bubble when new cards are published while you're in the app

**The DRAFT question sheet (lives on top of the feed):**
- A bottom sheet that opens when DRAFT has a question for you
- Shows DRAFT's question and DRAFT's icon
- Text input for the user to reply
- Three states to design:
  - **Active** — DRAFT is waiting for your response
  - **Processing** — you just replied, DRAFT is doing her thing ("DRAFT is writing…")
  - **Idle** — nothing to do right now, waiting for the next prompt
- Sheet is dismissible, but answering is the main CTA

### 3. Onboarding (Wednesday)
Minimal — we want to get people into the app as fast as possible.

Screens in order:
1. **Splash** — logo + app name, auto-advances
2. **Name** — first name only
3. **Age**
4. **Gender**
5. **Profile photo** — we need to ask for this (important for card attribution)
6. **Push permission** — explain why before triggering the native iOS prompt
7. **Home feed** — land here with welcome card already visible + DRAFT's first question open immediately as a bottom sheet

No sign-up, no account creation, no friend discovery during onboarding. Friends are added later via QR code or invite link.

### 4. If time: Friends screen + Settings
Lower priority — can be picked up the following week if needed.

**Friends screen:**
- Two sections: pending requests (accept / reject) + current friends list (remove)
- Persistent CTA at the top to share invite link or show QR code

**Settings:**
- Profile: Name (editable), Profile photo (editable)
- Access: Notifications
- Community: Help, Submit Feature Request, Give a Review
- Legal: Privacy Policy, Terms of Service
- Danger Zone: Delete Account
- Footer: app version, build number, User ID
- Reuse the Amon settings component from Frank / Orai

---

## What's NOT in MVP

No need to design for these — they're explicitly out of scope:

- Comments or in-app messaging (users go back to WhatsApp/Snap to follow up)
- Sub-groups or scoping
- Card detail screen
- Quick-reply chips on prompts
- Data integrations (Spotify, Photos, etc.)
- Weekly digest / newspaper format (the metaphor is the inspiration, not a literal format)
- Public profiles or discovery
- Android, web, non-US

---

## Card format

Cards are the heart of the product. A few things to keep in mind:

- Written in third person ("Margaux had a rough week but is already planning her weekend")
- Subject is attributed at the bottom: small profile photo + first name. Not the headline — the content is the hero, not the person's identity
- Text-only in MVP (no photos, music, location)
- Should feel shareable — simple, typographically clean, strong enough to screenshot and send to a friend
- Cards to design: standard card, welcome card ("[Name] just joined DRAFT!" with QR code + share CTA)

---

## References

- Frank (our health coach app) — for the conversational component and bottom sheet patterns
- Newspaper / magazine editorial layouts — for card typography and feed structure
- Pinterest board: [ask Margaux for the link]
- Figma explorations: [ask Margaux for the link]

---

## Timeline

| Day | Focus |
|-----|-------|
| Monday | Visual direction exploration, logo directions, card format |
| Tuesday | Home feed + DRAFT question sheet |
| Wednesday | Onboarding, iteration on feed/card |
| Following week | Friends screen, settings, iteration |

---

## Full product spec

The complete product spec (flows, business rules, analytics) is in `spec.md` in the same folder as this brief. You don't need to read it end to end — this brief covers everything you need to start. The spec is there if you want to go deeper on any specific flow or edge case.
