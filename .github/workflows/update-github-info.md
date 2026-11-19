---
name: update-github-info
description: Update GitHub info documentation from GitHub Blog and Changelog
on:
  workflow_dispatch:
  schedule:
    - cron: "0 9 * * *"
permissions:
  contents: read
  pull-requests: read
engine: copilot-sdk
tools:
  edit: true
network:
  allowed:
    - github
sandbox:
  agent:
    targets:
      copilot: {}
---

# Update GitHub Info

This workflow fetches the latest information from the GitHub Blog and Changelog,
reviews the notes from Mona, and proposes updates to the GitHub info documentation.

## Steps

1. Read Mona's notes for guidance on what makes good updates
2. Fetch latest posts from GitHub Blog
3. Fetch latest items from GitHub Changelog
4. Update the GitHub info documentation with key insights
5. Create a pull request for Mona to review

## Workflow

You are a helpful assistant that keeps GitHub documentation up to date.

**Task**: Review the GitHub Blog and Changelog for new information that would be
useful to add to our GitHub info documentation. Follow Mona's guidelines carefully.

**Instructions**:

1. First, read the guidelines from `notes/mona-notes.md` to understand what makes
   a good update.

2. Use web-fetch to get the latest posts from https://github.blog/latest/

3. Use web-fetch to get the latest items from https://github.blog/changelog/

4. Based on what you've learned from Mona's notes and the new content, determine
   what updates should be made to `site/content/github-info.md`.

5. Update `site/content/github-info.md` with new information that:
   - Helps developers learn GitHub faster
   - Includes practical updates
   - Mentions the source (GitHub Blog or Changelog)
   - Maintains consistency with existing content

6. Use the `create-pull-request` tool with safe-outputs to create a pull request.
   Set the PR title to "docs: update GitHub info from blog and changelog" and
   include a summary of changes. Set the reviewer to Mona.

**Output Specification**:

If updates are needed:
- Create a pull request with:
  - Title: "docs: update GitHub info from blog and changelog"
  - Description: Summary of what was updated and why
  - Reviewer: Mona
  - Files: `site/content/github-info.md`

If no meaningful updates are needed:
- Output: "No significant updates needed at this time"
- Take no action (noop)

tools:
  web-fetch:
    # Required to read GitHub Blog and Changelog
    enabled: true
  edit:
    # Required to update documentation file
    enabled: true
    allowed-paths:
      - "site/content/github-info.md"
  create-pull-request:
    # Required to propose changes for review with safe-outputs
    enabled: true
    safe-outputs: true
    allowed-files:
      - "site/content/github-info.md"
