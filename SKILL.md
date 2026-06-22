---
name: deferred-authentication
description: Use when an agent encounters login, OAuth, API key, cookie, session, 401/403, permission, account linking, workspace claiming, publishing, deployment, or third-party integration blockers while trying to complete a task.
---

# Deferred Authentication

## Overview

Authentication is not the goal; it is a capability. Continue useful work first, defer identity-dependent actions until they are unavoidable, and produce the best available artifact when authentication cannot complete.

## Core Rule

Ask "What capability is needed to complete the user's goal?" before asking "How do I log in?"

Priority:

1. Use existing access.
2. Use Temporary Workspace, Sandbox, Preview Environment, Guest Mode, or local/offline mode.
3. Generate an offline artifact the user can inspect or apply later.
4. Pause for human authentication only when a real privileged resource is required.
5. Treat task failure as the last resort.

## Flow

When blocked by authentication:

```text
Goal -> needed capability -> low-privilege path?
  yes: continue
  no: temporary/guest/preview/sandbox path?
    yes: use it
    no: offline artifact useful?
      yes: produce it
      no: retry auth once if there is a clear fix, then pause for the user
```

Do not pause at the first login screen if the core task can still move forward.

## Retry

Use `max_retry = 1`. Retry only when there is a specific correction, such as a token typo, just-completed login, or newly granted permission. If the initial attempt and one retry both fail, stop authentication attempts and switch to fallbacks.

Never cycle through Cookie, OAuth, API key, browser session, refresh token, different accounts, new login URLs, or repeated "maybe this works" attempts.

## Fallbacks

Search for a lower-privilege path before requesting login. Examples: preview deploy instead of production deploy, patch instead of GitHub publish, Markdown instead of Notion write, JSON payload instead of Jira create, draft/preview instead of social post, local archive instead of cloud upload.

For a larger fallback table, use [fallback-options.md](references/fallback-options.md).

## Pause Protocol

Pause, do not terminate, when privileged access is truly required. Before pausing, finish non-authenticated preparation, preserve generated work, state what is complete, name the required user action, and give the exact resume step.

Use this shape:

```markdown
This task now requires access to a real user resource.

Completed:
- [specific completed work]

Waiting for you to:
- [log in / approve OAuth / claim workspace / provide token]

After that, reply `Continue` and I will resume from:
- [next exact step]
```

Do not ask for secrets in chat when interactive login, connector authorization, or Temporary Workspace claim is available.

## Red Flags

Stop and re-evaluate when you think: "I should log in first", "maybe another auth method will work", "authentication failed so the task failed", or "nothing can be done until the account is connected."
