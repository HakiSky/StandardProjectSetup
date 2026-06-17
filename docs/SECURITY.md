# Security Guide

Use this file to keep security expectations visible during planning,
implementation, review, and release. Keep it project-specific over time, but use
the sections below as the default baseline for new Codex-assisted projects.

## Security Goals

- Protect users and their data.
- Protect repository, domain, hosting, deployment, and automation access.
- Avoid collecting unnecessary personal data.
- Keep public content clearly separated from private or administrative content.
- Prefer simple, maintainable controls over complex security systems that are
  not needed yet.
- Keep security checks explicit enough that Codex can update and test them when
  behavior changes.

## Scope

Document what this security guide applies to:

- Public application or website: TODO
- Admin, preview, or staging environments: TODO
- APIs, workers, server functions, or backend services: TODO
- Browser games, editors, or client-heavy tools: TODO
- External services such as payments, analytics, hosting, or email: TODO

Out of scope:

- TODO

## Baseline Principles

### Use The Minimum Required Surface

- Do not add authentication if the project does not need accounts.
- Do not add a database if local or static data is enough.
- Do not add uploads if text, JSON import, or local-only processing is enough.
- Do not add third-party scripts when a normal link or server-side integration is
  enough.
- Do not add an API if static files solve the problem.

### Do Not Trust The Client

Anything running in a browser, mobile app, desktop client, or game client can be
modified by users.

Treat these as untrusted unless verified server-side:

- Scores and leaderboards
- Save files and local storage
- Unlocks, inventory, timers, and achievements
- Imported project files
- Share-link contents
- Form submissions and API requests

Client-side data can be useful for convenience, but it must not be the only
source of truth for money, identity, permissions, moderation, or competitive
state.

### Treat Public Content As Public

Anything reachable without authentication should be considered public. Secret or
unguessable links may be acceptable for low-risk sharing, but they are not a
replacement for authentication or authorization.

### Centralize Reusable Security Controls

When a project grows, keep reusable security configuration in one place:

- Security headers
- CORS allowlists
- Rate-limit rules
- Input schemas and size limits
- Logging conventions
- Error response format
- Deployment and release checklist

## Sensitive Surfaces

| Surface | Current Status | Required Review |
| --- | --- | --- |
| Secrets and environment variables | TODO | Check no secrets are committed or exposed to clients |
| Authentication | TODO | Confirm identity handling and session lifetime |
| Authorization | TODO | Confirm permission boundaries and admin-only actions |
| File handling, uploads, and imports | TODO | Check paths, size limits, content types, and parsing |
| Network access and external URLs | TODO | Check SSRF, redirects, CORS, and allowlists |
| Database access | TODO | Check migrations, queries, user data, and backups |
| Build and deployment | TODO | Check release secrets, artifact contents, and rollback |
| Browser storage | TODO | Check local storage, cookies, and sensitive data exposure |
| User-generated content | TODO | Check XSS, sanitization, moderation, and abuse handling |
| Payments or donations | TODO | Prefer external providers; do not handle card data directly |
| Analytics, logging, and monitoring | TODO | Avoid unnecessary personal data and secret leakage |

## Accounts, Access, And Repository Protection

- Enable MFA or passkeys for repository, hosting, domain, and deployment
  accounts.
- Prefer scoped API tokens over global API keys.
- Remove unused tokens and unused collaborators.
- Enable branch protection for production branches.
- Use pull requests for production changes when practical.
- Enable secret scanning and dependency update alerts where available.
- Keep deployment logs private and avoid printing secrets.
- Protect preview, staging, and admin environments when they contain sensitive
  data or unreleased work.

## Secrets And Configuration

Examples of secrets:

- API tokens
- Webhook secrets
- OAuth client secrets
- Database URLs
- Signing keys
- Analytics or email provider secrets
- Admin passwords

Rules:

- Never commit secrets to the repository.
- Never expose secrets in frontend code.
- Never include secrets in screenshots, logs, fixtures, or sample data.
- Add `.env` files and local secret files to `.gitignore`.
- Use environment variables, hosting secrets, CI/CD secrets, or a secret manager.
- Use separate secrets per environment.
- Rotate secrets immediately if exposure is suspected.

Unsafe:

```ts
export const API_SECRET = "secret_123";
```

Preferred:

```ts
const secret = env.WEBHOOK_SECRET;
```

