# **LAB 02 (VARIANT A) – THE ART HEIST**

**Topic:** ECB vs CBC Modes (Visual Analysis).

**Story Context:**
> You are a Digital Art Curator. You are sending an encrypted scan of a priceless painting (Your ID).
> If you use ECB mode, the thief can still see the *shape* of the painting.
> You must demonstrate this weakness.

**Tools Required:** [BlockRefinery.html](https://uncc-fortress.github.io/itis-6200-f26/Lab02_BlockCiphers/tools/BlockRefinery.html) (Tab: Visual).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Image:** Draw your **ID Number** in Paint (Black text, White BG). Save as BMP/PNG.
2.  **Key:** `[LastName]Key123`.

---

## **Part 2: The Encryption**

1.  **Action:**
    -   Open Tool -> Visual Mode.
    -   Upload Image.
    -   Encrypt with **ECB**. Save Result.
    -   Encrypt with **CBC**. Save Result.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab02A.docx`

### **Screenshot 1: ECB Result**
-   **Show:** The encrypted image showing the ghost of your ID numbers.
-   **Markup:** **Red Circle** around the visible numbers.

### **Screenshot 2: CBC Result**
-   **Show:** The encrypted image appearing as Static/Noise.
-   **Markup:** **Green Check** indicating no patterns.

### **Part 4: Analysis (Homework Integration)**

1.  **Mechanics:** Why does the Tux/Penguin (or your ID) remain visible in ECB? Use the phrase "Deterministic Block encipherment".

### **Part 5: References & Further Reading**

1.  **The Famous Penguin:** [ECB vs CBC Mode Visualized](https://blog.filippo.io/the-ecb-penguin/)
    *   *Why you should never use ECB mode for images.*
2.  **Computerphile:** [AES Explained (Advanced Encryption Standard)](https://www.youtube.com/watch?v=O4xNJsjtN6E)
3.  **Diagram:**
```merm
graph TD
   subgraph ECB Mode
   B1[Block 1] --> E1[Encrypt] --> C1[Cipher 1]
   B2[Block 1] --> E2[Encrypt] --> C2[Cipher 1]
   end
   subgraph CBC Mode
   BB1[Block 1] --> X1((XOR)) --> EE1[Encrypt] --> CC1[Cipher 1]
   CC1 --> X2
   BB2[Block 2] --> X2((XOR)) --> EE2[Encrypt] --> CC2[Cipher 2]
   end
   style C1 fill:#f9f,stroke:#333
   style C2 fill:#f9f,stroke:#333
   style CC2 fill:#bbf,stroke:#333
```


