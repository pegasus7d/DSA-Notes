---
name: live-session-notes
description: Compile a single Live-Sessions/*.md note from an AlgoZenith (maang.in) live-session recording, using a screenshot-driven, user-paced capture loop. Use whenever the user wants to turn a watched (or re-watched) live-session video into a vault note, references "taking notes for the live session", or says things like "compile this into a note".
---

# Live-Session Notes Workflow (DSA-Notes vault)

This vault (`DSA-Notes`) keeps one markdown note per AlgoZenith live-session recording under `Live-Sessions/`. Notes are compiled by watching the recording together with the user and capturing screenshots at the moments the user flags — never by autonomously scrubbing through the whole video. Follow this exact loop.

## 1. Find the video

- Load the Chrome tools if not already loaded (`tabs_context_mcp`, `navigate`, `computer`, `get_page_text`, `find`).
- `tabs_context_mcp` first, then navigate to `https://maang.in/live-sessions`.
- Use the on-page search box (or `get_page_text` after typing a search term) to find the session by title. The user will usually tell you the title, sometimes garbled/typo'd — search anyway, it's almost always findable.
- Click "Watch Session" to open the recording. It's an embedded whiteboard/IDE screen-recording player, not a plain `<video>` — screenshots of the tab are the only way to capture content (there's no transcript to fetch).

## 2. Capture loop — user-paced, not autonomous

- **Do not** scrub through the video yourself or take screenshots on a timer/interval. The user watches the recording in real time (often at 1x outside your control) and tells you when to capture — typically just by saying "screenshot", "screenshot this", "screenshot again", or similar shorthand. Treat any such message as "take a screenshot of the current frame right now."
- After every screenshot, immediately describe back what you see in 1-3 sentences: the whiteboard diagram content (arrays, formulas, arrows), or the code visible in an IDE/LeetCode tab, plus the on-screen timestamp if visible (bottom-left of the player, format `MM:SS/H:MM:SS`). This lets the user correct misreads on the spot — they will, and you should update your understanding when they do (e.g. correcting which line runs before which, or explaining the intuition behind a diagram you found ambiguous).
- If a frame is genuinely illegible (overlapping strokes from multiple drawing passes, mid-derivation, cut off), say so plainly rather than guessing at content. Don't invent meaning to fill gaps.
- Keep every captured frame's content in the conversation (you don't need to save it anywhere else yet) — nothing gets written to a file during this phase.
- This can go on for many screenshots across a long recording (past sessions have run 1.5-2 hours). Just keep capturing and describing until the user is done.

## 3. Compile — only when the user says so

Trigger phrases: "compile this", "compile this correct", "compile into a single note", etc. When you see one, write ONE new file at `Live-Sessions/<Session-Title-With-Hyphens>.md` (match the session's title as it appears on the live-sessions page, hyphenated) using this exact structure, matching the existing files in the folder:

```markdown
---
tags: [live-session, <topic keywords, lowercase-hyphenated>]
status: Watched
duration: <H:MM:SS if known, from the player's total time>
source: https://maang.in/live-sessions/<slug>-<id>
---

# <Session Title>

<One short paragraph of session-level framing if the opening whiteboard had an outline/title card.>

## 1. <Problem/Topic Name>

**Problem:** <one-line statement>

**Worked example:** <concrete numbers from the whiteboard>

<Explanation prose — the *why*, not just the *what*. Prefer explaining the insight that lets you drop from a brute force to an optimized approach.>

\`\`\`cpp
<code exactly as captured, cleaned up only for whitespace/formatting — never invent code that wasn't shown>
\`\`\`

**Verified:** <trace the worked example through the code by hand, confirm it matches>

## 2. <Next topic>
...

---

*Note compiled from whiteboard/IDE screenshots taken at intervals while watching the recording. <Flag anything illegible/ambiguous here, honestly, with enough detail that a future pass could find it in the recording (approximate timestamp if known).>*
```

Rules for compiling:
- One file per session, even if multiple sub-problems were covered — this matches every existing file in `Live-Sessions/` (e.g. `Contribution-Technique.md` covers 3 problems in one note, `STL-Foundations-1.md` covers two back-to-back sessions in one note with `# Part 1` / `# Part 2` headers).
- Number sections `## 1.`, `## 2.`, ... in the order covered.
- Every code block should be exactly what was on screen (or the final/corrected version if the instructor fixed a bug live — call out the bug-and-fix explicitly when that happens, it's valuable to keep, e.g. see `Contribution-Technique.md`'s "Live debugging note").
- Include a `**Verified:**` line under every code block wherever a worked example lets you hand-trace the output — this is what makes the note trustworthy later.
- The closing italicized note is mandatory — it's the vault's convention for marking compiled-from-recording provenance and flagging gaps honestly.
- After writing the note, also add a link to it under the `## Live Sessions` section of `README.md` (create that section if it doesn't exist yet — check first), keeping the existing entries' order and format: `- [Title](Live-Sessions/File-Name.md)`.

## 4. Deliver, write to the vault, and commit

- `SendUserFile` the compiled note (and the updated `README.md` if changed) so the user gets a copy in the conversation.
- `device_stage_files` any file you need to re-read fresh from the vault before editing (README.md especially, since it's shared/mutable — re-stage rather than trusting a cached copy from earlier in the session).
- `device_commit_files` to write the finished file(s) onto the user's Mac. Note: `.claude/` itself is blocked from remote writes — edit files under `.claude/` directly via `device_bash` (heredoc) instead.
- Then, via `mcp__remote-devices__device_bash`, `cd` into the vault and `git add` + `git commit` the new note and any README change.

### Known quirk: stale git locks

This vault has an active auto-git process (looks like the Obsidian Git plugin) that periodically locks `.git`. Expect `git commit`/`git add` to sometimes fail with `Unable to create '.../.git/index.lock': File exists` or the same for `HEAD.lock`. Fix: `mv .git/index.lock .git/_stale<n> 2>/dev/null; mv .git/HEAD.lock .git/_stale<n+1> 2>/dev/null` immediately followed by the git command in the same shell call (don't split across two tool calls — the lock can reappear in the gap). `device_bash` cannot `rm` files (no delete permission), so always `mv` locks aside rather than trying to delete them.

### Known limitation: no push from here

`device_bash` runs in a sandboxed workspace with no outbound network access — `git push` will reliably fail with `fatal: unable to access '...': Received HTTP code 403 from proxy after CONNECT`. This is not fixable by retrying. After committing, tell the user their repo is N commits ahead of `origin/main` and give them the one-line command to run themselves on their Mac: `cd ~/DSA-Notes && git push`.
