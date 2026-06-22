# Fallback Options

| Original goal | Preferred fallback |
| --- | --- |
| Deploy to Cloudflare, Vercel, Netlify, or similar | Temporary workspace, preview deploy, local server, static export |
| Publish to GitHub | Patch file, branch diff, commit bundle, local repository changes |
| Write to Notion, Google Docs, Confluence, or CMS | Markdown, HTML, DOCX, import-ready content |
| Create Jira, Linear, Asana, or Trello issue | JSON export, CSV, Markdown ticket, API payload preview |
| Send email or social post | Draft, preview, subject/body, images, captions, scheduling notes |
| Upload to S3, OSS, Drive, or Dropbox | Local file, archive, manifest, upload instructions |
| Modify production settings | Proposed config diff, risk notes, manual checklist |

## Common Mistakes

| Mistake | Correct behavior |
| --- | --- |
| Starting with login before doing any work | Build, draft, inspect, or prepare everything possible first |
| Retrying OAuth, cookies, API keys, and browser sessions in sequence | Retry once, then search for low-privilege alternatives |
| Treating 401/403 as task failure | Treat it as a capability gap and produce the best available artifact |
| Asking the user to authenticate without context | Explain what is done, what is blocked, and the exact resume step |
| Waiting for auth when preview mode is enough | Use preview, temporary, guest, sandbox, or offline mode |
