# **LAB 11 (VARIANT C) – THE SMUGGLER**

**Topic:** IDS Evasion.

**Story Context:**
> Sneaking a gun past a detector by disassembling it.

**Tools Required:** `IDSLab.html` (Tab: Evasion).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Payload:** `ATTACK`.

---

## **Part 2: The Smuggle**

1.  **Action:**
    -   Send Standard (Blocked).
    -   Send Fragmented (Allowed).
    -   Server executes payload.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab11C.docx`

### **Screenshot 1: Execution**
-   **Show:** "Server Reassembly: ATTACK".
-   **Markup:** **Green Check**.

### **Part 4: Analysis (Homework Integration)**

1.  **Reassembly:** Why is reassembly hard for the IDS? (It must buffer packets for thousands of potential connections until they complete, which eats RAM).

### **Part 5: References & Further Reading**

1.  **Paper:** [Insertion, Evasion, and Denial of Service (Ptacek & Newsham)](https://insecure.org/stf/secnet_ids/secnet_ids.html)
    *   *The seminal paper (1998) that proved signature IDS can be evaded, including via packet fragmentation — see its evasion/insertion sections for the fragmentation-specific techniques.*


