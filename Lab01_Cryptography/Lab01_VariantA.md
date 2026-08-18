# **LAB 01 (VARIANT A) – THE ROMAN MESSENGER**

**Topic:** Caesar Cipher (Shift Cipher) & Moral Strength.  
**Visual Concept:**  
```merm
graph LR
    A[Plain: HELLO] -->|Shift +1| B(Cipher: IFMMP)
    B -->|Shift -1| A
```


**Story Context:**
> You are a messenger for Caesar. You must carry a message through enemy territory.
> The enemy can read Latin, so you must **Shift** the letters.
> If captured, you must NOT reveal the key (Shift Amount), even if they bribe you.
> Your Key is determined by your "Legion ID" (Student ID).

**Tools Required:** [FrequencyAnalyzer.html](https://uncc-fortress.github.io/itis-6200-f26/Lab01_Cryptography/tools/FrequencyAnalyzer.html) (Tab: Caesar).

*(Cross-check option: [CyberChef](https://uncc-fortress.github.io/CyberChef/) (backup: [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/)) -- operation **ROT13**, set "Amount" to your shift value.)*

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Plaintext:** `[YourFullName] is learning security`
2.  **Key (Shift):** Last Digit of your Student ID (If 0, use 10).

---

## **Part 2: The Encryption**

1.  **Action:**
    -   Open Tool -> Tab "Caesar".
    -   Enter your **Plaintext** and **Shift**.
    -   Record the Ciphertext.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab01A.docx`

### **Screenshot 1: The Setup**
-   **Show:** Tool with your unique input and output.
-   **Markup:** **Red Box** around the Shift Value.

### **Screenshot 2: Frequency**
-   **Show:** The Frequency Chart of your Ciphertext.
-   **Markup:** **Green Arrow** pointing to the most frequent letter (Likely 'E' shifted).

### **Part 4: Analysis (Homework Integration)**

1.  **Keyspace:** The Caesar Cipher has a keyspace of 26. Explain why checking 26 keys is considered "insecure" even for a human. How many keys per minute could you check manually?

### **Part 5: References & Further Reading**

1.  **Khan Academy:** [Journey into Cryptography](https://www.khanacademy.org/computing/computer-science/cryptography)
    *   *Excellent interactive history of Caesar, Vigenère, and Frequency Analysis.*
2.  **Video:** [Cracking the Code](https://youtu.be/T59hl2nlrT0) (Simons Singh)
    *   *From the author of "The Code Book", explaining the race between codemakers and codebreakers.*
3.  **Tool:** [dCode.fr](https://www.dcode.fr/caesar-cipher)
    *   *A powerful online tool for checking your work (after you do it manually!).*


