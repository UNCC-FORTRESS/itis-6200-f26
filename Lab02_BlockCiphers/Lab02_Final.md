# **LAB 02: Block Cipher Modes of Operation**

**Topic:** ECB, CBC, CTR, and the importance of IVs/Nonces.

**Goal:** Understand how different "Modes of Operation" using the same underlying cipher (e.g., AES) can result in vastly different security properties.

**Tools Required:** [BlockCipherModes.html](https://uncc-fortress.github.io/itis-6200-f26/Lab02_BlockCiphers/tools/BlockCipherModes.html)

---

## **Step 1: Visual Patterns (The ECB Problem)**

**Context:**
> Electronic Codebook (ECB) is the simplest mode. It breaks the message into blocks and encrypts each independently with the same key. `Enc(Block1)` always equals `Cipher1`.

### **1.1 The Experiment**

1. Open [BlockCipherModes.html](https://uncc-fortress.github.io/itis-6200-f26/Lab02_BlockCiphers/tools/BlockCipherModes.html) (Tab 1: Visual Patterns).
2. **Upload an Image** (Ideally one with large patches of single colors, like a logo or cartoon).
3. Select **Mode: ECB** and click **Encrypt**.
4. Observe the result. Can you still "see" the image?
5. Switch to **Mode: CBC** or **CTR**.
6. Click **Encrypt** again.
7. Observe the result. Is it indistinguishable from random noise?

### **1.2 Analysis**

* **Q1:** Explain why ECB preserves the visual patterns of the image. What does this tell you about encrypting repetitive data (like salary spreadsheets or headers) with ECB?
* **Q2:** Why does CBC (Cipher Block Chaining) fix this problem? (Hint: Look at the diagram logic in the tool if unsure).

---

## **Step 2: Error Propagation (CBC vs CTR)**

**Context:**
> What happens if a bit gets flipped during transmission? Does the whole message break, or just a part of it? This property is called "Error Propagation".

### **2.1 The Experiment**

1. Switch to **Tab 2: Error Propagation**.
2. **Mode: ECB**.
    * Encrypt. Flip a bit. **Decrypt**.
    * Observe: Only the block corresponding to the flipped bit is garbled.
3. **Mode: CBC (Self-Synchronizing)**.
    * Encrypt. Flip a bit in the **First Block**.
    * **Decrypt**. Observe:
        * Block N (flipped): Completely Garbled.
        * Block N+1: **ONE** single specific error.
        * Block N+2 onwards: **Perfectly Fine**. (This is called Self-Synchronization).
4. **Mode: PCBC (Propagating CBC)**.
    * Encrypt. Flip a bit in the First Block.
    * **Decrypt**. Observe:
        * **EVERYTHING** from that point onward is garbage. The error propagates infinitely!
        * This seems "safer" (integrity-wise) but is terrible for unreliable networks.
5. **Mode: CTR**.
    * Encrypt. Flip a bit.
    * **Decrypt**. Observe: Only **ONE** bit is flipped in plaintext.

### **2.2 Analysis**

* **Q3 (The Streaming Question):** If you are streaming 4K video over a noisy wifi connection where bits often flip, which mode is best?
  * **CTR:** Glitch affects 1 pixel.
  * **CBC:** Glitch ruins 1 block (16 pixels) + 1 pixel slightly wrong.
  * **PCBC:** Glitch ruins the **ENTIRE REST OF THE MOVIE**.
* **Q4 (Integrity vs Reliability):** Why is standard CBC preferred over PCBC for most applications, even though PCBC makes tampering more obvious? (Hint: Think about the cost of re-transmitting data if 1 bit flips).

---

## **Step 3: The Two-Time Pad (CTR Key Reuse)**

**Context:**
> Counter Mode (CTR) generates a "Keystream" and XORs it with the plaintext.
> **Critical Rule:** NEVER reuse the same Key + Nonce (Counter).
> If $C_1 = M_1 \oplus K$ and $C_2 = M_2 \oplus K$, then we can derive $K = C_1 \oplus M_1$. If we guess $M_1$ correctly, we steal the Key!

### **3.1 The "Known Plaintext" Attack**

1. Switch to **Tab 3: CTR Key Reuse**.
2. **Setup (Alice & Bob):**
    * Observe the two messages ($M_1, M_2$) and their Keys ($K_1, K_2$).
    * Currently, **Key 1 matches Key 2** (12345).
3. **The Scenario:**
    * You (the Attacker) have intercepted `C1` and `C2`.
    * You also **know the content of Message 1** (e.g., standard header, or you tricked Alice into sending it).
4. **The Automatic Calculation:**
    * The tool uses your knowledge of $M_1$ to calculate: $Recovered\_M2 = (C_1 \oplus C_2) \oplus M_1$.
    * Look at the **"Recovered Message 2"** box. It should perfectly match the secret $M_2$!

### **3.2 Analysis & Defense**

* **Task 1 (Attack):**
  * Change "Message 2" in the Setup to a secret phrase (e.g., "Launch Nuke").
  * Look at the "Attacker's Workspace".
  * Did the tool immediately recover "Launch Nuke"? (Yes, because the math $S \oplus M_1$ always isolates $M_2$ if keys are reused).
* **Task 2 (Defense):**
  * Change **Key 2** to `99999` (so $K_1 \neq K_2$).
  * Look at the "Recovered Message 2" box.
  * What do you see now? Explain why the attack failed. (Hint: The equation is now $M_2 \oplus K_1 \oplus K_2$).

* **Q5:** Why is reusing a Stream Cipher key strictly forbidden?
* **Q6:** If you encrypt 1000 messages with the same Key+Nonce, and the attacker discovers the plaintext of **one** message, what happens to the other 999?

*(Optional supplementary desktop tool covering this same IV/key-reuse idea: [aesVisual.py](https://raw.githubusercontent.com/UNCC-FORTRESS/itis-6200-f26/main/Lab02_BlockCiphers/tools/aesVisual.py) — no pre-built executable is provided, run it directly with `pip install customtkinter cryptography` then `python aesVisual.py`. Not required for this lab.)*

---

## **Step 4: Deliverables**

**Submission File:** `FirstName_LastName_Lab02_Final.docx`

**Include:**

1. **Screenshots:**
    * Step 1: Side-by-side comparison of ECB (Pattern Visible) vs CBC (Noise).
    * Step 2: Screenshot of CBC Decryption showing "Garbage Block + 1 Bit Error".
    * Step 3: Screenshot of the "Attacker's View" showing the recovered Keystream and M2.
2. **Answers:** Responses to Questions Q1 through Q6.

---

## **References**

* [NIST SP 800-38A — Recommendation for Block Cipher Modes of Operation](https://csrc.nist.gov/pubs/sp/800/38/a/final)
* [Block Cipher Modes Visualization (Wikipedia)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation)
