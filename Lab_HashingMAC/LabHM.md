# Lab: Hashing & Message Authentication Codes (MAC)

**Total: 100 points** (Part 0: 8, Part 1: 18, Part 2: 16, Part 3: 34, Part 4: 24).

**Topic:** Cryptographic hash functions and their security properties (preimage / second-preimage / collision resistance, the avalanche effect), and how those properties are — and are *not* — enough to build a secure message-authentication code. You will break a naive MAC with a length-extension forgery, fix it with HMAC, verify the fix independently in CyberChef, then look at why password storage needs the *opposite* of a fast hash.

**Tools required:**
- **[CyberChef](https://uncc-fortress.github.io/CyberChef/)** (backup: [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/)) — Parts 1, 2, 3 (cross-check), 4. Operations used: **MD5**, **SHA1**, **SHA2**, **SHA3**, **HMAC**, **From Hex**, **XOR**, **PBKDF2**, **Bcrypt**.
- **Python 3.8+** — Parts 3 and 4. **Standard library only, no `pip install`.** Scripts are in `tools/`:
  - `tools/sha256_lenext.py` — pure-Python SHA-256 + length-extension forger + a vulnerable/HMAC demo server.
  - `tools/password_kdf_bench.py` — salted vs. unsalted hashing and a KDF work-factor timing table.
  - `tools/md5_collision_blocks.txt` — the colliding input pair for Part 2.

As in Lab 03, you **may** re-implement any script in another language/toolchain, but your console screenshots must show the same scenarios.

---

## Part 0: Student Identity Parameters (SIP) — 8 points

Every value you hash in this lab is derived from your own identity. Submissions using generic or example values score **0 on this part and trigger the −20 penalty** (see rubric).

Derive and record these once, at the top of your report:

| Symbol | Derivation | Example (for "Ada Lovelace", SID `800123456`) |
|---|---|---|
| `NAME` | your full name, lowercased, spaces removed | `adalovelace` |
| `SID` | your 9-digit UNCC Student ID | `800123456` |
| `SIP` | `NAME + "-" + SID` | `adalovelace-800123456` |
| `FIRST` | your first name, lowercased | `ada` |
| `PASS` | `SIP + "!" + <last 2 digits of SID>` | `adalovelace-800123456!56` |

`SIP` is the "message" you hash in Parts 1–2. `FIRST` personalizes the API message in Part 3. `PASS` is the passphrase in Part 4.

**Deliverable:** a short table in your report showing all five values as *you* derived them.

---

## Part 1: Hash Properties, the Avalanche Effect & a First MAC — 18 points

**Context.** A cryptographic hash must behave like a random function: any change to the input — even one bit — should flip roughly half the output bits, with no predictable relationship between the change and the result (the *avalanche effect*). That is what lets a hash stand in for a whole document inside a signature. A **MAC** goes one step further: it mixes a secret **key** into the hash so that only someone who holds the key can produce or check the tag.

### Steps

1. CyberChef → **SHA2**, size **256**. Input: your `SIP`. Bake. Record the digest as `D1`.
2. Change **exactly one character** of `SIP` (state which). Re-bake. Record the digest as `D2`.
3. Compute the **bit difference (Hamming distance)** between `D1` and `D2`. In CyberChef: `From Hex` → `XOR` (scheme "Standard", key = the other digest as Hex) → look at the result, or add `To Binary` and count the `1`s. Or in Python: `bin(int(D1,16) ^ int(D2,16)).count("1")`. Record the count out of 256.
4. In fresh tabs, hash `SIP` with **MD5**, **SHA1**, **SHA2-256**, **SHA3-256**. Record all four digests and their lengths in bits.
5. **First MAC.** CyberChef → **HMAC**, hashing function **SHA256**, key **`FIRST`**. Input: `SIP`. Record the tag as `T1`. Now change the key to `FIRST` + `x` (one extra letter) and re-bake. Record the tag as `T2`.

### Deliverables (6 pts)

- **SS1 (2 pts):** CyberChef SHA2-256 of `SIP` — input highlighted yellow, digest `D1` boxed red.
- **SS2 (2 pts):** the one-character-changed input with digest `D2` — changed character boxed red — and your Hamming-distance result visible.
- **SS3 (2 pts):** CyberChef HMAC-SHA256 of `SIP` with key `FIRST` (key boxed red, `T1` boxed red), plus the key-`FIRST`+`x` result showing a completely different `T2`.

### Analysis (12 pts)

- **Q1 (4 pts):** Report your Hamming distance out of 256 bits and as a fraction. Which hash property does that result demonstrate, and roughly what fraction would you expect from an ideal hash? Explain why a signature scheme that hashes the message first would be *insecure* if the hash did **not** have this property.
- **Q2 (4 pts):** For an ideal `n`-bit hash, state the generic work (big-O of `n`) for (a) a preimage attack and (b) a collision attack. Give the four concrete exponents for MD5 (`n`=128) and SHA-256 (`n`=256).
- **Q3 (4 pts):** In Step 5 you changed only the *key* and got a totally different tag. An attacker sees the message `SIP` and the tag `T1` but not the key. (a) What can they compute about `SIP` from `T1` alone? (b) If Alice instead sent `SIP` next to a plain, unkeyed `SHA256(SIP)`, does that stop an *accidental* transmission error? Does it stop a *deliberate* attacker who wants Bob to accept a modified message? Explain the difference — this is the reason MACs exist.

---

## Part 2: Collisions — Why MD5 and SHA-1 Are Dead — 16 points

**Context.** Collision resistance is the first property to fall. `tools/md5_collision_blocks.txt` contains two 128-byte inputs, **A** and **B**, first published by Wang et al. They are different byte strings that hash to the *same* MD5 digest.

### Steps

1. Open `tools/md5_collision_blocks.txt`. Copy block **A**'s hex line.
2. CyberChef: **From Hex** → **MD5**. Record the digest. Repeat for block **B**. They should match — you should get `79054025255fb1a26e4bc422aef54eb4`.
3. Confirm A ≠ B: diff the two hex strings (CyberChef "Diff", or eyeball — only 6 bytes changed).
4. Run both blocks through **SHA2-256**. Record both digests.
5. *(Optional, no extra points, still good evidence:)* download `shattered-1.pdf` and `shattered-2.pdf` from <https://shattered.io> and hash both with **SHA1** in CyberChef — same digest, different files.

### Deliverables (4 pts)

- **SS4 (2 pts):** CyberChef showing block A and block B both producing MD5 `79054025…` — the identical digests highlighted yellow.
- **SS5 (2 pts):** CyberChef showing block A and block B producing **different** SHA2-256 digests.

### Analysis (12 pts)

- **Q4 (4 pts):** By the birthday bound, how many *random* MD5 inputs would you expect to try before a collision? Modern chosen-prefix MD5 attacks need roughly 2^39 or fewer. What does the gap between those numbers tell you — is MD5 broken because brute force got cheap, or because of a design/cryptanalytic weakness? Name the weakness in general terms.
- **Q5 (4 pts):** (a) Blocks A and B collide under MD5 but not SHA-256 — why does a collision in one hash imply nothing about another? (b) Define the difference between a **collision** and a **second-preimage**, and say which one matters for "here is a contract, sign its hash."
- **Q6 (4 pts):** Give one concrete real-world attack that a **collision** (not a preimage) is sufficient for. Name the two artifacts the attacker constructs and why the collision is what makes it work.

---

## Part 3: Building a Real MAC — Length-Extension Attack vs. HMAC — 34 points

**Context.** A web service authenticates API requests with a MAC. The developer chose the "obvious" construction:

```
tag = SHA256(secret || message)
```

and the server accepts a request if it can recompute the same `tag`. The attacker doesn't know `secret`, so they can't compute the tag — **but they don't need to.**

The request you'll attack is:

```
message = "user=<FIRST>&role=guest"
```

Your goal: produce a **different** message ending in `&role=admin` **with a valid tag**, without ever learning `secret`.

### Steps

1. Read `tools/sha256_lenext.py` top to bottom. It contains a pure-Python SHA-256 (asserted equal to `hashlib` at startup), the `sha256_extend()` length-extension primitive, a `Server` holding a random unknown `secret`, and an `attack()` routine that brute-forces `len(secret)` and forges the request.
2. Run: `python3 tools/sha256_lenext.py --first <FIRST>`
   - It prints and verifies the legitimate guest token, then forges `...&role=admin` and shows the server **accepting** it — including the recovered secret length and the raw glue-padding bytes now sitting inside the message.
3. Run the fixed version: `python3 tools/sha256_lenext.py --first <FIRST> --hmac`
   - Same attack against `tag = HMAC-SHA256(secret, message)`. The forgery **fails**. The script also prints the (normally hidden) secret in hex, labelled for your CyberChef cross-check.
4. **CyberChef cross-check.** From the `--hmac` run, take the revealed `secret` hex and the legit `message`. CyberChef → **HMAC**, hashing function **SHA256**, **key type: Hex**, key = the revealed secret, input = the message. Confirm the tag matches the script's `legit tag`. (Point: the "fix" is just a standard construction you can compute in any tool — nothing exotic.)

### Deliverables (12 pts)

- **(2 pts)** A link to a **public GitHub repo** titled `LabHM_LengthExtension` containing your (possibly modified) script, **or** the script file attached to your submission.
- **SS6 (2 pts):** console output — legitimate guest token generated and verified `True`.
- **SS7 (3 pts):** console output of the successful forgery against the naive MAC — recovered secret length boxed red, the `ACCEPTED` line highlighted yellow, `role=admin` visible in the forged message.
- **SS8 (3 pts):** console output showing the same forgery **rejected** by the `--hmac` version.
- **SS9 (2 pts):** CyberChef reproducing the HMAC `legit tag` from the revealed secret + message — matching tags highlighted yellow.

### Analysis (22 pts)

- **Q7 (5 pts):** Walk through, step by step, why knowing `SHA256(secret || m)` and the length of `secret || m` lets you compute `SHA256(secret || m || glue || suffix)` without `secret`. Which exact property of the Merkle–Damgård construction makes the digest a *resumable state* rather than a one-way endpoint?
- **Q8 (4 pts):** Your forged `message` contains raw glue padding — a `0x80` byte, a run of zeros, a 64-bit length field — in the middle of the string. Why does a typical server-side query-string parser accept it anyway? What does that say about defenses of the form "malformed bytes would be noticed"?
- **Q9 (5 pts):** HMAC is `H((k ⊕ opad) || H((k ⊕ ipad) || m))`. Explain precisely why the outer hash makes the length-extension forgery impossible — what does the attacker hold, and why can't they extend it into a valid tag?
- **Q10 (4 pts):** Name **two** fixes *other than* HMAC for a secret-prefix MAC (e.g. `H(secret || H(secret || m))`, truncate the output, switch to SHA-3 / KMAC, or CMAC). For each, explain the mechanism by which it removes the length-extension attack.
- **Q11 (4 pts):** SHA-256 (Merkle–Damgård) is length-extendable; SHA3-256 (sponge) and KMAC are not. Explain the structural reason — what does the sponge construction withhold from the output that Merkle–Damgård exposes? (This is the security-relevant consequence hinted at in Part 1.)

---

## Part 4: Password Hashing & Key-Derivation Functions — 24 points

**Context.** Password storage is a hashing problem where the "obvious" choice (`SHA256(password)`) is wrong for the *opposite* reason MD5 is wrong: SHA-256 is **too fast**. `tools/password_kdf_bench.py` demonstrates salting and work factors; CyberChef's **Bcrypt** operation shows a purpose-built password hash.

### Steps

1. Read `tools/password_kdf_bench.py`.
2. Run: `python3 tools/password_kdf_bench.py --passphrase "<PASS>"`

   It prints:
   - `SHA256(PASS)` with **no salt** (deterministic — identical for every user with this password),
   - `SHA256(salt || PASS)` for **two different salts** (two different stored values, same password),
   - a timing table for **PBKDF2-HMAC-SHA256** at 10 000 / 100 000 / 600 000 iterations,
   - a timing row for **scrypt**,
   - an estimated single-GPU offline guessing rate for each setting.
3. **CyberChef.** Add the **Bcrypt** operation, rounds = 10. Input: your `PASS`. Bake. Note the output. **Bake again.** The hash changes — bcrypt generated a fresh random salt, embedded in the output string. Record both outputs.

### Deliverables (8 pts)

- **SS10 (3 pts):** console output showing `PASS` producing one deterministic unsalted digest and **two different** salted digests — the two differing salted digests highlighted yellow.
- **SS11 (3 pts):** console output of the PBKDF2 / scrypt timing table — the per-setting time and estimated guessing rate boxed red.
- **SS12 (2 pts):** CyberChef Bcrypt run twice on the same `PASS` producing two different hash strings — both outputs highlighted, the differing salt portion boxed.

### Analysis (16 pts)

- **Q12 (4 pts):** Both `SHA256(salt || pw)` and PBKDF2 use a salt. What specific attack does the **salt** defeat, and — separately — what specific attack does the **iteration count** defeat? They are not the same attack.
- **Q13 (4 pts):** From your timing table, choose an iteration count that keeps one server-side verification under ~250 ms. At that setting, by what factor has an offline attacker's guess rate dropped versus raw `SHA256`? Show the arithmetic with your measured numbers.
- **Q14 (4 pts):** scrypt and Argon2 add a large *memory* requirement on top of slowness. Why does that specifically hurt a GPU or ASIC cracking rig, when a raw iteration count alone does not?
- **Q15 (4 pts):** A deliberately slow password hash introduces an availability risk. Describe the denial-of-service, and name the standard mitigation that lets you keep the slow hash.

---

## References & Further Reading

Lab-wise, if you face any difficulties with setup, tool usage, or markup, reach out to the TAs during office hours or by email. Assessments and deductions are at TA discretion but follow the rubric for fairness and consistency. Raise any grading concerns within three days of receiving your points.

These cover the *concepts*; none walk through this lab's specific parameters or give you the answers.

1. **Computerphile:** [SHA: Secure Hashing Algorithm](https://www.youtube.com/watch?v=DMtFhACPnTY) and [Hashing Algorithms and Security](https://www.youtube.com/watch?v=b4b8ktEV4Bg).
2. **Length extension:** [skullsecurity.org — "Everything you need to know about hash length extension attacks"](https://blog.skullsecurity.org/2012/everything-you-need-to-know-about-hash-length-extension-attacks).
3. **Collisions:** [shattered.io](https://shattered.io) (SHA-1); the [MD5 Wikipedia article](https://en.wikipedia.org/wiki/MD5#Collision_vulnerabilities) for the Part 2 block pair.
4. **HMAC:** Bellare, Canetti, Krawczyk, *Keying Hash Functions for Message Authentication* (1996); RFC 2104.
5. **Password storage:** [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html).

## AI Appendix & submission format

Follow `../guidelines.txt` (screenshot markup, `FirstName_LastName_LabHM.docx` naming, AI-use disclosure). Not restated here. The missing-screenshot cap in `../SCREENSHOT_PENALTY_POLICY.md` applies on top of the rubric.
