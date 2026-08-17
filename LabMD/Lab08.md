# **ITIS 6200/8200: LAB 8 – NETWORK DEFENSE**

**Topic:** HTTP Headers, Firewalls, & Packet Filtering.

**Tools Required:** Web Browser (Chrome/Firefox/Edge) & Text Editor.

---

## **Part 1: Student Identity Parameters (SIP)**

**CRITICAL:** You must use the specific parameters below based on your own identity.

1. **User-Agent String:** You will identify the specific "Signature" of your own web browser.
    
2. **Firewall Comment:** `# Rule added by Admin [Your_Name]`
    

---

## **Part 2: The Experiment**

### **Task A: Inspecting HTTP Headers**

Every time you visit a website, your browser sends a "User-Agent" string telling the server your OS and Browser version.

1. Open your Web Browser.
    
2. Press **F12** (or right-click $\rightarrow$ Inspect) to open **Developer Tools**.
    
3. Click the **Network** tab.
    
4. Visit `http://example.com` (Press Enter).
    
5. Click the first request in the list (named `example.com`).
    
6. Look at the **Request Headers** section on the right.
    
7. Find the line starting with `User-Agent:`.
    

### **Task B: Firewall Logic**

Scenario: You are the network admin. You notice a Denial of Service (DoS) attack coming from IP `192.168.1.50` targeting your web server on Port `80`.

1. Open a Text Editor.
    
2. Write a "Pseudo-code" Firewall Rule to block this traffic. Use this format:
    
    - `DROP [Protocol] FROM [Source_IP] TO [Dest_Port]`
        
3. Add your **SIP Comment** on the next line.
    
    - _Example:_
        
        Plaintext
        
        ```
        DROP TCP FROM 192.168.1.50 TO PORT 80
        # Rule added by Admin Jian Xiang
        ```
        

---

## **Part 3: Deliverables**

### **Screenshot 1: The Browser Signature**

- **Requirement:** Fullscreen screenshot of the Browser DevTools showing the Headers.
    
- **Markup:**
    
    - **Red Box** around the `User-Agent` string. (This proves you ran the tool on your machine).
        

### **Screenshot 2: The Firewall Rule**

- **Requirement:** Screenshot of your text editor.
    
- **Markup:**
    
    - **Yellow Highlight** over your name in the comment.
        

### **Analysis Questions**

1. **OSI Model:** In Task A, you inspected an HTTP request. Which Layer of the OSI model does HTTP belong to? (Layer 1, 3, 4, or 7?)
    
2. **Firewalls:** Lecture 17 discusses **Stateful** vs. **Stateless** firewalls.
    
    - If your firewall simply looks at one packet and says "Is this from IP 192.168.1.50? If yes, Drop it," is that **Stateful** or **Stateless**?
        
    - Why is this distinction important?
        

---

---

# **LAB 8 (VARIANT B): THE TRAFFIC ANALYST**

Topic: Packet Analysis & DoS Forensics.

Note: Teaches how to isolate specific traffic streams using Wireshark logic.

---

## **Part 1: Student Identity Parameters (SIP)**

1. **Attacker IP Filter:** `ip.src == [First_2_Digits_of_ID].0.0.1`
    
    - _Example:_ If ID is `801234567`, Filter = `ip.src == 80.0.0.1`
        

---

## **Part 2: The Experiment**

### **Task A: The Scenario**

You have been given a massive log file (PCAP) containing 1 million packets. You need to find the packets sent by a specific attacker. You don't need actual traffic for this lab; you just need to construct the valid **Display Filter**.

### **Task B: The Filter Construction**

1. Open **Wireshark** (if installed) OR simply open a mockup image of Wireshark.
    
2. Locate the **"Apply a display filter"** bar at the top.
    
