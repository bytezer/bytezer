# Bytezer Profile README — Cyberpunk Redesign

**Date:** 2026-06-22
**Status:** Approved (brainstorming complete)
**Repo:** `bytezer/bytezer` (GitHub profile README)
**Scope:** Visual-only redesign of the existing `README.md` + one new GitHub Actions workflow for the Spotify card.

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
- Self-hosting `github-readme-stats` (out of scope for a profile README pass; can be revisited if rate limits become an issue).
- Renaming the repo, adding a Pages site, or any structural repo changes.

## 3. Target Layout

The new `README.md` reads top-to-bottom as:

1. **Header line** — `> // ACCESSING :: BYTEZER` (terminal-prompt vibe, blockquote for emphasis).
2. **Neon typing banner** — readme-typing-svg showing rotating taglines in monospace, gradient through the palette.
3. **Intro line** — short tagline with cyberpunk-flavored emoji (kept close to current copy: full-stack developer, cybersecurity passion).
4. **Socials row** — the existing shields.io badges for Instagram, LinkedIn, Email (unchanged).
5. **Tech stack** — `skillicons.dev` grid replacing the plain "Tech Stack" bullet list. Same technologies, presented as a neon icon row.
6. **GitHub stats** — three `github-readme-stats` cards (overall stats, top languages, streak) themed to the palette.
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
- **Service:** `github-readme-stats` (github-readme-stats.vercel.app).
- **Theme:** custom theme `bytezer_cyberpunk` with:
  - `title_color=FF006E`
  - `text_color=00F5FF`
  - `icon_color=B026FF`
  - `bg_color=0D0D0D`
  - `border_color=FF006E`
  - `hide_border=false`
- **Cards:**
  1. Overall stats (`/api?username=bytezer&theme=bytezer_cyberpunk&show_icons=true`).
  2. Top languages (`/api/top-langs/?username=bytezer&theme=bytezer_cyberpunk&layout=compact`).
  3. Streak (`/api/streak?username=bytezer&theme=bytezer_cyberpunk`).
- **Layout:** three cards in a row on desktop; on narrow widths they fall back to a stacked layout via HTML `<picture>` with a `media="(max-width: 768px)"` `<source>` that points to a different layout. A `?v=<timestamp>` query parameter is appended to each URL for cache busting so updated stats render promptly.

### 4.4 Spotify now playing
- **Service:** `kittinan/spotify-github-profile` GitHub Action.
- **Mechanism:** scheduled workflow (every 30 minutes) calls the Spotify Web API, writes the rendered SVG to `metrics/spotify.svg` on the `metrics` branch, and the README embeds that SVG via raw.githubusercontent.com URL.
- **Card behavior:**
  - Playing something → shows track, artist, album art, progress bar.
  - Idle → renders a "not playing" state in the same cyberpunk theme.
  - Workflow error → the last successful SVG remains visible (acceptable degradation).

### 4.5 Socials row
- Keep the existing shields.io badges as-is. They already work on dark backgrounds and don't clash with the neon palette.

### 4.6 Section markers & bullets
- Use `▸` for list items in the "What I'm working on" and "Goals" sections to reinforce the terminal/CLI feel.
- Use a `---` divider followed by a centered `//` glyph between major sections.

## 5. File Changes

| Path | Change | Purpose |
|---|---|---|
| `README.md` | Rewrite | Apply the new layout, theme, and external service embeds. |
| `.github/workflows/spotify.yml` | New | Scheduled action that updates the Spotify SVG. |
| `metrics/spotify.svg` | Generated | Produced by the action; the README references its raw URL. |

No other files are modified. The repo's git history is preserved (the README is a single tracked file today, and it stays single-tracked).

## 6. Spotify Setup Procedure

The Spotify card requires three repository secrets. The procedure is documented here so it can be completed once after the spec is implemented.

