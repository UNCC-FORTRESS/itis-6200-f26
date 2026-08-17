# **LAB 04 (VARIANT B) – THE SLOT MACHINE HACK**

**Topic:** PRNG Prediction (LCG).

**Story Context:**
> Slot machines use math to generate "Random" numbers.
> If the math is $Next = (A \times Prev + C) \mod M$, and you know the parameters, you can win every time.

**Tools Required:** `RandomLab.html` (Tab: LCG).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Modulus (M):** 100.
2.  **Multiplier (A):** Last digit ID + 3.

---

## **Part 2: The Prediction**

1.  **Action:**
    -   Generate a sequence.
    -   Identify the "Period" (Repeat length).
    -   Predict the next number after a specific observation.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab04B.docx`

### **Screenshot 1: The Sequence**
-   **Show:** Example sequence e.g., `0 -> 7 -> 21...`
-   **Markup:** **Red Arrow** pointing to the start of the repeat.

### **Part 4: Analysis (Homework Integration)**

1.  **Theory:** Is a sequence with a period of $2^{64}$ "Random"? Why or why not? (Hint: It passes statistical tests, but is it unpredictable?).
2.  **Math:** Solve for $A$ if you know:
    *   $X_1 = 3$
    *   $X_2 = 7$
    *   $X_3 = 11$
    *   $M = 100, C = 0$

### **Part 5: References & Further Reading**

1.  **Concept:** [Linear Congruential Generators](https://en.wikipedia.org/wiki/Linear_congruential_generator)
    *   *Understand the math: $X_{n+1} = (aX_n + c) \mod m$.*
2.  **Risk:** [Modulo Bias (Wikipedia)](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle#Modulo_bias)
    *   *Why simple math isn't secure.*


