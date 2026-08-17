
# LAB 7 – CLIENT-SIDE CHAOS (XSS)**

Topic: Reflected Cross-Site Scripting (XSS), Cookies, & The Same-Origin Policy.


Tools Required: A Text Editor (Notepad/TextEdit) & A Web Browser.

---

## **Part 1: Student Identity Parameters (SIP)**

**CRITICAL:** You must use the specific parameters below based on your own identity.

1. **The Payload:** `<img src=x onerror=alert('Hacked by [Your_Student_ID]')>`
    
    - _Example:_ `<img src=x onerror=alert('Hacked by 801234567')>`
        
    - _Note:_ We use the `onerror` trick because modern browsers often block specific `<script>` tags in the URL, but they rarely block broken images.
        

---

## **Part 2: The Experiment**

### **Task A: The Vulnerable Search Engine**

1. Create a new file named `lab7.html` on your Desktop.
    
2. Paste the following code:
    
    HTML
    
    ```
    <!DOCTYPE html>
    <html>
    <body>
        <h1>ITIS 6200/8200 Search</h1>
        <p>You searched for: <span id="output"></span></p>
    
        <script>
            // VULNERABILITY: Taking URL params and putting them directly into the HTML
            const urlParams = new URLSearchParams(window.location.search);
            const query = urlParams.get('q');
    
            if (query) {
                // 'innerHTML' interprets the input as code!
                document.getElementById("output").innerHTML = query;
            }
        </script>
    </body>
    </html>
    ```
    
3. Save the file.
    

### **Task B: The Attack**

1. Open `lab7.html` in your browser by double-clicking it.
    
2. Look at the Address Bar. It should look like `file:///.../lab7.html`.
    
3. **Append your SIP Payload** to the end of the URL:
    
    - `file:///.../lab7.html?q=<img src=x onerror=alert('Hacked by 801234567')>`
        
4. Press **Enter**.
    
5. **Observation:** An alert box should immediately pop up displaying your Student ID.
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Pop-Up**

- **Requirement:** Full Desktop screenshot showing the browser alert box.
    
- **Markup:**
    
    - **Red Box** around the URL bar showing your injected code.
        
    - **Yellow Highlight** around the Alert Box text showing your Student ID.
        

### **Analysis Questions**

1. **Mechanism:** Why is this called **"Reflected"** XSS? Where was the malicious script stored before it executed? (Hint: Was it saved in a database, or was it part of the request?)
    
2. **Impact:** If this vulnerability existed on `bank.com`, and you sent the malicious link to a victim, the script would execute _as if_ the victim wrote it. What sensitive data could the script steal? (Hint: `document.cookie`).
    

---

---

# **LAB 7 (VARIANT B): THE VIRAL COMMENT (STORED XSS)**

Topic: Stored XSS & Persistence.

Note: Demonstrates how a single malicious comment can infect every user who visits a page.

---

## **Part 1: Student Identity Parameters (SIP)**

1. **Malicious Comment:** `Nice post! <img src=x onerror=alert('Virus Loaded: [Your_Name]')>`
    

---

## **Part 2: The Experiment**

### **Task A: The Social Media Simulation**

Since we don't have a database, we will use your browser's **Local Storage** to simulate a database.

1. Create a file named `social.html`.
    
2. Paste the following code:
    
    HTML
    
    ```
    <!DOCTYPE html>
    <html>
    <body>
        <h1>MySocialFeed</h1>
        <h3>Comments:</h3>
        <div id="commentsSection"></div>
    
        <button onclick="resetDB()">Clear Database</button>
    
        <script>
            // 1. Load "Comments" from LocalStorage (Simulated Database)
            let storedComment = localStorage.getItem("db_comment");
    
            if (storedComment) {
                // VULNERABILITY: Displaying DB content without sanitization
                document.getElementById("commentsSection").innerHTML = storedComment;
            } else {
                document.getElementById("commentsSection").innerText = "No comments yet.";
            }
    
            function resetDB() {
                localStorage.removeItem("db_comment");
                location.reload();
            }
        </script>
    </body>
    </html>
    ```
    

### **Task B: The Infection**

1. Open `social.html`. It says "No comments yet."
    
2. Open the **Developer Console** (F12 $\rightarrow$ Console).
    
3. **Inject the Virus:** Type the following command to insert your malicious comment directly into the storage:
    
    - `localStorage.setItem("db_comment", "Nice post! <img src=x onerror=alert('Virus Loaded: Jian')>")`
        
4. Press Enter.
    
5. **Trigger:** Refresh the page.
    
6. **Observation:** The alert pops up. Close the alert. Refresh again. The alert pops up _again_. It is now permanent.
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Persistent Attack**

- **Requirement:** Screenshot showing the Alert Box on the page.
    
- **Markup:**
    
    - **Red Box** around the Alert Text (`Virus Loaded...`).
        

### **Analysis Questions**

1. **Comparison:** How is **Stored XSS** different from **Reflected XSS** (Lab 7A)? Which one is considered more dangerous, and why?
    
