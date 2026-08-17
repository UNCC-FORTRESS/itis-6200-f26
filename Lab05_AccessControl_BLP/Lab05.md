# Lab 05: Bell-LaPadula Access Control, Web Origins, and Cookies

**Ported from the real Spring 2026 Canvas assignment** (`original_docs/Lab05_Original.docx`). This replaced the never-deployed `archive/Lab05_RSA/` design (RSA actually shipped as **Lab 04**) and `archive/Lab07_AccessControl/` (an RBAC/ABAC tool that was built but never deployed under any lab number — the real access-control lab used the Bell-LaPadula model instead, via the `BLP.py`/`Cases.py` files in this folder). See `../archive/README.md`.

Total: 100 points across 3 steps.

## Step 1: BLP Access Control Visualizer (54 points)

The **Bell-LaPadula (BLP)** model is a confidentiality-focused access-control model: every subject has a maximal and current security level, every object has a classification level, and BLP enforces **no read up, no write down**.

**Tool required:** `BLP.py` + `Cases.py` (in this folder — recovered from Canvas's `Lab5Starter.zip`; students receive this same starter pair). `BLP.py` implements the BLP logic (subjects/objects, security-level changes, read/write flow) and should not need modification. `Cases.py` is where you add each scenario (Case 1 is prewritten as an example); running the file visualizes each case via console output. You may use a different language, but must cover all 18 test cases and match the same console output format.

**Subjects:** Alice, Bob, Eve (each with their own current/maximal security level).
**Objects:** `pub.txt`, `emails.txt`, `username.txt`, `password.txt` (each with its own classification).
**Levels:** Unclassified (U), Classified (C), Secret (S), Top Secret (TS).

**Deliverables per case (1-18):** (1 pt) full screenshot of the console output, boxed red. (1 pt) 1-2 sentence explanation of why access was allowed/denied. (1 pt) public GitHub link to your code, titled `BLPVisualizer`, in your ITIS-course repo.

**The 18 cases:**
1. Alice reads `emails.txt`
2. Alice reads `password.txt`
3. Eve reads `pub.txt`
4. Eve reads `emails.txt`
5. Bob reads `password.txt`
6. Alice reads `emails.txt` then writes to `pub.txt`
7. Alice reads `emails.txt` then writes to `password.txt`
8. Alice reads `emails.txt` then writes to `emails.txt`; then reads `username.txt` then writes to `emails.txt`
9. Alice reads `username.txt` then writes to `emails.txt`; then reads `password.txt` then writes to `password.txt`
10. Alice reads `pub.txt` then writes to `emails.txt`; Bob then reads `emails.txt`
11. Alice reads `pub.txt` then writes to `username.txt`; Bob then reads `username.txt`
12. Alice reads `pub.txt` then writes to `password.txt`; Bob then reads `password.txt`
13. Alice reads `pub.txt` then writes to `emails.txt`; Eve then reads `emails.txt`
14. Alice reads `emails.txt` then writes to `pub.txt`; Eve then reads `pub.txt`
15. Alice sets her level to Secret then reads `username.txt`
16. Alice reads `emails.txt`, then sets her level to Unclassified and writes to `pub.txt`; Eve then reads `pub.txt`
17. Alice reads `username.txt`, then sets her level to Classified and writes to `emails.txt`; Eve then reads `emails.txt`
18. Eve reads `pub.txt` then reads `emails.txt`

*Note for whoever grades this: Cases 8-9, 13-14, and 16-17 are the ones that actually test the "no write down" / information-flow rule chains (a low-clearance subject later reading data that was written by a higher-clearance one, or a level change mid-sequence) — they're where most conceptual mistakes will show up in student submissions.*

## Step 2: URLs and Intro to Web (20 points)

**Same-origin analysis.** For each URL below, determine whether it shares the same origin as `https://cci.charlotte.edu`, and justify using protocol + domain + port (1 pt each):
1. `https://cci.charlotte.edu/sis-faculty/`
2. `https://www.charlotte.edu`
3. `https://cci.charlotte.edu:443`
4. `http://cci.charlotte.edu`
5. `https://cci.charlott3.edu`

**Analysis:**
- Q6 (3 pts): a sysadmin blocks any inbound HTTP request whose URL path contains the literal string `/etc/passwd`. Using URL escaping, explain how an attacker bypasses this filter and why it works.
- Q7 (8 pts): describe the relationship between HTML, JavaScript, and the DOM during page rendering. How can an attacker who embeds their own JavaScript into a page exploit this relationship for phishing?
- Q8 (4 pts): what's the functional difference between a URL's Domain and its Port? Why might one server use multiple ports?

## Step 3: Cookies (26 points)

- Q9 (10 pts): explain the operational difference between the Same-Origin Policy (SOP) and the browser's Cookie Policy. Give a concrete example where two URLs can share cookies under the Cookie Policy but would be blocked from interacting via JavaScript under SOP.
- Q10 (5 pts): a developer claims their app is CSRF-safe because SOP prevents external sites from reading their server's responses. Why is this reasoning flawed in the context of CSRF specifically?
- Q11 (6 pts): an engineer proposes defending against CSRF by strictly validating the `Referer` header and dropping requests where it's missing or mismatched. What breaks with this approach, and why is `SameSite=Strict` architecturally more robust?
- Q12 (5 pts): an attacker finds a way to execute their own JavaScript in a banking site's victim's browser, but can't steal the session token directly. Based on standard cookie security attributes, explain why the direct theft failed, and what alternative attack the JavaScript could still perform.

*(This section is theory only. The hands-on CSRF/XSS exploitation happens in **Lab 06**'s DVWA exercise — see `../Lab06_DVWA_PenTest/`. Optional supplementary interactive tools for this section: `../archive/Lab08_WebIntro/tools/CookieShop.html` and `CSRF_Simulation.html`, built but never deployed under their own lab number.)*

## References & Further Reading

Lab-wise, if you face any difficulties regarding the setup, tool usage, or markup help, please feel free to reach out to the TAs during office hours or via email. All assessments and deductions are done at the discretion of the TAs, but mostly based off of a preset rubric to ensure fairness and consistency — if you have any reservations with how your submission was graded, please raise them within three days of receiving your points.

1.  **Ref:** [Bell-LaPadula Model (Wikipedia)](https://en.wikipedia.org/wiki/Bell%E2%80%93LaPadula_model) — the formal no-read-up/no-write-down rules Step 1's 18 cases are testing.
2.  **MDN:** [Same-origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) — background for Step 2's origin-comparison questions.

## AI Appendix & submission format

Follow `../guidelines.txt` — not restated here.