## Web Security Baseline

For public websites, web apps, browser games, and static deployments:

- Use HTTPS for all public traffic.
- Redirect HTTP to HTTPS.
- Enable DNS, hosting, and deployment protections provided by the platform.
- Use security headers.
- Keep Content Security Policy strict and project-specific.
- Avoid unnecessary third-party scripts.
- Validate all external input.
- Treat local storage and client state as user-controlled.

### Security Headers

Recommended starting point:

```txt
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=(), payment=()
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'; base-uri 'self'; object-src 'none'; frame-ancestors 'none'; img-src 'self' data: https:; font-src 'self' data:; style-src 'self' 'unsafe-inline'; script-src 'self'; connect-src 'self'; form-action 'self'; upgrade-insecure-requests
```

For browser games or media-heavy apps, the CSP may need carefully tested
exceptions such as `blob:` for images, media, workers, or scripts. Add only the
permissions the actual build requires.

For WebAssembly builds, `wasm-unsafe-eval` may be needed in `script-src`. Do not
enable it unless the concrete build requires it.

CSP checklist:

- Test CSP per app or deployment target.
- Avoid wildcard script sources.
- Avoid third-party scripts when possible.
- Use report-only mode when introducing a new CSP to an existing app.
- Document required CSP exceptions and why they exist.

### CORS

- Only allow known origins.
- Do not combine `Access-Control-Allow-Origin: *` with credentials.
- Set `Vary: Origin` when reflecting allowed origins.
- Do not enable browser cross-origin access for APIs that do not need it.

Example:

```ts
const allowedOrigins = new Set([
  "https://example.com",
  "https://app.example.com",
]);

const origin = request.headers.get("Origin");

if (origin && allowedOrigins.has(origin)) {
  response.headers.set("Access-Control-Allow-Origin", origin);
  response.headers.set("Vary", "Origin");
}
```

## Input Validation

Validate all external input:

- URL parameters
- Query strings and hash fragments
- Form fields
- JSON bodies
- Imported files
- Local storage data
- Share-link data
- Score submits
- Feedback messages
- Webhook payloads

Recommended checks:

- Schema validation with a project-approved validator.
- Maximum string lengths.
- Maximum object counts.
- Maximum file and request sizes.
- Allowed content types.
- Rejection or cleanup of unexpected fields.
- Generic error messages for unsafe or malformed input.

## XSS And User-Generated Content

- Never render untrusted content as HTML without sanitization.
- Avoid `innerHTML` with user-controlled data.
- Render plain text with `textContent` or framework escaping.
- Sanitize Markdown or rich text before rendering.
- Validate URLs and block `javascript:` URLs.
- Remove event handlers from user-controlled HTML.
- Use `rel="noopener noreferrer"` for external links opened in a new tab.

Unsafe:

```ts
element.innerHTML = userInput;
```

Preferred:

```ts
element.textContent = userInput;
```

## Uploads, Imports, And Files

Prefer local imports over server uploads when possible. If uploads or imports
are needed:

- Limit file size.
- Check content type and do not trust file extensions alone.
- Validate file structure with a schema.
- Reject deeply nested or oversized data.
- Do not serve uploaded files as executable content from the same origin.
- Do not display SVG or HTML uploads without careful sanitization.
- Use random server-side filenames for stored uploads.
- Handle parse failures gracefully.

For JSON imports:

- Use `try/catch` around parsing.
- Validate schema after parsing.
- Enforce maximum object counts and string lengths.
- Treat imported data as untrusted even if it came from this app previously.

## Browser Games And Client-Heavy Tools

Browser game clients and local editors are user-controlled.

Suitable for local state:

- Settings
- Local progress
- Non-competitive unlocks
- Local high scores
- Graphics and audio options

Not suitable without server verification:

- Secure identity
- Global leaderboards
- Valuable unlocks or rewards
- Payment status
- Admin permissions

If leaderboards or competitive features exist:

- Rate-limit score submissions.
- Use run IDs, session IDs, or nonces where useful.
- Check score plausibility.
- Flag suspicious scores.
- Allow moderation or reset.
- Avoid displaying sensitive user data.

For multiplayer or competitive games, plan for server-side validation,
authoritative simulation, replay verification, signed events, anti-replay
controls, moderation, and abuse handling.

## Rate Limiting And Abuse Protection