1. Open https://developer.spotify.com/dashboard and create an app.
2. Note the **Client ID** and **Client Secret**.
3. In the app settings, add this redirect URI: `http://localhost:5543/callback`.
4. Run a one-time local script (or follow the `kittinan/spotify-github-profile` README) to authorize the app and obtain a **refresh token**. The script opens a browser, you click "Agree", and it prints the token.
5. In the GitHub repo, go to **Settings → Secrets and variables → Actions** and add:
   - `SPOTIFY_CLIENT_ID`
   - `SPOTIFY_CLIENT_SECRET`
   - `SPOTIFY_REFRESH_TOKEN`
6. Manually trigger the workflow once from the Actions tab to confirm the SVG is generated and the card renders on the README.
7. Subsequent runs happen on the schedule (every 30 minutes).

If the user declines to set up Spotify, the workflow and the README reference to `metrics/spotify.svg` are dropped, and the README ships without that section.

## 7. Error Handling & Failure Modes

| Service | Failure mode | Visible effect | Mitigation |
|---|---|---|---|
| readme-typing-svg | Service down or rate limited | Banner image fails to load; the alt-text `Typing...` shows | Acceptable; the rest of the README still renders. |
| skillicons.dev | Service down | Skill icons disappear | README falls back to the plain text list (the spec includes both — text list lives in an HTML comment that is the alt-text/`<img>` `alt` attribute). |
| github-readme-stats | Public instance rate limited | Cards may show a "rate limited" message | Can revisit with a self-hosted instance later; not in scope. |
| Spotify workflow | Action fails or token revoked | Card shows last known state or "not playing" | README still works; the user can re-authorize and re-run the workflow. |
| Any external service | Permanent outage | README looks plainer, not broken | No core content is locked inside an external image; the underlying text remains in the markdown. |

## 8. Testing & Verification

1. **Local preview.** Use a markdown previewer (e.g., VS Code, Obsidian) to scan the rendered structure. The external SVGs will not preview locally; only the alt-text will show. This is acceptable — local preview is for layout review, not for the dynamic content.
2. **External URL check.** Open each external SVG URL in a browser to confirm it returns a valid image. The URLs to test:
   - The full `https://readme-typing-svg.demolab.com/...` URL with all parameters baked in, exactly as it appears in the final `README.md` (the implementation plan will record the exact URL string).
   - `https://skillicons.dev/icons?i=...` (full icon list).
   - `https://github-readme-stats.vercel.app/api?username=bytezer&theme=bytezer_cyberpunk`.
   - `https://github-readme-stats.vercel.app/api/top-langs/?username=bytezer&theme=bytezer_cyberpunk&layout=compact`.
   - `https://github-readme-stats.vercel.app/api/streak?username=bytezer&theme=bytezer_cyberpunk`.
3. **GitHub render.** Commit the README and view it at `github.com/bytezer`. Verify all images load, the palette is consistent, and the layout reads cleanly on desktop and mobile (use browser devtools mobile view).
4. **Spotify action.** After secrets are configured, trigger the action manually from the Actions tab. Confirm `metrics/spotify.svg` is generated on the `metrics` branch and the card renders.
5. **Accessibility check.** Confirm the README still has reasonable `alt` text on images and that the section order makes sense when read as plain text (no information exists only inside an image).

## 9. Out of Scope (Explicit)

- Adding new content sections (e.g., blog, talks, certifications).
- Adding a contribution snake, visitor counter, or redesigned socials bar.
- Self-hosting `github-readme-stats`.
- Adding a GitHub Pages site or any non-README artifacts.
- Renaming the repository.
- Restructuring the repository (e.g., moving the README elsewhere, adding a `docs/` site beyond the spec file).

## 10. Success Criteria

The redesign is complete when:

1. The README at `github.com/bytezer` renders all six external assets (one typing banner, one skill-icons row, three stats cards, one Spotify card) with the cyberpunk palette.
2. No substantive content has been added, removed, or contradicted.
3. The README degrades gracefully if any single external service fails.
4. The Spotify workflow runs on schedule and the card reflects the current track.
5. The new layout reads cleanly on both desktop and mobile widths.
