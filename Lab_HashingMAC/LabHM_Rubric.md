# Lab: Hashing & MAC — Grading Rubric

**Total: 100 points** (Part 0: 8, Part 1: 18, Part 2: 16, Part 3: 34, Part 4: 24). See `../SCREENSHOT_PENALTY_POLICY.md` for the missing-screenshot cap, applied **on top of** this breakdown. **Penalty: −20 points** for generic/example values instead of SIP-derived ones, duplicate screenshots, or tampered evidence.

This rubric gives you the exact point value of every item and *what* each item is assessed on. It does not contain the answers, expected numbers, or worked calculations — those live in the private grading key the TAs use. Every `Q#` is graded on the correctness of the underlying reasoning, not just a correct final number or the right one-word outcome: an answer that states the right result without explaining the mechanism gets at most half of that item.

**Maps to `../basicGradingRubric.md`:** the SIP part + every screenshot line roll up into the 40-pt *Format* band; "did the deliverable appear at all" into the 30-pt *Completion* band; every `Q#` into the 30-pt *Analysis* band.

---

## Part 0: Student Identity Parameters — 8 pts

- **[8]** `NAME`, `SID`, `SIP`, `FIRST`, `PASS` all present and correctly derived per the Part 0 table from your own name and Student ID.
- **[4]** table present but one value mis-derived (spaces left in `NAME`, wrong digit slice for `PASS`, etc.).
- **[0]** missing, or generic/example values used anywhere in the lab (also triggers the −20 penalty).

---

## Part 1: Hash Properties, Avalanche & a First MAC — 18 pts

**Evidence — 6 pts**
- **[2] SS1** — CyberChef SHA2-256 of `SIP`, input highlighted yellow, digest `D1` boxed red.
- **[2] SS2** — the one-character-changed input with digest `D2`, changed character boxed red, and your Hamming-distance result visible.
- **[2] SS3** — HMAC-SHA256 of `SIP` with key `FIRST` (key and `T1` boxed red), plus the key-`FIRST`+`x` result showing a different `T2`.
- **[0]** per item if missing, cropped, illegible, or if the "one-character" edit changed more than one character.

**Analysis — 12 pts**
- **[4] Q1** — reports the Hamming distance as a value out of 256 and as a fraction; names the property a ~50% result demonstrates; explains why hash-then-sign is insecure without it. Full marks require the mechanism, not just the property's name.
- **[4] Q2** — generic work for both a preimage and a collision attack on an `n`-bit hash, plus the four concrete exponents (MD5 and SHA-256, each attack). All four required for full marks; formula right but exponents wrong → **[2]**.
- **[4] Q3** — (a) what an attacker who sees the message and tag but not the key can recover about the message; (b) why a plain unkeyed hash sent alongside a message stops accidental corruption but not a deliberate attacker. Full marks require the accidental-vs-deliberate distinction stated explicitly.

---

## Part 2: Collisions — 16 pts

**Evidence — 4 pts**
- **[2] SS4** — blocks A and B both producing the **same** MD5 digest (the value stated in the handout), the two identical digests highlighted.
- **[2] SS5** — blocks A and B producing **different** SHA2-256 digests.
- **[0]** per item if the shown digests are inconsistent with the handout or the wrong operation/tab is used.

**Analysis — 12 pts**
- **[4] Q4** — the birthday-bound expectation for a random MD5 collision, compared against the cost of a real modern MD5 attack, and the correct conclusion about *why* MD5 is broken (brute force vs. a named design weakness). Both the numeric comparison and the named weakness required.
- **[4] Q5** — (a) why a collision in one hash function implies nothing about another (must explain, not just assert); (b) a correct definition of the difference between a collision and a second-preimage, and which one the "sign this document's hash" scenario is exposed to.
- **[4] Q6** — one concrete real-world attack that only needs a collision (not a preimage), naming both attacker-controlled artifacts and why the collision is what makes it work.

---

## Part 3: Length-Extension Attack vs. HMAC — 34 pts

**Evidence — 12 pts**
- **[2]** public GitHub repo `LabHM_LengthExtension`, **or** the script file submitted.
- **[2] SS6** — legitimate guest token generated and verified `True`.
- **[3] SS7** — successful forgery against the naive MAC: recovered secret length boxed red, the `ACCEPTED` line highlighted, `role=admin` visible in the forged message, and no point at which the attacker code was handed the secret.
- **[3] SS8** — the same forgery attempt **rejected** by the `--hmac` version.
- **[2] SS9** — CyberChef HMAC-SHA256 reproducing the script's `legit tag` from the revealed secret (Hex key) + message, matching tags highlighted.
- Running a modified script is explicitly allowed — no deduction for it.

**Analysis — 22 pts**
- **[5] Q7** — a correct step-by-step account of why the digest can be resumed as hash state, and the exact Merkle–Damgård property that permits it. Half marks if the narrative is broadly right but the "digest = resumable state / no finalisation transform" point is missing.
- **[4] Q8** — why a real server-side parser accepts the glue-padding bytes sitting mid-message, and what that implies about "malformed input would be noticed" defenses. Both halves required.
- **[5] Q9** — precisely why HMAC's outer hash defeats the attack: what the attacker holds and why it cannot be extended into a valid tag. Must reference the outer hash specifically.
- **[4] Q10** — two correct non-HMAC fixes for a secret-prefix MAC, each with the mechanism by which it removes the attack (2 pts each).
- **[4] Q11** — the structural reason sponge constructions (SHA-3 / KMAC) are not length-extendable while Merkle–Damgård is. Half marks for "sponge is different" without the withheld-state / capacity point.

---

## Part 4: Password Hashing & KDFs — 24 pts

**Evidence — 8 pts**
- **[3] SS10** — the same `PASS` producing one deterministic unsalted digest and two **different** salted digests, the two differing values highlighted.
- **[3] SS11** — the PBKDF2 (three iteration counts) + scrypt timing table, per-setting time and estimated guessing rate boxed.
- **[2] SS12** — CyberChef Bcrypt run twice on the same `PASS` producing two different hash strings, both outputs shown, the differing salt portion boxed.

**Analysis — 16 pts**
- **[4] Q12** — which specific attack the **salt** defeats and which specific attack the **iteration count** defeats, correctly kept separate. Both required for full marks.
- **[4] Q13** — picks an iteration count consistent with their own measured <250 ms figure and shows the arithmetic for the factor drop in attacker guess rate versus raw SHA-256, using their own table numbers.
- **[4] Q14** — why a large per-guess memory requirement specifically hurts a GPU/ASIC cracking rig when a raw iteration count does not.
- **[4] Q15** — the denial-of-service a deliberately slow hash creates, and a standard mitigation that keeps the slow hash.

---

## Deduction summary

| Item | Effect |
|---|---|
| Missing required screenshot(s) | max-score cap per `../SCREENSHOT_PENALTY_POLICY.md` |
| Generic / example values instead of SIP | −20, and Part 0 → 0 |
| Duplicate or tampered screenshot | −20 |
| Late | −20 per 24h, no submission after 48h (`../basicGradingRubric.md`) |
| Cannot explain own submission when asked | integrity referral per `../guidelines.txt` |
