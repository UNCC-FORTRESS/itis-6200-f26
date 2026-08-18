# **LAB 00 – THE PROTOCOLS**

**Topic:** Understanding Student Identity Parameters (SIP) and Submission Guidelines.

**Objective:** This "Dummy Lab" counts for 1% of your grade. Its sole purpose is to ensure you can generate your unique parameters and format your submission correctly. Failure to master this lab will result in 0s on future labs.

---

## **Part 1: Generating Your SIP (Student Identity Parameters)**

Every week, your keys, inputs, and targets will depend on **WHO YOU ARE**. You cannot copy a friend's lab because their math will be different.

**Your Identity Vector:**
1.  **First Name ($N_{first}$):** Your official first name as registered.
2.  **Student ID ($ID$):** Your 9-digit ID (starts with 800/801).
3.  **Favorite Color ($C_{fav}$):** A string of your choice.

**The Derivation:**
For this lab, we will calculate a simple "Session Key".

$$ K_{session} = (Sum(Digits(ID)) + Length(N_{first})) \mod 10 $$

*Example:* 
-   **Student:** Jian Xiang
-   **ID:** 801234567
-   *Sum(Digits)* = $8+0+1+2+3+4+5+6+7 = 36$
-   *Length("Jian")* = 4
-   **Total:** $36 + 4 = 40$
-   **$K_{session}$:** $40 \mod 10 = 0$

---

## **Part 2: The Experiment**

### **Task A: The Deterministic String**
1.  Open a Python shell or text editor.
2.  Create a string in the following format:
    `[ID]-[N_first]-[K_session]`
3.  Example (for Jian): `801234567-Jian-0`

### **Task B: The Entropy Injection**
1.  Take your string from Task A.
2.  Append your $C_{fav}$ to it.
3.  Calculate the **MD5 Hash** of this final string using [CyberChef](https://uncc-fortress.github.io/CyberChef/) (backup: [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/)) or Python.
    -   *CyberChef Recipe:* `MD5`
    -   *Input:* `801234567-Jian-0Blue` (If Blue is the color)

---

## **Part 3: Deliverables & Evidence**

**Submission File:** `FirstName_LastName_Lab00.docx`

### **Screenshot 1: The Hash Generation**
-   **Context:** A full desktop screenshot showing you generating the hash.
-   **Markup Required:**
    -   **Red Box:** Around your Input String (proving it's you).
    -   **Yellow Highlight:** Around the first 4 characters of the Output Hash.
-   **Constraint:** The Taskbar/Menu Bar must be visible with the current time.

---

## **Part 4: Analysis**

1.  **Collision Check:** If you change **one letter** of your favorite color (e.g., "Blue" to "Glue"), how many characters in the MD5 output change? All of them, or just a few?
2.  **Identity:** Why do we ask for your specific ID in the input? (Hint: Non-repudiation / Anti-plagiarism).

## **References & Further Reading**

Lab-wise, if you face any difficulties regarding the setup, tool usage, or markup help, please feel free to reach out to the TAs during office hours or via email. All assessments and deductions are done at the discretion of the TAs, but mostly based off of a preset rubric to ensure fairness and consistency — if you have any reservations with how your submission was graded, please raise them within three days of receiving your points.

These resources explain the underlying idea (the avalanche effect) this lab asks you to observe — they won't tell you what your own hash output should look like, since that depends on your own SIP input.

1.  **Ref:** [Avalanche Effect (Wikipedia)](https://en.wikipedia.org/wiki/Avalanche_effect) — why a one-character input change should scramble a cryptographic hash's output almost completely.

