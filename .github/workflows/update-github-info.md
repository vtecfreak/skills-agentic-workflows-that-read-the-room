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
---

# Update GitHub Info

Read `notes/mona-notes.md` and `site/content/github-info.md` first. Use Mona's editorial guidance when deciding what is useful for developers.

Call the enabled built-in tool named `web_fetch` (underscore; this is a tool, not a skill) once for each source, passing its full URL in the `url` argument: `https://github.blog/latest/`, `https://github.blog/changelog/`, and `https://awesome-copilot.github.com/workflows/`. Read the returned content directly. Do not invoke `skill(web-fetch)`, use GitHub `get_file_contents`, use shell commands such as `curl` or `wget`, or save fetched pages to files. Use `web_fetch` for relevant links when needed to confirm details.

If any required `web_fetch` call fails, report the failed URL and actual tool error with `report_incomplete`. Do not describe a fetch failure as a filesystem permission issue or as a lack of new information.

Update only `site/content/github-info.md`. Keep the content concise, preserve its existing structure and themes, and include the source URL whenever an update is based on the GitHub Blog, Changelog, or Awesome Copilot workflows. Do not invent details or repeat items that are already covered. If there is no meaningful new information to add, leave the file unchanged and do not create an empty pull request.

When you make a meaningful update, use the configured create-pull-request safe output to open one draft pull request for Mona to review. Include the source URL or URLs used in the pull request description, as well as in the updated website content. Do not push changes directly to the default branch or modify any other files.