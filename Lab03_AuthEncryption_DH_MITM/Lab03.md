# Lab 03: Authenticated Encryption, Diffie-Hellman & PRNG Analysis, and the Stateful Interceptor

**Ported from the real Spring 2026 Canvas assignment** (`original_docs/Lab03_Original.docx`, "Lab03 Revised (1).docx" — that original file is the authoritative source of record and includes screenshot examples and equation diagrams that don't survive this markdown port; consult it directly for anything ambiguous below). This replaced the never-deployed `archive/Lab03_HashesMACs/` and `archive/Lab03_05_MegaLab/` designs — see `../archive/README.md`.

Total: 100 points (Part 1: 45, Part 2: 40, Part 3: 15).

## Part 1: Authenticated Encryption Analysis (45 points)

Alice wants to send Bob a message providing both **confidentiality** and **integrity**. Using **[CyberChef](https://uncc-fortress.github.io/CyberChef/)** (backup: [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/)), evaluate each of 9 candidate schemes using your own personalized parameters (below), and determine which scheme(s) actually deliver both properties.

**Your personalized parameters:**
- `M` = "[Your Name] does not know which one is Diffie and which is Hellman"
- `K1` = [Your Student ID] × 7
- `K2` = [Your Student ID]
- `IV` = FirstName + LastName, padded with your repeated Student ID to reach 16 bytes
- Encryption mode: AES-CBC
- HMAC: HMAC-SHA256
- Hash (where called for): SHA-256

CyberChef operations you'll need: "AES Encrypt" / "AES Decrypt", "HMAC", "SHA2".

**The 9 schemes.** Each scheme has Alice send either `(C, H)` or just `(C)` to Bob — some combine encryption and a hash/HMAC correctly, some don't. The original docx renders each scheme's exact construction as an equation image; a few examples recovered from the source file's embedded formulas to illustrate the pattern:
- `C = AES-CBC(K1, (M || HMAC(K2, M)))` — HMAC computed over the plaintext, then the whole thing encrypted (MAC-then-encrypt).
- `C = AES-CBC(K1, Hash(K1||M))` — a keyed hash of key‖plaintext, then encrypted.
- `H = HMAC(K2, C)` (separately) — HMAC computed over the ciphertext (encrypt-then-MAC), sent alongside `C`.
- `C = AES-CBC(K1, Hash(M))` — an unkeyed hash of the plaintext, then encrypted (no real integrity key at all).

**Important:** several of the 9 schemes are *intentionally broken by design* — some don't provide confidentiality and/or integrity, and some parameters are deliberately out of spec. This is a known, deliberate part of the lab (confirmed by the instructor in a Spring 2026 announcement, "Lab 03 doubts and update," after students initially read Scheme 4's missing key as a bug). Fall 2026 handout: **state this explicitly up front** rather than relying on a mid-semester clarification post — that announcement, plus a follow-up "Lab 03 update" with revised reference images, was needed last time specifically because the wording wasn't clear enough the first time. (The Spring 2026 reference images from that fix were in the Canvas Files area, not this repo — re-attach updated reference screenshots here if this scheme-ambiguity issue recurs.)

**Deliverables per scheme (5 points each: 3 for screenshots, 1 each for confidentiality/integrity analysis):**
- Full screenshot, Alice's side: input message highlighted yellow, output ciphertext boxed red.
- Full screenshot, Bob's side: input ciphertext highlighted yellow, output plaintext boxed red.
- Analysis: does the scheme provide confidentiality? If not, how can a passive eavesdropper (Eve) recover `M`?
- Analysis: does the scheme provide integrity? If not, how can an active attacker (Mallory) make undetected modifications?
- Conclusion: highlight in yellow whichever scheme(s) provide **both** properties.

## Part 2: Diffie-Hellman & PRNG Rollback Analysis (40 points)

**Personalized DH exchange.** Alice and Bob perform a DH key exchange using:
- Prime **p = 23**, generator **g = 5** (small values, intentionally hand-calculable)
- Alice's secret `a` = last digit of your Student ID (if 0 or 1, use 3)
- Bob's secret `b` = second-to-last digit of your Student ID (if 0 or 1, use 4)

**Tasks:**
- Q2 (3 pts): calculate Alice's public key `A = g^a mod p` and Bob's public key `B = g^b mod p`. Show your work.
- Q3 (3 pts): calculate the shared secret `S` that Alice computes (`S = B^a mod p`).
- Q4 (3 pts): calculate the shared secret `S` that Bob computes (`S = A^b mod p`) — should match Q3.

**PRNG rollback analysis** (Eve is eavesdropping on the above exchange):
- Q5 (9 pts): if Alice and Bob's PRNGs are *not* rollback-resistant, and Eve later learns their internal state, can Eve retroactively recover your shared secret `S`? Why or why not?
- Q6 (10 pts): if Alice switches to a rollback-resistant PRNG but Bob doesn't, is `S` still secure? Why or why not?
- Q7 (12 pts): Mallory intercepts the exchange and wants Alice and Bob to agree on a shared secret she knows in advance, specifically `S = 1`, without either side noticing a mismatch. Using your specific `p`, `g`, `a`, `b` values, find a value Mallory can substitute to force `S = 1` for both sides. Prove mathematically that it works. (Hint: what public value, raised to any secret exponent mod 23, always yields 1?)

## Part 3: Programming Assignment — The Stateful Interceptor (15 points)

Build a simulated secure session using Diffie-Hellman + a stateful PRNG stream cipher, then implement Mallory as an active man-in-the-middle who intercepts, decrypts, modifies, and re-encrypts traffic without either party detecting it.

**Starter template:** `Lab03DHProgram.py` in this folder (recovered from the Canvas Coding Templates area — matches the deliverable naming convention below). Do not remove or modify existing code; only add the logic described in its comments. You may use any language/libraries instead, but your console output must demonstrate the same two scenarios below.

**Classes/functions to implement:**
- `SecurePRNG`: `init()` seeds a 32-byte internal state from the DH shared secret; `generate()` produces N pseudorandom bytes; **must update its internal state via a hash function after every generation block** so the process can't be reversed (rollback resistance — this is Part 2's PRNG concept, made concrete).
- `stream_cipher()`: calls `prng.generate()` for a keystream, returns `plaintext XOR keystream`.
- `Entity` (Alice/Bob): `init()` sets up DH keys; `get_public_hex()` (already implemented, don't change); `establish_session()` computes the shared secret and seeds `SecurePRNG`.
- `Mallory`: `init()` sets up her own DH keys; `intercept()` — on a key exchange, stores the sender's key, generates her own fake shared secret, and returns her own public key instead; on an encrypted message, decrypts with the sender-side PRNG, modifies the plaintext, re-encrypts with the recipient-side PRNG.

**Two required execution flows:**
1. **Scenario A (secure):** Alice/Bob DH-exchange public keys, derive matching shared secrets, Alice stream-encrypts a message (your own choice of text), Bob decrypts it intact.
2. **Scenario B (MITM):** Mallory sits between Alice and Bob during key exchange, so each of them actually shares a secret with *her*, not each other. Alice encrypts a message; Mallory decrypts it, modifies a word, re-encrypts for Bob; Bob decrypts without error — he has no way to detect the tampering from the ciphertext alone.

**Deliverables (15 pts — 0 if any of the 3 are missing):**
- Public GitHub repo link, titled `Lab03DHProgram`.
- Full-screen screenshot of console output for Scenario A.
- Full-screen screenshot of console output for Scenario B, clearly showing your original message and Mallory's modified version.

## References & Further Reading

Lab-wise, if you face any difficulties regarding the setup, tool usage, or markup help, please feel free to reach out to the TAs during office hours or via email. All assessments and deductions are done at the discretion of the TAs, but mostly based off of a preset rubric to ensure fairness and consistency — if you have any reservations with how your submission was graded, please raise them within three days of receiving your points.

These explain the *concepts* behind Part 1's schemes and Part 2's key exchange — none of them walk through this lab's specific 9 schemes or tell you which ones are broken; that reasoning is yours to work out.

1.  **Computerphile:** [Secret Key Exchange (Diffie-Hellman)](https://www.youtube.com/watch?v=NmM9HA2MQGI) — the mechanics behind Part 2's key exchange.
2.  **Ref:** [Authenticated Encryption (Wikipedia)](https://en.wikipedia.org/wiki/Authenticated_encryption) — the general idea of combining confidentiality and integrity, background for reasoning about Part 1's schemes.

## AI Appendix & submission format

Follow `../guidelines.txt` (screenshot markup, naming convention, AI-use disclosure) — not restated here.
