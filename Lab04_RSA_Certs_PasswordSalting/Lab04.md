# Lab 04: RSA, Digital Signatures & Hybrid Encryption, Certificates, and Password Salting

Total: 100 points across 4 steps. Tool required throughout Steps 1-2: **[CyberChef](https://uncc-fortress.github.io/CyberChef/)** (backup: [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/)).

## Step 1: RSA Key Pairs and Encryption (24 points)

RSA is asymmetric: a Public Key (shareable) encrypts, a Private Key (secret) decrypts.

1. **Generate Alice's keys.** In CyberChef, use "Generate RSA Key Pair," 1024-bit, PEM format. Bake. You now have Alice's public and private key.
2. **Bob encrypts a message.** New CyberChef tab: "RSA Encrypt," paste Alice's public key as the key, mode RSA-OAEP, digest SHA-1, then "To binary." Message: `"Can you send me <your student ID> dollars for V-Bucks"`. The output is the ciphertext.
3. **Alice decrypts.** New tab: "From binary" then "RSA Decrypt," paste the ciphertext and Alice's private key.

**Deliverables:** (2 pts) Alice's key pair screenshot. (2 pts) Bob's window, message highlighted yellow, ciphertext boxed red. (2 pts) Alice's window, ciphertext highlighted yellow, plaintext boxed red.

**Analysis:**
- Q1 (6 pts): why is it safe to post the Public Key publicly but catastrophic to lose the Private Key? What's the mathematical relationship between them?
- Q2 (6 pts): Bob's message is confidential (only Alice can read it) — but can Alice confirm it actually came from Bob and not an impostor? Why/why not?
- Q3 (6 pts): why include a hash (message digest) in RSA encryption/signing rather than operating on the raw message directly?

## Step 2: RSA Digital Signatures & Hybrid Encryption (30 points)

Step 1 gave confidentiality only. Now add authenticity: Bob AES-encrypts a message, then signs it with his own RSA private key.

1. **Bob generates his own RSA key pair** (1024-bit, PEM), posts the public key.
2. **Bob AES-encrypts.** "AES Encrypt," CBC mode, raw input, hex output, key = IV = your Student ID (repeated to meet the length requirement). Message: `"Thank you for sending me <your student ID> dollars, I have a lot of Fortnite skins now!"`. Save the resulting ciphertext.
3. **Bob signs the ciphertext.** Add "RSA Sign" then "To binary" after the AES step; key = Bob's private key; digest SHA-256. Output is the signature.
4. **Alice verifies.** New tab: "From binary" then "RSA Verify"; key = Bob's public key, signature input = the signature from step 3, message input = the ciphertext from step 2. A valid result shows "Verified: true".

**Deliverables:** (2 pts) Bob's key pair. (2 pts) Bob's message highlighted yellow + signature boxed red. (2 pts) Alice's signature and ciphertext inputs highlighted yellow + verification result boxed red.

**Analysis:**
- Q4 (8 pts): why does Bob sign with his *Private* key rather than his Public key? Would signing with the Public key still prove authorship?
- Q5 (8 pts): Alice got Bob's public key from "an online forum" — can she actually prove it's really Bob's key and not Mallory's? Why/why not? (This sets up Step 3.)
- Q6 (8 pts): this lab uses **hybrid encryption** — AES for the message, RSA only for the signature. Why not just use RSA to encrypt the whole message and sign it directly? (Hint: computational efficiency.)

## Step 3: Certificates (20 points)

Conceptual, referencing Lecture 7's Certificate Authorities content. Scenario: Alice receives a certificate for "Mallory," signed by "Bob," and must follow a chain of trust back to a root CA ("BC").

- Q7 (7 pts): list the specific chain of certificates Alice must verify to trust Mallory's key.
- Q8 (7 pts): if Bob's intermediate certificate was signed using an algorithm now considered broken, does Alice still trust Mallory? Why/why not?
- Q9 (6 pts): "You cannot gain trust if you trust nothing — you need a root of trust." In a real browser, who are the trust anchors, and how do they get onto your computer? What happens if you manually delete all Root CA certificates and then visit a site like Google or Canvas?

*(Optional supplementary interactive practice for this section: [CertAuthorityTool.html](https://uncc-fortress.github.io/itis-6200-f26/archive/Lab06_CertsPasswords/tools/CertAuthorityTool.html) and [CertLab.html](https://uncc-fortress.github.io/itis-6200-f26/archive/Lab06_CertsPasswords/tools/CertLab.html) — built but never deployed as their own lab, still useful here as hands-on reinforcement of the chain-of-trust concept.)*

## Step 4: The Exponential Wall — Password Salting Simulation (26 points)

1. Open the provided Python/Colab notebook: **https://colab.research.google.com/drive/1C4E4JwCuJ_W6-4olBm0GH2RA_fGLITMd?usp=sharing**.
2. Select "Open in Colab" if prompted, read through the code, then run it.
3. Observe the brute-force time difference between the unsalted and salted password databases (the salted attack may take 1-3 minutes — that delay *is* the security benefit in action).

**Deliverable** (2 pts): full screenshot of the output console, red box around the elapsed time and extra-hashes-computed count for the salted database.

**Analysis:**
- Q10 (8 pts): salts are stored in plaintext next to the username — an attacker who steals the database can see them. If salts are visible, how do they still defeat a pre-computed rainbow-table attack?
- Q11 (8 pts): a slow hash defends against *offline* attacks — but explain the new vulnerability it creates for *online* attacks. If verifying one password costs the server 0.5 CPU-seconds, how could an attacker weaponize that to crash the login portal?
- Q12 (8 pts): why do attackers use GPUs rather than CPUs for password cracking specifically? How does salting reduce a GPU cracking rig's efficiency?

*(Optional supplementary interactive practice: [CrackLab.html](https://uncc-fortress.github.io/itis-6200-f26/archive/Lab06_CertsPasswords/tools/CrackLab.html) and [HashCracker.html](https://uncc-fortress.github.io/itis-6200-f26/archive/Lab06_CertsPasswords/tools/HashCracker.html).)*

## References & Further Reading

Lab-wise, if you face any difficulties regarding the setup, tool usage, or markup help, please feel free to reach out to the TAs during office hours or via email. All assessments and deductions are done at the discretion of the TAs, but mostly based off of a preset rubric to ensure fairness and consistency — if you have any reservations with how your submission was graded, please raise them within three days of receiving your points.

These explain the underlying concepts across all four steps — none of them are CyberChef walkthroughs for this specific lab.

1.  **Computerphile:** [Public Key Cryptography](https://www.youtube.com/watch?v=GSIDS_lvRv4) — the RSA/asymmetric-encryption ideas behind Steps 1-2.
2.  **Ref:** [Chain of Trust (Wikipedia)](https://en.wikipedia.org/wiki/Chain_of_trust) — background for Step 3's certificate questions.
3.  **OWASP:** [Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) — background for Step 4's salting/hashing questions.

## AI Appendix & submission format

Follow `../guidelines.txt` — not restated here.
