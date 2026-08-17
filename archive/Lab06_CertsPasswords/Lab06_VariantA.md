# **LAB 06 (VARIANT A) – THE FAKE BANK**

**Topic:** Public Key Infrastructure (PKI) & Digital Certificates.

**Story Context:**
> You are browsing `Bank.com`.
> An attacker intercepts your connection and presents a specific certificate.
> If the certificate is signed by a "Trusted Root", your browser accepts it.
> If the attacker generates a "Self-Signed" cert, your browser warns you.

**Tools Required:** `CertLab.html` (Tab: Certificates).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Subject:** `[YourName]`.
2.  **Issuer:** `RootCA_[ID]`.

---

## **Part 2: The Validation**

1.  **Action:**
    -   Generate Root CA.
    -   Issue Valid Cert (Signed by Root). -> Browser: **Trusted**.
    -   Issue Fake Cert (Self-Signed by Attacker). -> Browser: **Untrusted**.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab06A.docx`

### **Screenshot 1: The Trust**
-   **Show:** "Chain Validated".
-   **Markup:** **Green Chain** icon.

### **Part 4: Analysis (Homework Integration)**

1.  **Trust Anchor:** Where does your operating system store the "Root CAs"? (e.g., Windows Certificate Store). How do they get there?

### **Part 5: References & Further Reading**

1.  **Case Study:** [DigiNotar Hack (Wikipedia)](https://en.wikipedia.org/wiki/DigiNotar)
    *   *Real-world consequence of a compromised Root CA.*
2.  **Visual:**
```merm
graph TD
    Root[Root CA - Trusted] -->|Signs| Intermediate[Intermediate CA]
    Intermediate -->|Signs| Leaf[Google.com]
    Browser -->|Trusts| Root
    Browser -->|Validates| Leaf
```
3.  **Concept:** [Chain of Trust](https://knowledge.digicert.com/generalinformation/INFO1745.html)


