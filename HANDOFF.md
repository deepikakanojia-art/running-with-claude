# Handoff — The Marked Chart

Last updated: 2026-08-30
Live: https://deepikakanojia-art.github.io/running-with-claude/
Files: `index.html`, `twin-voyages.html`

This file is updated after each completed task. It records what changed, the
actual verification performed, and anything left unresolved or undecided.
Full change history is in `git log`.

---

## Most recent task: fix Apps Script CORS/auth failure (commit `55fdf84`)

**What changed:** `API` in both `index.html` and `twin-voyages.html` pointed
to a domain-restricted deployment URL
(`.../a/macros/betterup.co/s/.../exec`). Unauthenticated requests — a fresh
browser, incognito, no active `@betterup.co` Google session — were redirected
toward a Google sign-in page instead of reaching `doGet`/`doPost`. In a
browser this surfaced as a CORS error (`No 'Access-Control-Allow-Origin'
header`); via `curl` it surfaced as a 401/302. Fix: switched both files to
the public, non-restricted URL (`.../macros/s/.../exec`) after the
deployment's access setting was changed to "Anyone."

**Test output (real, this session):**
- Ran the same request in a fresh browser context (no prior Google session)
  against the new URL: zero console errors, zero page errors.
- App state after a real save + leaderboard fetch:
  ```
  railTag: "Live"
  railLiveSample: [{"name":"deepika","xp":1850}, ...]
  ```
  Confirms round-trip: save → sheet → leaderboard read-back, with real
  pre-existing data.
- Confirmed via `curl` that the endpoint now returns `access-control-allow-origin: *`
  and that following its redirect returns valid JSON.
- Confirmed the fix is live at the actual GitHub Pages URL (not just locally)
  after CDN cache expired.

## Unresolved / needs a decision

- **"Enroll in Your Next Course" card** (final leg) still links to `#` —
  no real Coursera URL provided yet for "Augment & Automate Your Business."
- **"Ask your peers" Slack link** is still a `#` placeholder — needs the real
  channel URL.
- **Quiz question presentation** — a request to restructure Mission Check
  questions into one-at-a-time popups (vs. current stacked-in-panel layout)
  was explicitly deferred pre-launch, at the requester's instruction, to
  avoid a structural UI change the day before go-live. Not started.
- **`twin-voyages.html`** has not had the same 5-width overlap/hit-test
  suite run against it as `index.html` — only a lighter manual spot-check
  (toasts vs. the back-link, no overlap found).
- Data capture is wired and confirmed reaching the sheet, but the reviewer
  (or Deepika) should still open the sheet directly and confirm the `RWC —`
  tab contents/column values look correct end-to-end — this note is the
  network-layer + app-state proof, not a screenshot of the sheet itself.

## Known constraints for whoever picks this up

- This assistant cannot push to GitHub directly (no stored credentials in
  the working environment) — every push in this project's history has been
  done manually via GitHub Desktop or terminal by Deepika after a local
  commit. Commits and their hashes are always reported so this is auditable.
- This assistant cannot open the linked Google Sheet — verification of sheet
  contents always requires a human with access.
