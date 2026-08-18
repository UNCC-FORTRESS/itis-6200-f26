# **LAB 01 (VARIANT C) – THE ZODIAC PUZZLE**

**Topic:** Monoalphabetic Substitution & Brute Force Resistance.

**Story Context:**
> You are a Detective solving a serial killer's puzzle.
> He didn't just shift the alphabet; he mixed it up completely. `A` -> `Q`, `B` -> `F`, etc.
> This creates a massive number of possibilities.

**Tools Required:** [FrequencyAnalyzer.html](https://uncc-fortress.github.io/itis-6200-f26/Lab01_Cryptography/tools/FrequencyAnalyzer.html) (Tab: Substitution).

*(Cross-check option: [CyberChef](https://uncc-fortress.github.io/CyberChef/) (backup: [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/)) -- operation **Substitute**, using your scrambled alphabet.)*

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Plaintext:** `[YourFullName] is learning security`
2.  **Key (Alphabet):** A scrambled alphabet starting with your initials. (e.g., `AB...`).

---

## **Part 2: The Encryption**

1.  **Action:**
    -   Open Tool -> Tab "Substitution".
    -   Enter Plaintext.
    -   Generate a Random Key (or type one).
    -   Record Ciphertext.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab01C.docx`

### **Screenshot 1: The Setup**
-   **Show:** Tool with your unique input and output.
-   **Markup:** **Red Box** around the Substitution Key.

### **Screenshot 2: Frequency**
-   **Show:** The Frequency Chart.
-   **Markup:** **Green Arrow** showing that the *peaks* are still there (just moved).

### **Part 4: Analysis (Homework Integration)**

1.  **Keyspace:** The keyspace is $26!$ (Factorial). Calculate this value (approximate in scientific notation). Why makes this secure against Brute Force but insecure against Frequency Analysis?

### **Part 5: References & Further Reading**

1.  **Article:** [Frequency Analysis Guide](https://crypto.interactive-maths.com/frequency-analysis-breaking-the-code.html)
    *   *How to use letter frequency (E, T, A) to break substitution ciphers.*
2.  **The Zodiac Killer:** [Z340 Solution](https://blog.wolfram.com/2021/03/24/the-solution-of-the-zodiac-killers-340-character-cipher/)
    *   *Real-world example of a complex substitution cipher solved by community computing.*


