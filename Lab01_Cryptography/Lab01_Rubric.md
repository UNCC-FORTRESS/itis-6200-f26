# Lab 01 — Grading Rubric

**Total: 100 points.** See `../SCREENSHOT_PENALTY_POLICY.md` for the missing-screenshot cap policy, applied on top of this breakdown. **Penalty: -20 points for submitting generic/example values, duplicate screenshots, or tampered evidence**, on top of any missing-screenshot cap.

## Part 1: Student Identity Parameters (SIP) — 10 pts
- **[10]** Your cipher parameters (shift/key/alphabet, depending on your variant) are correctly derived from your own Student ID per the formula in your variant's instructions.
- **[0]** Using generic/example values (e.g., a textbook shift of 3, or the literal word "KEY") instead of your ID-derived values.

## Part 2: Experiment Evidence — 40 pts
- **Screenshot 1 (Config/Ciphertext) — 20 pts.** Clear screenshot of the tool with your correctly-derived input. **[10]** partial credit if the input doesn't match your SIP but the encryption itself is technically executed correctly.
- **Screenshot 2 (Analysis/Chart) — 20 pts.** Frequency-analysis chart visible and consistent with your cipher's expected behavior. **[0]** if missing or irrelevant.

## Part 3: Analysis & Homework Questions — 50 pts
- **Q1, Keyspace Calculation — 25 pts.** Calculate the exact key space size for your specific variant. **[25]** correct formula and correct final number (scientific notation acceptable). **[15]** correct formula, arithmetic error. **[5]** unsupported guess.
- **Q2, Brute Force Feasibility — 25 pts.** Given a stated guesses-per-second rate, calculate how long a brute-force attack would take against your variant's keyspace. **[25]** correct dimensional analysis (including unit conversion, e.g. seconds to years, where relevant). **[10]** a bare qualitative answer ("instant" / "forever") with no supporting math.
