# **LAB 02 (VARIANT C) – THE SUBMARINE GLITCH**

**Topic:** Error Propagation (ECB vs CBC).

**Story Context:**
> You are communicating with a submarine. The channel is noisy (bit flips happen).
> You need to know: If ONE bit gets corrupted in the ciphertext, how much of the message is destroyed upon decryption?

**Tools Required:** [BlockRefinery.html](https://uncc-fortress.github.io/itis-6200-f26/Lab02_BlockCiphers/tools/BlockRefinery.html) (Tab: Error Prop).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Message:** `Coordinates: [YourID] North`.
2.  **Error:** Flip the 5th bit of the 1st block.

---

## **Part 2: The Glitch**

1.  **Action:**
    -   Encrypt message with **ECB**. Modify 1 bit. Decrypt.
    -   Encrypt message with **CBC**. Modify 1 bit. Decrypt.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab02C.docx`

### **Screenshot 1: ECB Damage**
-   **Show:** Decryption.
-   **Markup:** **Red Box** around the *single* corrupted block.

### **Screenshot 2: CBC Damage**
-   **Show:** Decryption.
-   **Markup:** **Red Box** around the *two* corrupted blocks (One garbled, one with bit flip).

### **Part 4: Analysis (Homework Integration)**

1.  **Avalanche:** In CBC, why does an error in C1 affect P2? (Look at the decryption formula $P_i = D(C_i) \oplus C_{i-1}$).

### **Part 5: References & Further Reading**

1.  **Standard:** [NIST SP 800-38A](https://csrc.nist.gov/publications/detail/sp/800-38a/final)
    *   *Recommendation for Block Cipher Modes of Operation (ECB, CBC, OFB, CFB, CTR).*
2.  **Concept:** [Bit Flipping Attacks](https://www.youtube.com/watch?v=VR-TuXXi3A8)
    *   *How changing 1 bit in ciphertext changes the plaintext.*


