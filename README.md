# Concept Lab Quizzes

Public GitHub Pages site hosting self-graded companion quizzes for select
[The Concept Lab](https://www.youtube.com/@IGCSEMathPhysicsLab) videos.

Live site: https://rishi000888.github.io/concept-lab-quizzes/

## Pattern

Each quiz is a single self-contained HTML file (hand-drawn Kalam / Permanent
Marker visual style, matching Rishi's Physics/Math Notes projects) that:

1. Embeds the paired YouTube video at the top via the **YouTube IFrame
   Player API**.
2. Listens for `onStateChange`; when `event.data === YT.PlayerState.ENDED`
   fires, it reveals a quiz section that is hidden (`hidden` attribute) until
   then.
3. Shows 5 multiple-choice questions, each independently self-graded in
   plain JS (click an option -> instant correct/incorrect styling +
   feedback text, no backend).
4. Tallies a running score and shows a summary card once every question has
   been answered.

**Known, accepted limitation:** this only gates viewers who open the quiz
page itself. It does nothing for anyone who watches the video directly in
the YouTube app/site and is never told the quiz exists until they finish —
that's fine, the gate is a nudge, not enforcement.

## Adding a new quiz (Part 2, Part 3, or any other video)

1. Copy `circular-motion/part1-quiz.html` into a new folder, e.g.
   `circular-motion/part2-quiz.html` or `friction/quiz.html`.
2. Update: `<title>`, the `<h1>`/`.dek` header text, the 5 quiz questions
   (`.quiz-q` blocks — keep the `data-quiz` / `.opt` / `data-correct`
   markup exactly, that's what the shared JS at the bottom hooks into).
3. Set `YOUTUBE_VIDEO_ID` near the bottom of the `<script>` block to the
   real YouTube video ID (the 11-character code from the video's URL,
   e.g. `dQw4w9WgXcQ` from `youtube.com/watch?v=dQw4w9WgXcQ`).
   - If the video isn't published yet, leave the placeholder value
     (anything containing `REPLACE_WITH`) — the page automatically shows a
     "Video coming soon" card instead of a broken embed, and the quiz stays
     locked. Swap in the real ID later (one line) and push; no other change
     needed.
4. Add a card for it to the root `index.html` list.
5. Commit and push to `main` — GitHub Pages (serving from `main` / `/`
   root) picks it up automatically within a minute or two.

## Hosting setup (already done, for reference)

- Repo: `rishi000888/concept-lab-quizzes` (public — nothing sensitive lives
  here, and public is required for free GitHub Pages).
- Pages source: `main` branch, `/` (root) folder.
- No build step, no dependencies — plain static HTML/CSS/JS, Google Fonts
  CDN for Kalam/Permanent Marker (this is a live web page, not an offline
  share file, so CDN fonts are used here instead of the base64-embedded
  fonts the offline PhysicsNotes `.html` files use).

## Verification done before shipping (2026-09-13)

Tested via a headless-Edge CDP script against a local copy with a real,
embeddable YouTube video ID substituted in:
- Quiz section is `hidden` on load; the "coming soon" card is hidden and
  the real IFrame video embeds when a valid ID is set.
- Calling the exact reveal path the `onStateChange`/`ENDED` handler uses
  correctly un-hides the quiz and scrolls to it.
- All 5 questions graded correctly (both a correct-click and a
  deliberately-wrong-click path checked): right answers highlight green,
  the picked wrong answer highlights red with the correct one revealed
  alongside it, options lock after answering.
- Score summary card appears only after all 5 are answered and shows the
  right tally.
- Placeholder-ID fallback ("Video coming soon" card, quiz stays locked)
  verified with a screenshot.