Add rate limits where abuse is realistic:

- Feedback forms
- Share-link creation
- Score submissions
- Imports or uploads
- Login and admin actions
- Expensive API routes

Record concrete project limits here:

```txt
POST /api/feedback: TODO
POST /api/share: TODO
POST /api/score: TODO
POST /api/import: TODO
```

## Dependency And Build Security

- Commit lockfiles.
- Use reproducible install commands in CI.
- Enable dependency alerts where available.
- Remove unused packages.
- Review new packages before adding them.
- Avoid large dependency trees when a small local solution is safer.
- Do not copy external snippets, tutorials, templates, or generated code without
  review.

Pay extra attention to external code touching:

- Authentication and authorization
- API handlers
- File imports and uploads
- CORS and CSP
- HTML or Markdown rendering
- Score submissions
- Database access
- Redirects
- Shell commands

## CI/CD And Release Baseline

Every pull request or release should run the project-relevant subset of:

```txt
install dependencies with the lockfile
lint
typecheck
test
build
security or dependency audit
```

Deployment rules:

- Production deploys should be traceable to a commit.
- Preview deployments should not expose sensitive content by default.
- Rollback should be possible and tested before the project becomes critical.
- Build logs should not expose secrets or private data.

## Logging, Monitoring, And Privacy

Log enough to debug and respond to abuse, but avoid collecting unnecessary
personal data.

Useful API logs:

- Request ID
- Timestamp
- Route or action
- HTTP method
- Status code
- Error class
- Rate-limit hits
- Deployment version

Do not log:

- Secrets or tokens
- Payment data
- Private content without a clear need
- Full personal data histories without a retention policy
- Sensitive request or response bodies

For public projects, plan for:

- Privacy notice
- Cookie or local storage explanation if needed
- Analytics disclosure if analytics are used
- Deletion contact or deletion process for stored user content

This document is not legal advice.

## Release Security Checklist

Before publishing a new project, app, subdomain, or major feature:

- Project URL and deployment target are clear.
- HTTPS works.
- Security headers are configured.
- CSP is tested.
- No secrets are present in frontend code or tracked files.
- Unnecessary third-party scripts are avoided.
- Inputs, imports, uploads, and share data are validated.
- Local storage is robust against corrupt or malicious data.
- User content is escaped or sanitized.
- External links use safe attributes when opened in a new tab.
- Build is reproducible.
- Dependencies are reviewed.
- Preview or staging access is protected when needed.
- Rollback path is known.
- Privacy notice is updated when data collection changes.

## Incident Checklist

If a security issue is suspected:

- Identify the affected app, route, feature, user group, or deployment.
- Disable the feature, route, deployment, or integration if needed.
- Review hosting, repository, deployment, and security logs.
- Rotate secrets if exposure is possible.
- Preserve relevant evidence without spreading sensitive data.
- Reproduce the issue safely.
- Implement a fix.
- Add or update tests.
- Deploy the fix.
- Verify the fix in production or the affected environment.
- Notify affected users or maintainers if data or access was impacted.
- Update this document with any new prevention steps.

## Security Review Checklist

- Are secrets or credentials kept out of tracked files?
- Does the change accept user input or external data?
- Does the change read, write, upload, download, parse, render, or execute files?
- Does the change introduce new permissions, roles, or admin actions?
- Does the change expose internal data in logs, errors, client responses, or
  analytics?
- Are dependencies pinned, reviewed, and necessary?
- Are failure cases handled without leaking sensitive information?
- Are preview, staging, and admin surfaces protected when needed?
- Does the change affect privacy, local storage, cookies, analytics, or user
  data retention?
- If the project is a game or client-heavy tool, does the implementation avoid
  trusting client-side state for competitive or valuable outcomes?

## Codex Security Prompts

```text
Review this change for security impact. Check docs/SECURITY.md, identify any
new sensitive surfaces, and update the security guide if assumptions or required
checks changed.
```

```text
Before adding this feature, identify whether it introduces user input, file
handling, browser storage, authentication, authorization, external network calls,
database writes, third-party scripts, or deployment secrets. Update
docs/SECURITY.md and docs/TESTING.md with required checks.
```

```text
Prepare a release security review. Check security headers, CSP, secrets,
dependencies, input validation, preview access, rollback, privacy notes, and
incident response steps.
```
