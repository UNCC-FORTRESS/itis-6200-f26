# Lab 05 — Grading Rubric

**Total: 100 points** (Step 1: 54, Step 2: 20, Step 3: 26). See `../SCREENSHOT_PENALTY_POLICY.md` for the missing-screenshot cap policy, applied on top of this breakdown.

## Step 1: BLP Access Control Visualizer — 54 pts
18 scenarios, 3 pts each:
- **[1]** console-output screenshot for the case, boxed red.
- **[1]** correct allowed/denied determination for every read/write in the case.
- **[1]** brief (1-2 sentence) correct explanation of *why*, referencing the specific BLP rule (no read up / no write down) that applies.

## Step 2: URLs and Intro to Web — 20 pts
- **Q1-Q5 (1 pt each):** correct same-origin determination for each of the 5 given URLs, with protocol/domain/port justification.
- **Q6 (3 pts):** how URL escaping can bypass a literal-string path filter, and why it works.
- **Q7 (8 pts):** the HTML/JS/DOM relationship, and how an attacker exploiting it can execute a phishing attack.
- **Q8 (4 pts):** the functional difference between a URL's domain and port, and why a server might use multiple ports.

## Step 3: Cookies — 26 pts
- **Q9 (10 pts):** the operational difference between the Same-Origin Policy and the browser's Cookie Policy, with a concrete example scenario.
- **Q10 (5 pts):** why "SOP blocks CSRF" is a flawed claim.
- **Q11 (6 pts):** why relying solely on the `Referer` header for CSRF defense is weak, and why `SameSite=Strict` is more robust.
- **Q12 (5 pts):** why a direct session-token theft via injected JS would fail against an `HttpOnly` cookie, and what alternative attack the attacker could still use.

**Deduction note (applies across the whole lab, not just this step):** -0.5 per incorrect markup (must be done in-document, not on the raw image) and -0.5 per incorrectly-formatted screenshot (must be full-screen), per `guidelines.txt` — these are separate from, and stack with, the whole-assignment missing-screenshot cap in `SCREENSHOT_PENALTY_POLICY.md`.
