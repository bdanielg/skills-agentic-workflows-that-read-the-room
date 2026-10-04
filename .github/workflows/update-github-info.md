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

Update `site/content/github-info.md` with concise,
practical updates for readers and include source context when content comes
from the GitHub Blog or GitHub Changelog.

Open a pull request for Mona to review. 
Use a pull request title that mentions Mona or GitHub Info. 
Do not write directly to `main`;
rely on `safe-outputs` with `create-pull-request`.

## Instructions

1. Read [notes/mona-notes.md](../../notes/mona-notes.md) to understand Mona's editorial preferences and tone.
2. Web fetch `https://github.blog/latest/` to see the latest GitHub Blog posts.
3. Web fetch `https://github.blog/changelog/` to see the latest GitHub Changelog entries.
4. Read the current content of `site/content/github-info.md`.
5. Update `site/content/github-info.md` to reflect notable, relevant stories from the blog and changelog, following Mona's preferences (short, practical summaries; mention the source whenever a change comes from the GitHub Blog or GitHub Changelog).
6. Open a pull request with the updated content so Mona can review the changes before they go live. Do not push directly to the default branch.
