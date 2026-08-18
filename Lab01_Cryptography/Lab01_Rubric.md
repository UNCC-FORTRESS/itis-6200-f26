# Lab 01 — Grading Rubric

**Total: 100 points.** Graded against `Lab01__Final.md` (the assigned lab — Caesar + Vigenère + Substitution + AES Avalanche Effect). `Lab01_VariantA/B/C.md` are optional single-cipher alternates kept in this folder for reference/enrichment, not separately graded. See `../SCREENSHOT_PENALTY_POLICY.md` for the missing-screenshot cap policy, applied on top of this breakdown. **Penalty: -20 points for submitting generic/example values, duplicate screenshots, or tampered evidence**, on top of any missing-screenshot cap.

## Part 1: Student Identity Parameters (SIP) — 10 pts
- **[10]** Plaintext, shift/keyword/substitution-key, and AES key across all four steps are correctly derived from your own name/Student ID per the formulas in Steps 1.1, 2.1, 3.1, and 4.1.
- **[0]** Using generic/example values (e.g., a textbook shift of 3, the literal word "KEY") instead of your ID-derived values, in any step.

## Part 2: Experiment Evidence (Step 6, items 1-7) — 40 pts
- **[5] Caesar Setup** — tool output with Shift boxed red.
- **[5] Caesar Frequency** — chart with the most frequent letter arrowed green.
- **[5] Vigenère Setup** — tool output with Keyword boxed red.
- **[5] Vigenère Frequency** — chart showing the flattened distribution.
- **[5] Substitution Setup** — tool output with Key boxed red.
- **[5] Substitution Frequency** — chart showing surviving peaks.
- **[10] Avalanche Effect** — `AvalancheAES` screenshot showing the flipped bit and the scrambled/garbage decryption result.
- **[0]** per item if missing, illegible, or not from the correct tool/tab.

## Part 3: Analysis (Step 5) — 50 pts

**5.1 Keyspace Analysis — 25 pts**
- **[8] Caesar:** why checking all 26 keys is "insecure" even for a human despite the tiny keyspace.
- **[8] Vigenère:** correct calculation of $26^5$ and why it's insecure against a modern computer.
- **[9] Substitution:** correct approximation of $26!$ in scientific notation, and why this resists brute force but not frequency analysis.

**5.2 Avalanche Analysis — 20 pts**
- **[7]** Observation: whether the decrypted message changed slightly or completely after a single flipped ciphertext bit.
- **[7]** CBC vs. CTR comparison: whether the corruption spreads to the whole block/rest of stream or stays local to the flipped bit.
- **[6]** Reflection: why the avalanche effect is desirable, and what a weak (non-avalanching) cipher would expose to an attacker.

**5.3 Real-World Security Principles — 5 pts**
- **[5]** Three distinct real-life security principles (e.g., Least Privilege, Defense in Depth, Separation of Duties) with a concrete example of each, not copied from the lab's own cafe example.
- **[3]** partial credit if only 1-2 principles are given, or examples are vague/unexplained.

Each analysis item is graded on correctness of the underlying reasoning, not just a correct final number or observation — an answer that states the right outcome without explaining the mechanism should not receive full credit.
