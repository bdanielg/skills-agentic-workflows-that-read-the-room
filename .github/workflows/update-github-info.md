---
name: update-github-info
description: Reads GitHub Blog updates and Mona's notes, then proposes refreshed content for the GitHub Info site via a pull request.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
engine: copilot
tools:
  edit:
  web-fetch:
network:
  allowed:
    - defaults
    - github.com
    - github.blog
    - awesome-copilot.github.com
safe-outputs:
  create-pull-request:
    title-prefix: "[update-github-info] "
    labels: [automation, content-update]
    draft: true
---

# Update Mona's GitHub Info website

Read `notes/mona-notes.md` before making changes.

Use these sources:
- `notes/mona-notes.md`
- GitHub Blog: https://github.blog/latest/
- GitHub Changelog: https://github.blog/changelog/
- Awesome Copilot workflows: https://awesome-copilot.github.com/workflows/

This workflow has a `web-fetch` tool (compiled as the native `web_fetch`
capability) enabled for every run. Call it directly for each URL above
instead of a shell command. Shell commands (`curl`, `wget`, etc.) do not have
network access in this sandbox and will be denied.

IMPORTANT — tool-availability fallback: on rare runs the `web-fetch`/`web_fetch`
tool may not appear in your available tools/functions list. If that happens,
do NOT call `missing_tool` and do NOT stop. Calling `missing_tool` aborts the
run before any pull request is opened, which blocks this workflow's purpose.
Instead, always complete the task end-to-end:
- If `web-fetch`/`web_fetch` is unavailable or a fetch call fails (even after
  one retry), continue without live blog/changelog content.
- Still update `site/content/github-info.md` using at least
  `notes/mona-notes.md`, and list the GitHub Blog, GitHub Changelog, and
  Awesome Copilot workflows URLs above as "Sources to check" so Mona knows
  which pages to review manually this run.
- Still open the pull request. A pull request that updates
  `site/content/github-info.md` and references these source URLs must be
  produced on every run, regardless of which sources were actually fetched.

Update `site/content/github-info.md` with concise,
practical updates for readers and include source context when content comes
from the GitHub Blog or GitHub Changelog.

Open a pull request for Mona to review. 
Use a pull request title that mentions Mona or GitHub Info. 
Do not write directly to `main`;
rely on `safe-outputs` with `create-pull-request`.

## Instructions

1. Read [notes/mona-notes.md](../../notes/mona-notes.md) to understand Mona's editorial preferences and tone.
2. Check whether the `web-fetch` tool is available. If it is, use it (not a shell command) to fetch `https://github.blog/latest/` and see the latest GitHub Blog posts. If it is not available, skip to step 4 and note the URL as a source to check manually.
3. If `web-fetch` is available, use it (not a shell command) to fetch `https://github.blog/changelog/` and see the latest GitHub Changelog entries. If it is not available, note the URL as a source to check manually.
4. Read the current content of `site/content/github-info.md`.
5. Update `site/content/github-info.md` to reflect notable, relevant stories from the blog and changelog, following Mona's preferences (short, practical summaries; mention the source whenever a change comes from the GitHub Blog or GitHub Changelog). If sources could not be fetched, still make a small, honest update (for example, referencing `notes/mona-notes.md` and listing the unfetched source URLs for manual review).
6. Open a pull request with the updated content so Mona can review the changes before they go live. Do not push directly to the default branch. Always complete this step, even when some sources were unavailable — never end the run with only a `missing_tool` report.
