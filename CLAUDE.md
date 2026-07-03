# Oboeru — 覚える

A single-page Japanese study app: spaced-repetition review of vocabulary and phrases, with libraries for managing your own words and phrases.

## Project overview

- **`oboeru.html`** — the entire app (HTML + CSS + JS in one file). No build step, no dependencies to install.
- **`vocabulary.json`** — standalone vocabulary data (the app also embeds its starter data inline in `oboeru.html`, inside `<script type="application/json">` blocks: `#vocab-data` and `#phrase-data`).
- **`index.html`** — a tiny redirect to `oboeru.html` so the site root loads the app.

### How it works (key spots in `oboeru.html`)

- **Study modes** (`state.mode`): `vocab` (English → Japanese), `reading` (Japanese → English), `phrase` (phrase recall). Switched via `switchMode()`; rendered in `renderStudyCardArea()`.
- **Spaced repetition**: `grade()` implements an SM-2-style scheduler (`ease`, `interval`, `status` of New / Learning / Review). Queue built by `buildQueue()`; session size is `SESSION_GOAL`.
- **Libraries**: `renderLibraryView()` (words) and `renderPhrasesView()` (phrases) — add / edit / delete / search. `analyzePhrase()` segments a phrase and flags vocabulary not yet in your library.
- **Audio**: `speak()` uses the browser Web Speech API with a `ja-JP` voice.
- **Persistence**: all state is saved through a host-provided `window.storage` API (`loadProgress`/`saveProgress`, `loadCustomVocab`, `loadPhrases`, …). On a plain browser / GitHub Pages that API is absent, so the app runs but does not persist across reloads — see issue for the `localStorage` fallback.

## Working agreement (please follow for every change)

The developer on this project is new to Git, so all Git/GitHub mechanics are handled for them. Every piece of work follows the same loop:

1. **Issue** — make sure a GitHub issue exists for the work (create one if needed).
2. **Branch** — do the work on a feature branch, never on `main`.
3. **Commit** — commit the work to that branch.
4. **Pull request** — open a PR that references the issue (e.g. `Closes #NN`) and includes a browser preview link.
5. **Review** — the developer inspects the result in the browser.
6. **Merge** — merge **only after the developer confirms the work is complete**.

`main` is only ever updated by merging a PR. Direct commits/pushes to `main` are not allowed (enforced by a hook — see below).

## Viewing the app in the browser

- **Live app (`main`):** https://photogj9.github.io/Oboeru/ (GitHub Pages)
- **Current `main`, direct file:** https://raw.githack.com/photogj9/Oboeru/main/oboeru.html
- **Per-PR / per-branch preview:** `https://raw.githack.com/photogj9/Oboeru/<branch>/oboeru.html`

Every PR should include the matching branch preview link so the work can be inspected before merging.

## Workflow enforcement (hook)

A `PreToolUse` hook blocks any `git commit` or `git push` that would write directly to `main`/`master`, and blocks explicit pushes to `main` from any branch. It lives in local (untracked) config so it stays active regardless of the checked-out branch:

- `.claude/settings.json` — registers the hook on the Bash/PowerShell tools.
- `.claude/hooks/enforce-branch.sh` — the guard script.

These are intentionally kept local (not committed) so a branch switch can't disable the guard. To share enforcement with other contributors later, commit them to the repo.

## Repo

https://github.com/photogj9/Oboeru
