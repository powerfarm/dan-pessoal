# User-Facing Platform Mandates

**User:** dcamarilho (dcamarilho@gmail.com)  
**Created:** 7/5/2025 16:14  
**Updated:** 7/16/2025 6:21  
**Exported:** 7/16/2025 6:21  
**Link:** [https://chatgpt.com/c/68694162-2b54-8008-be62-294c89538d6d](https://chatgpt.com/c/68694162-2b54-8008-be62-294c89538d6d)  

## Prompt:
Understood. You are asking for the final layer of professional consideration for Part III: The User-Facing Platforms.
This is about defining the implicit rules and strategic decisions that will make the user experience feel intentional, safe, and "upper-class," as you requested. This is the information needed to guide the actual frontend development and community management.
Part III Addendum: The User-Facing Platforms - Unspoken Mandates
1. On voulezvous.tv ("Party Mode"): The Principle of Implicit Consent
The Problem: A feature that shows live camera feeds of everyone online can be startling or invasive if not handled correctly.
The Mandate: The user must give clear, one-time, implicit consent. The experience is predicated on this buy-in.
Implementation:
During Sign-Up: The final step of the registration process is not a checkbox. It is a full-screen, institutional statement:
"Welcome to VoulezVous.tv. This is a private, local community built on shared presence. By proceeding, you understand and consent that when you are in 'Party Mode,' your camera will be active and a silent, live feed will be shared with other logged-in members. This is the core of the experience. To continue, please grant camera permissions."
The only buttons are [ Grant Permission & Enter ] and [ Decline & Exit ]. This is a hard gate. It ensures everyone inside understands and agrees to the rules of the space. There is no ambiguity.
2. On the Presence Column: The "Atmosphere" over "Information"
The Problem: A grid of faces can feel like a surveillance panel or a corporate video call. This is the wrong aesthetic.
The Mandate: The Presence Column must feel like an ambient, atmospheric representation of the party, not a list of participants.
Implementation:
No Names, No Text: The video rectangles must never have usernames or any text overlaid on them. They are pure presence.
Soft & Ephemeral: The feeds should have a subtle visual treatment—perhaps a soft vignette, a gentle visual noise filter, or a slow, almost imperceptible "breathing" animation (a slight scale-up and down).
Dynamic Sorting: The column is not static. It should subtly re-order itself based on user activity. A user who just went live on their personal broadcast might gently rise to the top. A user who has been idle for a long time might slowly sink. This makes the column feel alive.
3. On Personal Broadcasts (/username): The "Performer/Audience" Contract
The Problem: A user needs to understand the shift in dynamic when they go from being a participant in a party to a performer for an audience.
The Mandate: The UI must create a clear, intentional separation between Party Mode and Personal Broadcast Mode.
Implementation:
The "Go Live" button in Party Mode should trigger a modal that acts as a final confirmation:
"You are about to start a public broadcast at voulezvous.tv/your_username. Your stream will be open to anyone with the link. Viewers will only be able to interact via text chat. Are you ready to perform?"
This confirms the user's intent and clarifies the rules of engagement for their broadcast. It establishes the "1-to-Many with text" topology contract.
4. On Privacy Controls: The Power of Invisibility
The Problem: Privacy can't feel like a complex settings page. It needs to be an immediate, powerful tool.
The Mandate: The "Invisible" mode is the most important privacy feature.
Implementation:
In addition to the main settings, there should be a single, easily accessible "eye" icon (👁️) always visible in the UI when logged in.
Clicking it instantly toggles "Invisible" mode. The user's own video feed in the Presence Column immediately fades out for everyone else. When they are invisible, the icon shows a slash through it (👁️‍🗨️).
This gives the user instant, one-click control over their own presence, which builds trust and encourages participation, as they know they can retreat at any moment.
5. On the "Local Brand": Fostering the Community
The Problem: The system needs to feel like it belongs to a specific, local community, not a generic global platform.
The Mandate: The platform's features should reinforce the sense of a shared, trusted space.
Implementation:
Invitation-Only Growth: New user registration should ideally be disabled by default. New users can only join if they receive an invitation link from an existing, trusted member. This keeps the community "local" and curated.
No Public User Directory: There is no way to search for a user by name. The only way to find someone is to see them live in the Party Mode's Presence Column, reinforcing the importance of shared, real-time presence. Discovery is serendipitous, not surgical.
This extra information ensures that the experience of using the platforms is as professional, intentional, and unique as the underlying technology that powers them.

## Response:
Pensou por 18 segundos

O usuário forneceu uma descrição detalhada sobre as regras das plataformas voltadas para o público, especificamente sobre a "Party Mode" em voulezvous.tv e o princípio do consentimento implícito. Parece que estão continuando os detalhes para nossa consideração final. Eu preciso reconhecer, resumir, e talvez fornecer um feedback ou implementar a última camada profissional para a Parte III, conforme solicitado. Como não temos as partes anteriores, nosso foco será ajudar de forma concisa e precisa no que é solicitado!

O usuário forneceu detalhes sobre os mandatos para a plataforma voltada ao público, com foco em uma experiência profissional e intencional. Embora não tenha feito uma pergunta direta, parece que o próximo passo seria revisar e refinar esse conteúdo, ou talvez integrar essas informações em um documento final para os desenvolvedores. Eu posso revisar os mandatos, criar um guia de implementação ou elaborar uma especificação clara, atendendo à solicitação sem perder a profissionalidade e o foco na experiência do usuário.

Pronto

**Voulez Vous Part Iii User Facing Platforms Implicit Mandates**

# VoulezVous.tv – User‑Facing Platforms
## Part III Addendum: Implicit UX Mandates

*This document crystallises the “unspoken rules” that shape the front‑end experience and community governance for VoulezVous.tv ("Party Mode") and the personal broadcast subsystem. It is intended for product, design, engineering and community‑ops leads. Follow it as a living standard: every UI decision should reinforce these principles.*

---

### 1 · Party Mode – Principle of Implicit Consent
| Goal | Ensure every participant explicitly acknowledges that their webcam is live to the community while in Party Mode. |
|------|-------------------------------------------------------------------------------------------------------------|
**UX Contract**  | Full‑screen final step of sign‑up. Replace conventional checkbox with a decisive gate.
Copy snippet  | *“Welcome to VoulezVous.tv. This private space thrives on shared presence. By continuing, you consent that while in Party Mode your camera will be live to other members. This is core to the experience.”*
Action set     | **[Grant Permission & Enter]** (camera request + submit session)  ·  **[Decline & Exit]** (log out + clear session)
Fail‑safe      | If permission denied, show a single action: **[Try Again]**. No degraded “lurker” mode; presence must be reciprocal.
Technical note | Use WebRTC `getUserMedia` permissions with `camera` param only; no microphone by default.
Audit          | Store a timestamped `partyConsentGranted` flag in user metadata (hashed) for compliance tracking (GDPR “proof of consent”).

---

### 2 · Presence Column – Atmosphere > Information
| Goal | Turn the feed grid into ambient "vibe lighting", avoiding corporate or surveillance undertones. |
|------|----------------------------------------------------------------------------------------------------------|
Visual treatment | • No labels, usernames or text overlays  ·  • Apply CSS `filter: contrast(.9) brightness(.95)` and subtle vignette mask.  ·  • "Breathing" animation: 10 s keyframe scale 1 → 1.02 → 1.
Dynamic sorting | Real‑time ranking algorithm (WebSocket stream):
1. **Recent Activity Score** (broadcast start/stop, emoji flares, chat sent)
2. **Idle Decay** (–1 pt per 30 s inactivity; threshold pushes tile downward)
3. Smooth reorder with CSS `transition: transform .6s ease`.
Performance | Tiles capped at 120×80 px @ 8 fps preview; click opens full peer feed.

---

### 3 · Personal Broadcasts (`/username`) – Performer / Audience Contract
| Goal | Make the context switch from “party peer” to “public performer” unmistakable. |
|------|-----------------------------------------------------------------------------------------------------------|
Trigger | Primary **Go Live** button inside Party UI.
Modal copy | *“You’re about to start a public broadcast at voulezvous.tv/your_username. Anyone with the link can view. Interaction is text‑chat only. Ready to perform?”*
Controls | **[Start Broadcast]** / **[Cancel]**. Show viewer count badge once live.
Isolation | While broadcasting, user’s tile in Presence Column shows a "stage" frame and persists at top.
Security | Viewer chat sandboxed; performer cannot see viewers’ cams.

---

### 4 · Privacy Controls – The Power of Invisibility
| Goal | Provide instant withdrawal without cognitive load. |
|------|-------------------------------------------------------------------------------------------|
Universal toggle | Sticky eye‑icon (`position: fixed; bottom 16 px; right 16 px`).
States | **Visible** 👁️  /  **Invisible** 👁️‍🗨️ (slash). Tooltip on hover.
Behaviour | On toggle → broadcast `presence:off`; fade out user tile for peers (`opacity:0` over 300 ms). Camera locally muted to save bandwidth.
Edge cases | Re‑toggle restores feed; on refresh default to last state.

---

### 5 · Local Brand & Community – Invitation‑Only Culture
| Goal | Preserve a curated, trust‑centric membership. |
|------|------------------------------------------------------------------------------------|
Growth model | Registrations disabled by default; invite link (`/invite/uuid`) expires after first use or 7 days.
Discovery | No global search or directory; presence is discovered live. Encourage serendipity.
Moderation | Community leads receive “spectator tokens” allowing invisible audit for ToS breaches.

---

## Implementation Checklist
- [ ] Consent gate integrated into onboarding flow (blocker, cannot continue without grant)
- [ ] Presence Column tile component with styling + breathing animation
- [ ] Real‑time sorting service (Redis pub/sub or Firestore listener)
- [ ] Go Live modal and route protection (`/username`) with separate WebRTC room
- [ ] Eye‑icon visibility toggle wired to backend `presence` flag & WebRTC track enable/disable
- [ ] Invite system with single‑use tokens + expiry cron job
- [ ] Audit logging for consent and visibility toggles (for compliance & support)

## Risk & Mitigation Notes
| Risk | Mitigation |
|------|-----------|
Unintended exposure of non‑consenting users | Hard gate + automatic disconnection if camera permission revoked mid‑session.
Performance spikes with many live tiles | Downscale preview, lazy‑load off‑screen, adaptive FPS.
Legal (GDPR) data subject requests | Store minimal PII; allow user‑initiated data export & deletion.

---

## Open Questions
1. How many simultaneous tiles are expected on bandwidth‑constrained devices? Define fallback.
2. Should mic audio be later integrated into Party Mode (opt‑in group voice)?
3. Community guidelines enforcement workflow – auto‑flag profanity in chat?

*Prepared for: VoulezVous Product & Engineering Teams – 05 Jul 2025*

I’ve distilled the unspoken mandates into a concise specification document for your product, design, and community-ops teams. Let me know anytime if you’d like further refinements or implementation detail.

## Prompt:
VoulezVous.tv – User‑Facing Platforms

Part III Addendum: Implicit UX Mandates

This document crystallises the “unspoken rules” that shape the front‑end experience and community governance for VoulezVous.tv ("Party Mode") and the personal broadcast subsystem. It is intended for product, design, engineering and community‑ops leads. Follow it as a living standard: every UI decision should reinforce these principles.

1 · Party Mode – Principle of Implicit Consent

Goal

Ensure every participant explicitly acknowledges that their webcam is live to the community while in Party Mode.

UX Contract

Full‑screen final step of sign‑up. Replace conventional checkbox with a decisive gate.

Copy snippet

“Welcome to VoulezVous.tv. This private space thrives on shared presence. By continuing, you consent that while in Party Mode your camera will be live to other members. This is core to the experience.”

Action set

[Grant Permission & Enter] (camera request + submit session)  ·  [Decline & Exit] (log out + clear session)

Fail‑safe

If permission denied, show a single action: [Try Again]. No degraded “lurker” mode; presence must be reciprocal.

Technical note

Use WebRTC getUserMedia permissions with camera param only; no microphone by default.

Audit

Store a timestamped partyConsentGranted flag in user metadata (hashed) for compliance tracking (GDPR “proof of consent”).

2 · Presence Column – Atmosphere > Information

Goal

Turn the feed grid into ambient "vibe lighting", avoiding corporate or surveillance undertones.

Visual treatment

• No labels, usernames or text overlays  ·  • Apply CSS filter: contrast(.9) brightness(.95) and subtle vignette mask.  ·  • "Breathing" animation: 10 s keyframe scale 1 → 1.02 → 1.

Dynamic sorting

Real‑time ranking algorithm (WebSocket stream):

Recent Activity Score (broadcast start/stop, emoji flares, chat sent)

Idle Decay (–1 pt per 30 s inactivity; threshold pushes tile downward)

Smooth reorder with CSS transition: transform .6s ease.
Performance | Tiles capped at 120×80 px @ 8 fps preview; click opens full peer feed.

3 · Personal Broadcasts (/username) – Performer / Audience Contract

Goal

Make the context switch from “party peer” to “public performer” unmistakable.

Trigger

Primary Go Live button inside Party UI.

Modal copy

“You’re about to start a public broadcast at voulezvous.tv/your_username. Anyone with the link can view. Interaction is text‑chat only. Ready to perform?”

Controls

[Start Broadcast] / [Cancel]. Show viewer count badge once live.

Isolation

While broadcasting, user’s tile in Presence Column shows a "stage" frame and persists at top.

Security

Viewer chat sandboxed; performer cannot see viewers’ cams.

4 · Privacy Controls – The Power of Invisibility

Goal

Provide instant withdrawal without cognitive load.

Universal toggle

Sticky eye‑icon (position: fixed; bottom 16 px; right 16 px).

States

Visible 👁️  /  Invisible 👁️‍🗨️ (slash). Tooltip on hover.

Behaviour

On toggle → broadcast presence:off; fade out user tile for peers (opacity:0 over 300 ms). Camera locally muted to save bandwidth.

Edge cases

Re‑toggle restores feed; on refresh default to last state.

5 · Local Brand & Community – Invitation‑Only Culture

Goal

Preserve a curated, trust‑centric membership.

Growth model

Registrations disabled by default; invite link (/invite/uuid) expires after first use or 7 days.

Discovery

No global search or directory; presence is discovered live. Encourage serendipity.

Moderation

Community leads receive “spectator tokens” allowing invisible audit for ToS breaches.

Implementation Checklist

Risk & Mitigation Notes

Risk

Mitigation

Unintended exposure of non‑consenting users

Hard gate + automatic disconnection if camera permission revoked mid‑session.

Performance spikes with many live tiles

Downscale preview, lazy‑load off‑screen, adaptive FPS.

Legal (GDPR) data subject requests

Store minimal PII; allow user‑initiated data export & deletion.

Open Questions

How many simultaneous tiles are expected on bandwidth‑constrained devices? Define fallback.

Should mic audio be later integrated into Party Mode (opt‑in group voice)?

Community guidelines enforcement workflow – auto‑flag profanity in chat?

Prepared for: VoulezVous Product & Engineering Teams – 05 Jul 2025

## Response:
To help you further with this document, could you please clarify what you'd like to do next? For example, do you want to:

1.  Add another section or mandate?
2.  Refine or rewrite a specific part?
3.  Begin turning this into a technical specification or development brief?
4.  Translate it into another language?
5.  Prepare a public-facing version or press release?

Let me know how you'd like to proceed.



---
Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)