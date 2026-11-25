# KinNai MVP1 — clickable prototype

Review artifact. Synthetic data only; no backend, no real location, no payments, no real login.

- `index.html` — open directly in a browser (standalone copy).
- `kinnai-prototype.html` — the same page without the document wrapper (source used for publishing).

Updated for D-016 to D-023 (existing colour palette kept):

- Kampai is the first tab. Organizer flow: create, share link, simulated friends join, start voting, vote, tie handling, confirm.
- Tie: the organizer is reminded with a 5-minute countdown; if they do not choose, the group closes with no result and they can restart voting.
- "Open as a friend" shows the participant view: no login, no tab bar, vote within 3 taps, result card, "create your own" button.
- Login (Apple / Google, simulated) is offered after a decision, skippable, not shown again for 7 days; the in-app-browser (LINE) case is simulated from the review console.
- Review console shows Time to Yes (Group TTY starts at "start voting", end-to-end guardrail), K = I x A x H worked example, requirement coverage and the analytics events.
- Solo flow is kept and no longer asks for login before confirming.

After editing `kinnai-prototype.html`, regenerate `index.html` by wrapping it in
`<!doctype html><html lang="th"><head>...</head><body>` ... `</body></html>`.
