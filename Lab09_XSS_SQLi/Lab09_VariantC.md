# **LAB 09 (VARIANT C) – THE LEAKY DATABASE**

**Topic:** SQL Injection (Union).

**Story Context:**
> You can't see the query code, but you can see the *results*.
> By appending a second query (`UNION SELECT...`), you can force the app to display data from other tables.

**Tools Required:** `InjectionLab.html` (Tab: SQLi Union).

---

## **Part 1: Student Identity Parameters (SIP)**

1.  **Table:** `users`.

---

## **Part 2: The Leak**

1.  **Action:**
    -   Inject `UNION SELECT user, password FROM users`.
    -   See credentials appear in product list.

---

## **Part 3: Deliverables**

**Submission File:** `FirstName_LastName_Lab09C.docx`

### **Screenshot 1: The Dump**
-   **Show:** Usernames/Passwords visible.
-   **Markup:** **Red Box**.

### **Part 4: Analysis (Homework Integration)**

1.  **Schema:** If the first query returns 3 columns `(ID, Name, Price)`, and your injected query `SELECT user, pass` returns 2 columns, what happens? (Error). How do you fix it? (`SELECT user, pass, NULL`).

### **Part 5: References & Further Reading**

1.  **Explanation:** [SQL Injection Union Attacks](https://portswigger.net/web-security/sql-injection/union-attacks)
    *   *How to determine column counts and data types to make UNION work.*


---

![XKCD 327: Exploits of a Mom](https://imgs.xkcd.com/comics/exploits_of_a_mom_2x.png)


**School**: Hi, this is your son's school. We're having some computer trouble.

**Mom**: Oh, dear -- Did he break something?

**School**: In a way. Did you really name your son `Robert'); DROP TABLE Students --`?

**Mom**: Oh. Yes. Little Bobby Tables we call him.

**School**: Well, we've lost this year's student records. I hope you're happy.

**Mom**: And I hope you've learned to sanitize your database inputs