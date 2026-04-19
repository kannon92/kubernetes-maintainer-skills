---
name: triage-github-notifications
description: Fetch and triage GitHub notifications, highlighting items that require action
user-invocable: true
---

## Name

triage-github-notifications:triage-github-notifications

## Synopsis

Fetch GitHub notifications for a user over a configurable date range and display actionable items.

## Description

Triages GitHub notifications by fetching them via the `gh` CLI, categorizing by reason (review_requested, assign, author, mention, comment, subscribed), and presenting actionable notifications in prioritized tables.

The argument (if any) is: $ARGUMENTS

## Implementation

### Step 1: Ask for GitHub username

Prompt the user for their GitHub username. Do not assume a default.

### Step 2: Ask for date range

Ask the user how many days back they want to look (e.g., 1, 2, 7, 14, 30).

### Step 3: Fetch notifications

Use the `gh` CLI to fetch notifications:

```bash
gh api /notifications --method GET \
  -f since="$(date -u -d '<N> days ago' +%Y-%m-%dT%H:%M:%SZ)" \
  -f all=true \
  --paginate \
  --jq '.[] | {id, reason, unread, updated_at, title: .subject.title, type: .subject.type, repo: .repository.full_name, url: .subject.url}'
```

### Step 4: Categorize notifications

Group each notification by its `reason` field into these priority-ordered categories:

1. **Review Requested** (`review_requested`) — PRs where the user's review is requested. Highest priority.
2. **Assigned** (`assign`) — Issues/PRs assigned to the user.
3. **Author** (`author`) — The user's own PRs/issues that have new activity and may need follow-up.
4. **Mention** (`mention`) — The user was @-mentioned and may need to respond.
5. **Comment** (`comment`) — Threads the user is participating in that have new comments.
6. **Subscribed** (`subscribed`) — FYI only, no action needed.

### Step 5: Convert API URLs to web URLs

Transform subject URLs from API format to web format:
- `https://api.github.com/repos/{owner}/{repo}/pulls/{number}` → `https://github.com/{owner}/{repo}/pull/{number}`
- `https://api.github.com/repos/{owner}/{repo}/issues/{number}` → `https://github.com/{owner}/{repo}/issues/{number}`

### Step 6: Display results

For each category (except subscribed), display a markdown table with columns:
- Repo
- Item (linked PR/issue number)
- Title
- Updated date

For the **Subscribed** category, display a brief bullet list since these are FYI only.

End with a **Summary** section that:
- Counts items per category
- Highlights which reviews are likely quick approvals (dependency bumps, automated cherry-picks) vs. substantive reviews (KEPs, features, bug fixes)
- Notes any PRs authored by the user that may need follow-up

## Return Value

Markdown-formatted triage report with prioritized, categorized notification tables.