3. Type your **SIP Filter Logic**:
    
    - `ip.src == 80.0.0.1` (Replace `80` with your ID's start).
        
4. If using real Wireshark, the bar will turn **Green** if the syntax is valid.
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Syntax Check**

- **Requirement:** Screenshot of the Wireshark interface with your filter typed in the bar.
    
- **Markup:**
    
    - **Red Box** around the filter string containing your ID digits.
        

### **Analysis Questions**

1. **DoS Mechanics:** In a **SYN Flood** attack, the attacker sends thousands of `SYN` packets but never sends the final `ACK`. How does this crash the server? (Hint: Does the server have infinite memory to remember these "half-open" connections?)
    
2. **Spoofing:** If the attacker uses a "Spoofed IP" (fake return address), can we still block them easily using the IP filter you just wrote? Why or why not?
    

---

---

# **LAB 8 (VARIANT C): THE SIGNATURE HUNTER (IDS)**

Topic: Intrusion Detection Systems (NIDS) & Snort Rules.

Lecture Reference: Lecture 18 (Intrusion Detection).

---

## **Part 1: Student Identity Parameters (SIP)**

1. **Alert Message:** `msg:"Alert_[Your_Student_ID]_Malware_Detected"`
    
2. **Malicious Content:** `"evil_script"`
    

---

## **Part 2: The Experiment**

### **Task A: Writing a NIDS Rule**

We need a rule for the **Snort** IDS that alerts us whenever someone sends the text "evil_script" over HTTP (Port 80).

1. Open a text editor.
    
2. Construct the rule using this syntax:
    
    - `[Action] [Proto] [SrcIP] [SrcPort] -> [DestIP] [DestPort] ( [Options] )`
        
3. **Your Rule:**
    
    - Action: `alert`
        
    - Protocol: `tcp`
        
    - Source/Dest: `any any -> any 80`
        
    - Options: `msg:"Alert_801234567_Malware_Detected"; content:"evil_script";`
        

### **Task B: Verification**

1. Ensure your syntax contains semicolons `;` correctly inside the parenthesis.
    
2. Ensure your Student ID is in the `msg` field.
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Snort Rule**

- **Requirement:** Screenshot of your text editor showing the full rule.
    
- **Markup:**
    
    - **Yellow Highlight** over the `msg` section with your ID.
        
    - **Red Box** around the `content` section.
        

### **Analysis Questions**

1. **Detection Styles:** Lecture 18 discusses **Signature-based** vs. **Anomaly-based** detection. Is the rule you just wrote Signature or Anomaly?
    
2. **Evasion:** If the attacker changes their malware to send `"eVil_ScRiPt"` (mixed case) or `"evil_script_v2"`, will your rule still catch it? What is the weakness of this detection style?
    

---

---

# **MARKING KEY: LAB 8**

### **Quick Plagiarism Check**

- **Lab 8 (Standard):**
    
    - **User-Agent:** Does it look like a real UA string (e.g., `Mozilla/5.0...`)?
        
    - **Firewall:** Does the comment contain `# ... [Student Name]`?
        
- **Variant B (Wireshark):**
    
    - Does the filter start with the first 2 digits of their ID? (e.g., `ip.src == 80...`).
        
- **Variant C (Snort):**
    
    - Does the `msg` field contain their Student ID?
        

### **Answer Key (Analysis Questions)**

**Lab 8 (Standard)**

- **Q1:** **Layer 7 (Application Layer).** HTTP is the data the user interacts with. (Note: IP is Layer 3, TCP is Layer 4).
    
- **Q2:**
    
    - **Stateless.** It looks at the packet in isolation (Source IP) without caring if it's part of an established login session or a new connection.
        
    - **Importance:** Stateless is faster but dumber. Stateful is slower but can block things like "packets that claim to be part of a stream but actually aren't."
        

**Variant B (Wireshark)**

- **Q1:** **Resource Exhaustion (State Table).** The server allocates memory (a TCB - Transmission Control Block) for every SYN it receives, waiting for the ACK. If millions of SYNs arrive, the server's memory fills up, and it drops legitimate connections.
    
- **Q2:** **No.** If the attacker spoofs random IPs (e.g., 1.2.3.4, then 5.6.7.8), blocking one specific IP won't stop the attack. You would need to block the _pattern_ or use rate limiting.
    

**Variant C (Snort)**

- **Q1:** **Signature-based.** It is looking for a known "fingerprint" (the specific string "evil_script").
    
- **Q2:** **No.** Signature matching is usually exact. If the attacker changes one byte, the signature fails. This is the main weakness of signature-based detection (it cannot detect new/zero-day attacks).