# Bytezer Profile README — Cyberpunk Redesign

**Date:** 2026-06-22
**Status:** Approved (brainstorming complete)
**Repo:** `bytezer/bytezer` (GitHub profile README)
**Scope:** Visual-only redesign of the existing `README.md`. Spotify card uses a hosted service (no GitHub Action needed).

## 1. Purpose

Convert the existing text-heavy profile README into a cyberpunk-themed, neon-styled profile without changing the substantive content. The reader's first impression of `github.com/bytezer` should feel like a hacker's terminal: dark, glowing, deliberate, and unmistakably *Bytezer*.

The change is **visual** — no new biographical sections, no new projects added, no factual claims altered. The existing sections (intro, tech stack, what I'm working on, goals, projects, socials) are restyled in place, with two structural additions driven by the visual brief:

- A neon hero / typing banner above the intro.
- A live Spotify now-playing card below the stats section.

## 2. Goals & Non-Goals

### Goals
- Establish a distinct cyberpunk / neon-hacker aesthetic consistent across all sections.
- Use the palette hot pink `#FF006E` + electric cyan `#00F5FF` + magenta `#B026FF` on a black background.
- Keep all substantive content identical to the current README.
- Remain GitHub-flavored-markdown compatible (no JS, no inline CSS).
- Degrade gracefully if any external service is unavailable.

### Non-Goals
- Adding new biographical sections (e.g., blog, experience timeline, certifications).
- Adding the contribution snake graph, visitor counter, or a redesigned socials bar (the user explicitly excluded these in scoping).
- Self-hosting `github-readme-stats` or `streak-stats.demolab.com` (out of scope for a profile README pass; can be revisited if rate limits become an issue).
- Renaming the repo, adding a Pages site, or any structural repo changes.

## 3. Target Layout

The new `README.md` reads top-to-bottom as:

1. **Header line** — `> // ACCESSING :: BYTEZER` (terminal-prompt vibe, blockquote for emphasis).
2. **Neon typing banner** — readme-typing-svg showing rotating taglines in monospace, gradient through the palette.
3. **Intro line** — short tagline with cyberpunk-flavored emoji (kept close to current copy: full-stack developer, cybersecurity passion).
4. **Socials row** — the existing shields.io badges for Instagram, LinkedIn, Email (unchanged).
5. **Tech stack** — `skillicons.dev` grid replacing the plain "Tech Stack" bullet list. Same technologies, presented as a neon icon row.
6. **GitHub stats** — two `github-readme-stats` cards (overall stats, top languages) plus one `streak-stats.demolab.com` card — all three themed to the palette.
7. **Spotify now playing** — live card showing the currently playing track (or "not playing" when idle).
8. **What I'm working on** — bullet list, neon `▸` markers (same content as today).
9. **Goals** — bullet list, neon `▸` markers (same content as today).
10. **Projects** — Altoke Shop section with the existing description and small tech badges (NestJS, React, PostgreSQL, Firebase).
11. **Footer** — `// EOF · built with neon and coffee_` terminal-style closing line.

Each section uses a small typographic divider (e.g., `---` followed by a centered `//` glyph) to reinforce the terminal feel without adding visual noise.

## 4. Components

### 4.1 Hero typing banner
- **Service:** `readme-typing-svg` (readme-typing-svg.demolab.com).
- **Format:** monospace font, dark background, multi-color gradient cycling through `#FF006E → #00F5FF → #B026FF`.
- **Lines:** the static intro line plus rotating taglines, e.g.:
  - `Full-stack developer crafting scalable systems`
  - `Cybersecurity enthusiast // ethical hacking`
  - `NestJS · React · PostgreSQL · clean architecture`
  - `Open to collaborate on secure-by-default projects`
- **Size:** centered, ~28–32px height, single-line.

### 4.2 Skill icons grid
- **Service:** `skillicons.dev`.
- **Theme:** dark background, neon palette tint where supported.
- **Icons (initial set):** `nestjs, react, postgres, typescript, javascript, nodejs, docker, git, linux, vscode, firebase, postman`.
- **Layout:** single row, `?perline=12` so it stays one line on desktop and wraps cleanly on mobile.

### 4.3 GitHub stats cards
- **Service:** `github-readme-stats` (github-readme-stats.vercel.app) for the overall stats and top languages; `streak-stats.demolab.com` (DenverCoder1/github-readme-streak-stats) for the streak card — the dedicated streak service has richer per-element color theming.
- **Color overrides** (apply on top of the default light theme so the cards render dark):
  - `title_color=FF006E`
  - `text_color=00F5FF`
  - `icon_color=B026FF`
  - `bg_color=0D0D0D`
  - `border_color=FF006E`
  - `hide_border=false`
- **Cards:**
  1. Overall stats: `https://github-readme-stats.vercel.app/api?username=bytezer&show_icons=true&bg_color=0D0D0D&title_color=FF006E&text_color=00F5FF&icon_color=B026FF&border_color=FF006E&hide_border=false&v=1`
  2. Top languages: `https://github-readme-stats.vercel.app/api/top-langs/?username=bytezer&layout=compact&bg_color=0D0D0D&title_color=FF006E&text_color=00F5FF&icon_color=B026FF&border_color=FF006E&hide_border=false&v=1`
  3. Streak: `https://streak-stats.demolab.com/?user=bytezer&theme=dark&background=0D0D0D&border=FF006E&stroke=0D0D0D&ring=00F5FF&fire=B026FF&currStreakNum=FF006E&sideNums=00F5FF&currStreakLabel=FF006E&sideLabels=B026FF&dates=00F5FF`
- **Layout:** three cards in a row on desktop, wrapped by GitHub's markdown renderer to a stacked layout on narrow widths. No `<picture>` element needed — natural wrap is sufficient and avoids per-card cache-bust duplication.

### 4.4 Spotify now playing
- **Service:** `kittinan/spotify-github-profile` hosted endpoint at `https://spotify-github-profile.kittinanx.com`. This is a hosted service — no GitHub Action, no Vercel, no Firebase, no Spotify developer app setup required.
- **Mechanism:** the user visits https://spotify-github-profile.kittinanx.com/api/login, authorizes Spotify once, and receives a `UID`. The README embeds an `<img>` tag pointing at `https://spotify-github-profile.kittinanx.com/api/view?uid=<UID>&...&theme=default&bar_color=FF006E&background_color=0D0D0D`.
- **Card behavior:**
  - Playing something → shows track, artist, album art, animated equalizer bars in the chosen neon color.
  - Idle → renders a "not playing" state in the same dark theme.
  - Service outage → the image fails to load and the alt-text shows (acceptable degradation).

### 4.5 Socials row
- Keep the existing shields.io badges as-is. They already work on dark backgrounds and don't clash with the neon palette.

### 4.6 Section markers & bullets
- Use `▸` for list items in the "What I'm working on" and "Goals" sections to reinforce the terminal/CLI feel.
- Use a `---` divider followed by a centered `//` glyph between major sections.

## 5. File Changes

| Path | Change | Purpose |
|---|---|---|
| `README.md` | Rewrite | Apply the new layout, theme, and external service embeds. |

No other files are modified. The repo's git history is preserved.

## 6. Spotify Setup Procedure (one-time, user-driven)

The Spotify card uses the hosted `kittinan/spotify-github-profile` service. There are no repository secrets and no workflow to maintain.

1. Visit https://spotify-github-profile.kittinanx.com/api/login in a browser.
2. Sign in with Spotify and click **Agree** to grant `user-read-currently-playing` and `user-read-recently-played`.
3. The page redirects back and displays a `UID` (a short string).
4. The user pastes the `UID` into the README's `<img src="...&uid=PLACEHOLDER_UID&...">` URL, replacing `PLACEHOLDER_UID`.
5. Commit and push. The Spotify card now updates on its own (the hosted service polls Spotify continuously).

If the user declines, the Spotify section is dropped from the README and the `<img>` tag is removed. The remaining five external assets (typing banner, skill icons, three stats cards) still work.

## 7. Error Handling & Failure Modes

| Service | Failure mode | Visible effect | Mitigation |
|---|---|---|---|
| readme-typing-svg | Service down or rate limited | Banner image fails to load; the alt-text `Typing...` shows | Acceptable; the rest of the README still renders. |
| skillicons.dev | Service down | Skill icons disappear | The `<img>` `alt` attribute lists the technologies in text so readers can still identify the stack. |
| github-readme-stats | Public instance rate limited | Cards may show a "rate limited" message | Can revisit with a self-hosted instance later; not in scope. |
| Spotify (hosted) | Service outage or token revoked | Card fails to load; alt-text shows | README still works; the user can re-authorize via the hosted login flow to refresh. |
| Any external service | Permanent outage | README looks plainer, not broken | No core content is locked inside an external image; the underlying text remains in the markdown. |

## 8. Testing & Verification

1. **Local preview.** Use a markdown previewer (e.g., VS Code, Obsidian) to scan the rendered structure. The external SVGs will not preview locally; only the alt-text will show. This is acceptable — local preview is for layout review, not for the dynamic content.
2. **External URL check.** Open each external SVG URL in a browser to confirm it returns a valid image. The URLs to test:
   - The full `https://readme-typing-svg.demolab.com/...` URL with all parameters baked in, exactly as it appears in the final `README.md` (the implementation plan will record the exact URL string).
   - `https://skillicons.dev/icons?i=...` (full icon list).
   - `https://github-readme-stats.vercel.app/api?username=bytezer&show_icons=true&bg_color=0D0D0D&title_color=FF006E&text_color=00F5FF&icon_color=B026FF&border_color=FF006E&hide_border=false`.
   - `https://github-readme-stats.vercel.app/api/top-langs/?username=bytezer&layout=compact&bg_color=0D0D0D&title_color=FF006E&text_color=00F5FF&icon_color=B026FF&border_color=FF006E&hide_border=false`.
   - `https://streak-stats.demolab.com/?user=bytezer&theme=dark&background=0D0D0D&border=FF006E&stroke=0D0D0D&ring=00F5FF&fire=B026FF&currStreakNum=FF006E&sideNums=00F5FF&currStreakLabel=FF006E&sideLabels=B026FF&dates=00F5FF`.
3. **GitHub render.** Commit the README and view it at `github.com/bytezer`. Verify all images load, the palette is consistent, and the layout reads cleanly on desktop and mobile (use browser devtools mobile view).
4. **Spotify UID.** Visit https://spotify-github-profile.kittinanx.com/api/login, authorize, and obtain the `UID`. Replace the `PLACEHOLDER_UID` in the README.
5. **Accessibility check.** Confirm the README still has reasonable `alt` text on images and that the section order makes sense when read as plain text (no information exists only inside an image).

## 9. Out of Scope (Explicit)

- Adding new content sections (e.g., blog, talks, certifications).
- Adding a contribution snake, visitor counter, or redesigned socials bar.
- Self-hosting `github-readme-stats` or `streak-stats.demolab.com`.
- Adding a GitHub Pages site or any non-README artifacts.
- Renaming the repository.
- Restructuring the repository (e.g., moving the README elsewhere, adding a `docs/` site beyond the spec file).

## 10. Success Criteria

The redesign is complete when:

1. The README at `github.com/bytezer` renders all six external assets (one typing banner, one skill-icons row, three stats cards, one Spotify card) with the cyberpunk palette.
2. No substantive content has been added, removed, or contradicted.
3. The README degrades gracefully if any single external service fails.
4. The Spotify card displays the user's currently playing track (or "not playing" when idle) via the hosted `kittinan/spotify-github-profile` service.
5. The new layout reads cleanly on both desktop and mobile widths.
