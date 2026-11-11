# KinNai MVP1 — clickable prototype

Review artifact for the Gate 1 decision. Synthetic data only; no backend, no real location, no payments.

- `index.html` — open directly in a browser (standalone copy).
- `kinnai-prototype.html` — the same page without the document wrapper (source used for publishing).

Covers SOLO-001…005 and GRP-001 (Kampai, simulated friends), with a review console showing
Time to Yes, scenario simulation (slow / one result / no result / provider error / location denied),
requirement coverage and the analytics events the app would emit.

After editing `kinnai-prototype.html`, regenerate `index.html` by wrapping it in
`<!doctype html><html lang="th"><head>…</head><body>` … `</body></html>`.
