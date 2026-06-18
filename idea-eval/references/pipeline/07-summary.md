# Stage 7: Summary & Persist

## Executive Summary Format

Write a concise executive summary covering:

1. **Verdict paragraph**: 2-3 sentences on whether this is worth building and why
2. **Top 3 strengths**: What makes this idea compelling
3. **Top 3 concerns**: What could sink it
4. **Recommended next steps**: 3-5 specific, actionable steps (not vague advice)
5. **Team lead recommendation**: Who on the team should lead this and why (reference team bios)

## Persistence

After generating the summary, save everything to the configured store (see Setup in SKILL.md — local files by default, optionally a GitHub repo):

1. Build `idea.json` with all data from stages 1-7
2. Create `notes.json`: `{"notes": []}`
3. Save history snapshot: `history/YYYY-MM-DD-initial.json`
4. Write all files under `<store>/ideas/<slug>/`
5. Read current `RANKINGS.md`, insert new idea in score order, update timestamp
6. Save updated `RANKINGS.md`

For a local store, just use the Write tool. For a GitHub store, save locally first as a durable backup, then push with whatever GitHub tooling the user has configured — if the push fails, the files are safe locally.

Tell the user the results are saved and show the updated rankings.
