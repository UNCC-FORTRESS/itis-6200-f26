# Lab 03 — Grading Rubric

**Total: 100 points** (Part 1: 45, Part 2: 40, Part 3: 15). See `../SCREENSHOT_PENALTY_POLICY.md` for the missing-screenshot cap policy, applied on top of this breakdown.

## Part 1: Authenticated Encryption Analysis — 45 pts
9 schemes, 5 points each (3 for screenshots, 1 each for confidentiality/integrity analysis):
- **[3]** Full screenshots (Alice's side + Bob's side) per `guidelines.txt` markup conventions.
- **[1]** Correct confidentiality determination + reasoning.
- **[1]** Correct integrity determination + reasoning.

Note: several of the 9 schemes are intentionally broken by design — grading rewards correctly *identifying and explaining* why a scheme fails, not just picking "secure" for all of them.

## Part 2: Diffie-Hellman & PRNG Rollback Analysis — 40 pts
- **Q2 (3 pts):** correct calculation of Alice's public key.
- **Q3 (3 pts):** correct calculation of the shared secret from Alice's side.
- **Q4 (3 pts):** correct calculation of the shared secret from Bob's side (must match Q3).
- **Q5 (9 pts):** correct determination of whether Eve can retroactively recover S with non-rollback-resistant PRNGs, with full reasoning.
- **Q6 (10 pts):** correct determination of whether S is secure when only one party uses a rollback-resistant PRNG, with full reasoning.
- **Q7 (12 pts):** a mathematically valid value forcing S=1, with a rigorous proof using the student's own specific `p`/`g`/`a`/`b` values (not generic/example values).

## Part 3: Programming Assignment — 15 pts
- **[15]** all three required deliverables present: public GitHub repo link (titled `Lab03DHProgram`), Scenario A console screenshot, Scenario B console screenshot clearly showing the original vs. Mallory-modified message. **[0] if any of the 3 is missing** — this is an all-or-nothing deliverable, not partial credit per item.
