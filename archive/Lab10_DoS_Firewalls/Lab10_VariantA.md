# **LAB 10 (VARIANT A) – THE FLOOD**

**Topic:** DoS.

**Story Context:**
> Bandwidth is finite. If you fill the pipe with junk, legitimate cars (packets) can't get through.

**Tools Required:** `FirewallLab.html` (Tab: DoS).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Attacker IP:** `192.168.1.[LastDigit]`.

---

## **Part 2: The Flood**

1.  **Action:**
    -   Set High Rate.
    -   Start Flood.
    -   Watch Server Status -> **OFFLINE**.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab10A.docx`

### **Screenshot 1: Offline**
-   **Show:** "Server Offline" red status.
-   **Markup:** **Red Circle**.

### **Part 4: Analysis (Homework Integration)**

1.  **Botnets:** Why do attackers use Botnets (Mirai) instead of their own PC? (1 PC = Weak. 100,000 IoT cameras = Strong. Plus anonymity).

### **Part 5: References & Further Reading**

1.  **Explanation:** [Denial-of-service attack (Wikipedia)](https://en.wikipedia.org/wiki/Denial-of-service_attack)
2.  **Visual:**
```merm
graph TD
    Attacker[Attacker/Botnet] -->|Thousands of Requests| Target[Victim Server]
    User[Legitimate User] -->|Request| Target
    Target -->|503 Service Unavailable| User
```