2. **Defense:** Lecture 12 mentions **HTML Sanitization**. If we used a function to convert `<` into `&lt;` and `>` into `&gt;`, would the attack still work? Why or why not?
    

---

---

# **LAB 7 (VARIANT C): THE INVISIBLE OVERLAY (CLICKJACKING)**

Topic: UI Redress Attacks & Frames.

Lecture Reference: Lecture 12 (UI Attacks).

---

## **Part 1: Student Identity Parameters (SIP)**

1. **Decoy Button Text:** `CLAIM PRIZE for [Your_Student_ID]`
    

---

## **Part 2: The Experiment**

### **Task A: The Setup**

We will create a "Delete Account" button, and then hide it under a "Free Prize" button.

1. Create `clickjack.html`.
    
2. Paste the following code:
    
    ```html
    <!DOCTYPE html>
    <html>
    <style>
        .container { position: relative; width: 300px; height: 100px; }
    
        /* The Victim Button (Bottom Layer) */
        #victim-btn {
            position: absolute; top: 0; left: 0;
            width: 100%; height: 100%;
            background: red; color: white; font-size: 20px;
            border: none; z-index: 1;
        }
    
        /* The Attack Overlay (Top Layer) */
        #decoy-btn {
            position: absolute; top: 0; left: 0;
            width: 100%; height: 100%;
            background: blue; color: white; font-size: 20px;
            border: none; z-index: 2;
    
            /* FOR THE LAB: We keep it semi-transparent so you can see it.
               IN REALITY: Attackers set opacity to 0.0 (Invisible) */
            opacity: 0.5; 
            cursor: pointer;
        }
    </style>
    <body>
        <h1>Clickjacking Demo</h1>
        <div class="container">
            <button id="victim-btn" onclick="alert('ACCOUNT DELETED!')">DELETE ACCOUNT</button>
    
            <button id="decoy-btn" onclick="document.getElementById('victim-btn').click()">
                REPLACE_WITH_SIP_TEXT
            </button>
        </div>
    
        <script>
            // Put your text here automatically or edit HTML above
            document.getElementById('decoy-btn').innerText = "CLAIM PRIZE for 801234567"; 
        </script>
    </body>
    </html>
    ```
    

### **Task B: The Deception**

1. Open the file.
    
2. You will see a purple-ish button (Blue decoy mixed with Red victim).
    
3. Click the button that says **CLAIM PRIZE**.
    
4. **Observation:** The browser executes the action of the button underneath: `ACCOUNT DELETED!`.
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Layering**

- **Requirement:** Screenshot showing the semi-transparent buttons.
    
- **Markup:**
    
    - **Red Box** around the text "CLAIM PRIZE for [YourID]".
        
    - **Yellow Highlight** around the alert saying "ACCOUNT DELETED".
        

### **Analysis Questions**

1. **Mechanics:** In a real attack, the attacker loads the victim's bank website in an invisible `<iframe>`. If the user clicks on the "Cat Video," what are they actually clicking on?
    
2. **Defense:** Lecture 12 mentions the HTTP Header `X-Frame-Options`. If a website sets this header to `DENY` or `SAMEORIGIN`, how does that stop this attack?
    

---

---

# **MARKING KEY: LAB 7**

### **Quick Plagiarism Check**

- **Lab 7 (Reflected):**
    
    - Does the URL bar show the `?q=<img...` payload?
        
    - Does the Alert Box contain the Student ID?
        
- **Variant B (Stored):**
    
    - Does the Alert Box contain the Student Name?
        
- **Variant C (Clickjacking):**
    
    - Does the button text include the Student ID?
        

### **Answer Key (Analysis Questions)**

**Lab 7 (Standard)**

- **Q1:** **Reflected.** The script was never stored on the server's hard drive. It was part of the **Request** (URL) sent by the user, and the server immediately "reflected" it back in the Response.
    
- **Q2:** **Session Cookies.** The script runs in the context of the victim's session. It can read `document.cookie`, send that data to the attacker's server, and allow the attacker to hijack the victim's logged-in session.
    

**Variant B (Stored)**

- **Q1:**
    
    - **Difference:** Reflected requires sending a specific link to a victim. Stored saves the malicious script in the database.
        
    - **Danger:** **Stored is more dangerous** because the attacker doesn't need to trick individual users into clicking a link. Anyone who simply views the page (like a forum post) gets infected automatically.
        
- **Q2:** **No.** Sanitization (turning special characters into HTML entities) renders the script harmless. The browser will display the text `<script>` literally instead of executing it as code.
    

**Variant C (Clickjacking)**

- **Q1:** They are clicking on the **Target Site** (e.g., the "Transfer Money" button on the bank site) hidden inside the iframe.
    
- **Q2:** `X-Frame-Options: DENY` tells the browser: "Do not allow this website to be displayed inside an `<frame>` or `<iframe>`." If the browser refuses to load the bank site in the invisible frame, the user cannot click on it, neutralizing the attack.