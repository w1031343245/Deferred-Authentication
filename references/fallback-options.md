# Fallback Options

Temporary changes are allowed only when they are reversible, avoid privileged side effects, and keep the original goal moving. Always label them as fallback, preview, draft, local, or proposed when they are not the final authenticated action.

| Original goal | Preferred fallback |
| --- | --- |
| Deploy to Cloudflare, Vercel, Netlify, or similar | Temporary workspace, preview deploy, local server, static export |
| Publish to GitHub | Patch file, branch diff, commit bundle, local repository changes |
| Write to Notion, Google Docs, Confluence, or CMS | Markdown, HTML, DOCX, import-ready content |
| Create Jira, Linear, Asana, or Trello issue | JSON export, CSV, Markdown ticket, API payload preview |
| Send email or social post | Draft, preview, subject/body, images, captions, scheduling notes |
| Upload to S3, OSS, Drive, or Dropbox | Local file, archive, manifest, upload instructions |
| Modify production settings | Proposed config diff, risk notes, manual checklist |

## Temporary Change Patterns

| Blocked capability | Allowed temporary change | Recommended artifact | Do not do |
| --- | --- | --- | --- |
| `git push` or GitHub publish | Keep local commit, export patch/diff/bundle | `.patch`, `.bundle`, commit hash, changed-file list | Rotate credentials or rewrite remote history repeatedly |
| Deployment login | Build locally or create preview/static export | `dist/`, preview zip, run instructions | Claim production deploy succeeded |
| CI/CD unavailable | Run local checks and capture results | test report, logs, failure summary | Treat missing CI as passing CI |
| Package publish blocked | Build package tarball and release notes | `.tgz`, checksum, publish checklist | Change registry credentials in a loop |
| API write blocked | Generate request body without calling the API | `payload.json`, `curl` preview, schema notes | Send partial writes to production |
| Notion/CMS write blocked | Produce import-ready content | Markdown, HTML, frontmatter | Invent that content was published |
| Upload blocked | Archive locally with manifest | `.zip`, `manifest.json`, upload checklist | Delete or mutate remote storage |
| Email/social blocked | Produce a draft and preview | subject/body, captions, images, schedule notes | Send from another account without approval |
| Production config blocked | Produce proposed diff only | config patch, risk notes, rollback checklist | Apply settings through an unreviewed side channel |

## Decision Boundary

Use the fallback immediately when it only creates local, preview, draft, or proposed artifacts.

Pause for the user when continuing would mutate real user data, alter production state, bypass permission boundaries, require secrets, or make an irreversible external change.

## Common Mistakes

| Mistake | Correct behavior |
| --- | --- |
| Starting with login before doing any work | Build, draft, inspect, or prepare everything possible first |
| Retrying OAuth, cookies, API keys, and browser sessions in sequence | Retry once, then search for low-privilege alternatives |
| Treating 401/403 as task failure | Treat it as a capability gap and produce the best available artifact |
| Asking the user to authenticate without context | Explain what is done, what is blocked, and the exact resume step |
| Waiting for auth when preview mode is enough | Use preview, temporary, guest, sandbox, or offline mode |
