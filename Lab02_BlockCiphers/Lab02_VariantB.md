# **LAB 02 (VARIANT B) – THE COOKIE FORGERY**

**Topic:** ECB Cut-and-Paste Attack.

**Story Context:**
> You are a hacker targeting a website that uses ECB to encrypt cookies.
> Cookie Format: `user=[Name];role=user`.
> You want to create a valid cookie for: `user=[Name];role=admin`.
> Since ECB encrypts blocks independently, you can "Cut" the encrypted `admin` block from one cookie and "Paste" it into yours.

**Tools Required:** `BlockRefinery.html` (Tab: Forge).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Username:** `[LastName]`.
2.  **Target:** `admin`.

---

## **Part 2: The Forge**

1.  **Action:**
    -   Follow tool instructions to generate a block containing `admin` in the right position.
    -   Splice it onto your username block.
    -   Decrypt to verify.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab02B.docx`

### **Screenshot 1: The Splicing**
-   **Show:** The Tool showing the Hex blocks being rearranged.
-   **Markup:** **Red Arrow** moving the Admin block.

### **Screenshot 2: Success**
-   **Show:** Decrypted result saying `role=admin`.
-   **Markup:** **Green Box** around the role.

### **Part 4: Analysis (Homework Integration)**

1.  **Padding:** PKCS#7 Padding is essential. If your name is 5 bytes (`ABCDE`), how many padding bytes are needed to fill a 16-byte block? What is the hex value of those bytes?

### **Part 5: References & Further Reading**

1.  **Attack:** [ECB Cut and Paste](https://cryptopals.com/sets/2/challenges/13)
    *   *The inspiration for this lab (Cryptopals Challenge 13).*
2.  **Article:** [The ECB Penguin — why ECB is bad](https://blog.filippo.io/the-ecb-penguin/)


