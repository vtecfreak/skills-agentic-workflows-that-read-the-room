---
name: update-github-info
description: Keep the GitHub Info website current with practical, sourced updates from the GitHub Blog and Changelog.
model: gpt-4.1
engine:
  id: copilot
  version: "1.0.90"
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

steps:
  - name: Fetch updater sources
    run: |
      mkdir -p /tmp/gh-aw/agent
      curl --fail --silent --show-error --location --retry 2 --max-time 60 https://github.blog/latest/ --output /tmp/gh-aw/agent/github-blog-latest.html
      curl --fail --silent --show-error --location --retry 2 --max-time 60 https://github.blog/changelog/ --output /tmp/gh-aw/agent/github-changelog.html
      curl --fail --silent --show-error --location --retry 2 --max-time 60 https://awesome-copilot.github.com/workflows/ --output /tmp/gh-aw/agent/awesome-copilot-workflows.html
---

# Update GitHub Info

Read `notes/mona-notes.md` and `site/content/github-info.md` first. Use Mona's editorial guidance when deciding what is useful for developers.

Read the three source files fetched by the workflow's runner-side `steps:` block: `/tmp/gh-aw/agent/github-blog-latest.html`, `/tmp/gh-aw/agent/github-changelog.html`, and `/tmp/gh-aw/agent/awesome-copilot-workflows.html`. Use their contents directly. Do not invoke `skill(web-fetch)`, call `web_fetch`, or use GitHub `get_file_contents` for these external pages from inside the agent sandbox. Review the pages for verified, practical information that helps developers learn GitHub faster.

If a source file is missing or empty, report which source is unavailable with `report_incomplete`. Do not describe a missing source file as a web-fetch or filesystem permission issue.

Update only `site/content/github-info.md`. Keep the content concise, preserve its existing structure and themes, and include the source URL whenever an update is based on the GitHub Blog, Changelog, or Awesome Copilot workflows. Do not invent details or repeat items that are already covered. If there is no meaningful new information to add, leave the file unchanged and do not create an empty pull request.

When you make a meaningful update, use the configured create-pull-request safe output to open one draft pull request for Mona to review. Include the source URL or URLs used in the pull request description, as well as in the updated website content. Do not push changes directly to the default branch or modify any other files.