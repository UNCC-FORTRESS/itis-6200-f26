# **LAB 06 (VARIANT B) – THE RAINBOW CRACK**

**Topic:** Password Cracking (Time-Memory Tradeoff).

**Story Context:**
> You found a database leak. Passwords are hashed with MD5 (no salt).
> Instead of brute-forcing every hash (Time expensive), you buy a "Rainbow Table" (Pre-computed hashes).
> Lookup is $O(1)$ (Instant).

**Tools Required:** `CrackLab.html` (Tab: Rainbow).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Password:** `pass[ID_LastDigit]`.

---

## **Part 2: The Lookup**

1.  **Action:**
    -   Hash your password.
    -   Use "Rainbow Table Lookup".
    -   Result: Password found instantly.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab06B.docx`

### **Screenshot 1: The Crack**
-   **Show:** "Password Found: passX".
-   **Markup:** **Red Box** around the recovered plaintext.

### **Part 4: Analysis (Homework Integration)**

1.  **Tradeoff:** Rainbow tables trade **Disk Space** for **Time**. Explain.

### **Part 5: References & Further Reading**

1.  **Video:** [Rainbow Tables (Computerphile)](https://www.youtube.com/watch?v=eG66uzPTz2s)
2.  **Resource:** [Rainbow Table (Wikipedia)](https://en.wikipedia.org/wiki/Rainbow_table)
    *   *How precomputed lookup tables trade storage for cracking speed (concept only — don't run cracking tools on school PCs).*


