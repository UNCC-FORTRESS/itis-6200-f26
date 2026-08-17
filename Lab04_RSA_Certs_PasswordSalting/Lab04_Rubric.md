# Lab 04 — Grading Rubric

**Total: 100 points** (Step 1: 24, Step 2: 30, Step 3: 20, Step 4: 26). See `../SCREENSHOT_PENALTY_POLICY.md` for the missing-screenshot cap policy, applied on top of this breakdown.

## Step 1: RSA Key Pairs and Encryption — 24 pts
- Deliverables (6 pts): 3 screenshots (key pair generation, Bob's encryption, Alice's decryption), 2 pts each.
- **Q1 (6 pts):** why it's safe to publish the public key but catastrophic to lose the private key, and the mathematical relationship between them.
- **Q2 (6 pts):** whether Alice can verify the message really came from Bob, and why/why not.
- **Q3 (6 pts):** why RSA operations use a message digest (hash) rather than operating on the raw message directly.

## Step 2: RSA Digital Signatures & Hybrid Encryption — 30 pts
- Deliverables (6 pts): 3 screenshots (Bob's key pair, Bob's signature, Alice's verification), 2 pts each.
- **Q4 (8 pts):** why signing uses the private key (not the public key), and whether public-key signing would still prove origin.
- **Q5 (8 pts):** whether Alice can actually prove a forum-sourced public key belongs to Bob and not an impostor.
- **Q6 (8 pts):** why hybrid encryption (AES for bulk data + RSA for the signature/key) is used instead of RSA for everything.

## Step 3: Certificates — 20 pts
- **Q7 (7 pts):** the specific chain-of-trust verification steps Alice must perform.
- **Q8 (7 pts):** whether a broken signing algorithm on an intermediate certificate still allows trust, and why/why not.
- **Q9 (6 pts):** what root trust anchors are, how they get onto a computer, and what happens if they're all deleted.

## Step 4: The Exponential Wall — 26 pts
- Deliverable (2 pts): screenshot of the salted-vs-unsalted brute-force timing comparison.
- **Q10 (8 pts):** how a visible/public salt still defeats pre-computed rainbow-table attacks.
- **Q11 (8 pts):** how a deliberately slow hash creates an online (availability) vulnerability even though it helps against offline attacks.
- **Q12 (8 pts):** why GPUs (not CPUs) are used for password cracking, and how salting specifically reduces GPU cracking efficiency.
