---
name: update-github-info
description: Keep the GitHub Info website current with practical, sourced updates from the GitHub Blog and Changelog.
model: copilot/auto
on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read
  pull-requests: read

tools:
  edit: true
  web-fetch:

network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com

safe-outputs:
  create-pull-request:
    title-prefix: "[GitHub Info] "
    draft: true
---

# Update GitHub Info

Read `notes/mona-notes.md` and `site/content/github-info.md` first. Use Mona's editorial guidance when deciding what is useful for developers.

Fetch https://github.blog/latest/, https://github.blog/changelog/, and https://awesome-copilot.github.com/workflows/. Review recent blog posts, changelog entries, and Awesome Copilot workflows for verified, practical information that helps developers learn GitHub faster. Follow relevant links on github.com when needed to confirm details.

Update only `site/content/github-info.md`. Keep the content concise, preserve its existing structure and themes, and include the source URL whenever an update is based on the GitHub Blog, Changelog, or Awesome Copilot workflows. Do not invent details or repeat items that are already covered. If there is no meaningful new information to add, leave the file unchanged and do not create an empty pull request.

When you make a meaningful update, use the configured create-pull-request safe output to open one draft pull request for Mona to review. Do not push changes directly to the default branch or modify any other files.