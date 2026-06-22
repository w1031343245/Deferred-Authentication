# Deferred Authentication

Deferred Authentication is an agent skill for keeping long-running tasks focused when authentication, permissions, publishing, deployment, upload, or account-bound actions block progress.

The central idea is simple:

> Authentication is not the goal. Authentication is only one capability that may be needed to complete the goal.

When an agent hits a login screen, OAuth failure, API key issue, expired cookie, missing session, `401/403`, broken `git push`, blocked deployment, or failed upload, it can accidentally let the task drift. The original objective slowly becomes "fix authentication" or "prove this one blocked capability works." This skill teaches the agent to protect the original goal instead.

## Why This Matters

Long-running agent loops are especially vulnerable to capability drift. A development task might start as:

```text
Build the feature and prepare it for release.
```

Then one blocked capability changes the center of gravity:

```text
Need to push to GitHub
  -> git push fails
  -> fix token
  -> retry auth
  -> change remote
  -> try another credential path
  -> the task has become "make git push work"
```

Deferred Authentication prevents that drift. If a capability blocks repeatedly, the agent should preserve progress with a resumable artifact: a patch, diff, bundle, local build, preview, JSON payload, draft, archive, or checklist.

## Temporary Change Strategy

Different blockers need different temporary changes. A blocked `git push` should become a patch or bundle; a blocked deployment should become a local build or preview package; a blocked API write should become a payload preview; a blocked CMS write should become Markdown or HTML.

The rule is that temporary changes must be reversible, visibly labeled, and free of privileged side effects. They should keep the original goal moving without pretending that a final authenticated action already happened.

## Core Behavior

The skill guides the agent to:

- finish all useful work that does not require real user identity;
- prefer Temporary Workspace, Sandbox, Preview Environment, Guest Mode, or Offline Mode;
- retry authentication only once when there is a clear fix;
- avoid cycling through cookies, OAuth, API keys, browser sessions, and new login attempts;
- pause for human authentication only when a real privileged resource is unavoidable;
- produce the best available artifact instead of failing the whole task.

## Typical Fallbacks

| Blocked capability | Better continuation |
| --- | --- |
| `git push` fails | create a patch, diff, commit bundle, or local repository state |
| deployment login fails | create a local build, static export, or preview package |
| Notion or CMS write fails | produce Markdown, HTML, or import-ready content |
| Jira or Linear creation fails | produce JSON, CSV, or a Markdown ticket |
| cloud upload fails | produce a local archive, manifest, and upload instructions |
| social posting fails | produce image assets, captions, and a preview draft |

## Package Layout

- `SKILL.md`: main skill instructions.
- `agents/interface.yaml`: skill interface and adapter compatibility metadata.
- `manifest.json`: package metadata.
- `references/fallback-options.md`: fallback and temporary-change pattern library.
