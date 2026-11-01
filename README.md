# XYZ University Portal (Demonstration)

A static, single-file demonstration of a university academic and student services portal.

**All names, courses, grades, attendance figures and accounts are fictional sample data. This demo is not connected to any official university database and is not an official university service.**

Developed by Muhammad Sudaid Hanif.

## Files
- `index.html`: the entire application (HTML, CSS, JavaScript). No build step, no server runtime, no external requests.
- `.nojekyll`: tells GitHub Pages to serve the file as-is.
- `README.md`: this file.

## Run locally
Open `index.html` in a browser, or run `python3 -m http.server` in this folder and visit http://localhost:8000. Registration needs a secure context (localhost or HTTPS).

## Publish on GitHub Pages
1. Create a new public repository on github.com (for example `xyz-portal`).
2. Upload `index.html`, `.nojekyll` and `README.md` to the repository root.
3. Settings > Pages > Build and deployment > Source: "Deploy from a branch", branch `main`, folder `/ (root)`, then Save.
4. After about a minute the site is at `https://<your-username>.github.io/<repository-name>/`.

## Configuration
Edit the `UNI` (branding), `CAT` (departments, programs, courses) and `DEMO` (public demo logins) objects near the top of the script.

## Public demo logins (never reuse in production)
- Student: `student@xyz.demo` / `demo123`
- Faculty: `faculty@xyz.demo` / `faculty123`

## Security notes (static demo)
- **Not real authentication.** Sign-in, roles and permissions are checked only in the visitor's browser, so anyone can bypass them with the browser console. Do not use this for real student data.
- Registered demo accounts are stored in the visitor's own browser (`localStorage`): name, email, student ID, and a salted PBKDF2-SHA-256 hash (600,000 rounds). Stored records are validated on load (character sets, program, section, salt format, and an iteration count between 100,000 and 1,000,000); malformed records are discarded before any hashing. Accounts without an iteration field are treated as 100,000 rounds.
- User-entered text is restricted by allowlists (names, student IDs, emails) and HTML-escaped before it is placed in markup. Plain-text-only outputs (toasts, counters, the password-reset message) use `textContent`/DOM nodes. All other markup is built from fixed templates plus escaped values. No `eval`, no inline event-handler attributes.
- **Content Security Policy** (a `<meta>` tag): `default-src 'none'; script-src 'unsafe-inline'; style-src 'unsafe-inline'; img-src data:; connect-src 'none'; base-uri 'none'; form-action 'none'`. It blocks all network requests, remote scripts, fonts, frames and form posts, so the page cannot send data anywhere. Limitation: because the app is one file with inline script and style attributes, `'unsafe-inline'` is required, so the policy does **not** stop injected inline script. Escaping and validation are the defences against that. Hardening options for a later version: move the script to a separate file or use a script hash (`'sha256-...'`, which must be regenerated on every edit and retested), and move inline `style=` attributes into the stylesheet. A `<meta>` policy also cannot set `frame-ancestors`; that needs a real HTTP header.
- Demo credentials are public and appear in the source and on the login screen by design.

## What needs a backend before real use
Real accounts and password reset; server-side authentication, sessions and per-record authorization (students see only their records, faculty only their courses, department and university administrators); server-side validation; a database; shared attendance, assignments, grades, announcements and notifications; file uploads with type, size and malware checks; rate limiting and lockout; audit logging; encrypted storage and backups; HTTP security headers (including `frame-ancestors`); privacy, retention and legal review. Not built here: administrator roles and screens, faculty registration/approval, grades and results, timetable editor with conflict detection, assignment file submissions, fees, complaints, academic calendar.

## Licence
Copyright (c) 2026 Muhammad Sudaid Hanif. All rights reserved. No open-source licence has been chosen; add one before allowing reuse.
