# **LAB 05 (VARIANT C) – THE AUCTION HACK**

**Topic:** RSA Malleability (Homomorphic Property).

**Story Context:**
> An auction house encrypts bids using RSA. `EncBid = Bid^e mod N`.
> You see a competitor's encrypted bid $C$. You don't know the value.
> But you want to bid *Double*.
> Because RSA is "Malleable", you can calculate $Enc(2 \times Bid)$ without knowing $Bid$!
> Formula: $C_{new} = C \times 2^e \mod N$.

**Tools Required:** `RSALab.html` (Tab: Malleability).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Competitor Bid:** 10.
2.  **Goal:** 20.

---

## **Part 2: The Attack**

1.  **Action:**
    -   Intercept Encrypted Bid (10).
    -   Create "Modifier" (Encrypt 2).
    -   Multiply them.
    -   Decrypt result.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab05C.docx`

### **Screenshot 1: The Win**
-   **Show:** Decrypted Bid = 20.
-   **Markup:** **Green Box**.

### **Part 4: Analysis (Homework Integration)**

1.  **Proof:** Prove that $E(m_1) \times E(m_2) = E(m_1 \times m_2)$ for RSA. ($m_1^e \times m_2^e = (m_1 m_2)^e$).

### **Part 5: References & Further Reading**

1.  **Concept:** [Homomorphic Encryption (Wikipedia)](https://en.wikipedia.org/wiki/Homomorphic_encryption)
    *   *Explanation of the multiplicative property $E(x)E(y) = E(xy)$.*
2.  **Defense:** [OAEP Padding (Wikipedia)](https://en.wikipedia.org/wiki/Optimal_asymmetric_encryption_padding)
    *   *How padding breaks the malleability.*


