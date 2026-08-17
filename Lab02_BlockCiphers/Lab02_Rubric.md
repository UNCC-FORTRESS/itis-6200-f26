# Lab 02 — Grading Rubric

**Total: 100 points.** See `../SCREENSHOT_PENALTY_POLICY.md` for the missing-screenshot cap policy, applied on top of this breakdown. **Penalty: -20 points for submitting generic/example values, duplicate screenshots, or tampered evidence**, on top of any missing-screenshot cap.

## Part 1: Student Identity Parameters (SIP) — 10 pts
- **[10]** Your experiment inputs (image/data used) are correctly personalized per your variant's SIP derivation.
- **[0]** Generic example inputs used instead (e.g., a stock test image, generic placeholder text).

## Part 2: Experiment Evidence — 40 pts
- **Screenshot (Step 1, ECB vs CBC) — 20 pts.** Clear side-by-side comparison showing the pattern-preserving ECB result vs. the noise-like CBC/CTR result. **[10]** partial credit for a correct-tool-wrong-input screenshot.
- **Screenshot (Step 2/3, error propagation & two-time pad) — 20 pts.** Clear screenshots of the CBC decryption error pattern and the recovered-plaintext attacker's view. **[0]** if missing or the wrong artifact.

## Part 3: Analysis (Q1-Q6) — 50 pts
- **Q1 (8 pts):** why ECB preserves visual/structural patterns, and what that implies about encrypting repetitive data.
- **Q2 (8 pts):** why CBC fixes the pattern-leakage problem.
- **Q3 (9 pts):** which mode (CTR/CBC/PCBC) is best for a lossy streaming scenario, and why.
- **Q4 (8 pts):** why standard CBC is generally preferred over PCBC despite PCBC making tampering more obvious.
- **Q5 (9 pts):** why reusing a stream-cipher key+nonce is strictly forbidden.
- **Q6 (8 pts):** what happens to the confidentiality of many messages if even one is compromised under key reuse.

Each question is graded on correctness of the core security reasoning, not just a correct final observation — an answer that names the right outcome without explaining the underlying mechanism should not receive full credit.
