# **LAB 5 – TRUST & SECRETS (PKI)**

**Topic:** Public Key Infrastructure (PKI), Certificates, & Password Hashing.

**Tools Required:** Web Browser, [CrackStation](https://crackstation.net/), & Terminal (or [Online Self-Signed Cert Generator](https://www.google.com/search?q=https://getacert.com/self-signed-cert-generator.html)).

---

## **Part 1: Student Identity Parameters (SIP)**

**CRITICAL:** You must use the specific parameters below based on your own identity.

1. **Common Name (CN):** `[Your_Student_ID].com`
    
    - _Example:_ `801234567.com`
        
2. **Weak Password:** `password[First_Letter_Of_Name]`
    
    - _Example:_ `passwordJ`
        
3. **Salt:** Your **Student ID**.
    

---

## **Part 2: The Experiment**

### **Task A: The "Untrusted" Certificate**

We will generate a digital certificate to see how browsers react to unknown authorities.

1. Use an **Online Self-Signed Cert Generator** (or OpenSSL if you are comfortable).
    
2. **Configuration:**
    
    - **Common Name:** Enter your **SIP Common Name** (e.g., `801234567.com`).
        
    - **Organization:** `ITIS 6200/8200 Lab`.
        
    - **Country:** `US`.
        
3. **Generate:** Click the button to create the certificate.
    
4. **Inspect:** Download (or view) the Certificate file (`.crt` or `.pem`). Open it to view the **Details**.
    
5. **Observation:** Notice that your OS or Browser warns that "This root certificate is not trusted."
    

### **Task B: Salting the Hash**

1. **Unsalted (The Mistake):**
    
    - Calculate the **MD5 hash** of your **Weak Password** (e.g., `passwordJ`).
        
    - Copy the hash to [CrackStation.net](https://crackstation.net/).
        
    - **Result:** It should crack instantly (Green).
        
2. **Salted (The Defense):**
    
    - Concatenate `WeakPassword` + `Salt`.
        
    - _Example:_ `passwordJ801234567`.
        
    - Calculate the **MD5 hash** of this new string.
        
    - Copy this new hash to CrackStation.
        
    - **Result:** It should fail (Red/Not Found).
        

---

## **Part 3: Deliverables**

### **Screenshot 1: The Certificate**

- **Requirement:** Screenshot of the Certificate Viewer (Windows/Mac) showing the General or Details tab.
    
- **Markup:**
    
    - **Red Box** around the **Issued To / Common Name** (Must match your ID).
        

### **Screenshot 2: The Salted Defense**

- **Requirement:** Screenshot of CrackStation.
    
- **Markup:**
    
    - **Yellow Highlight** showing the **"Not Found"** result for your salted hash.
        

### **Analysis Questions**

1. **PKI:** Why does your browser trust `google.com`'s certificate but automatically rejects the one you just made? (Keyword: **Root CA**).
    
2. **Passwords:** Lecture 7 mentions "Offline Attacks." Explain how adding the **Salt** prevents an attacker from using a pre-computed **Rainbow Table** to crack your password.
    

---

---

# **LAB 5 (VARIANT B): THE ROOT OF TRUST**

Topic: Installing Root CAs & Man-in-the-Middle (MITM).

Note: Demonstrates how corporate proxies or malware can intercept encrypted traffic by installing a custom Root CA.

---

## **Part 1: Student Identity Parameters (SIP)**

1. **Organization Name:** `TrustMe_[YourStudentID]_Inc`
    
    - _Example:_ `TrustMe_801234567_Inc`
        

---

## **Part 2: The Experiment**

### **Task A: The Rejection**

1. Generate a Self-Signed Certificate (using OpenSSL or online tool).
    
2. Set the **Organization (O)** to your **SIP Organization Name**.
    
3. Open the certificate file on your computer.
    
4. Observe the big red 'X' or warning: _"This certificate is not trusted."_
    

### **Task B: The Injection**

_Warning: Only do this on your personal machine, and delete the cert afterwards._

1. **Windows:** Click "Install Certificate" $\rightarrow$ "Local Machine" $\rightarrow$ "Place all certificates in the following store" $\rightarrow$ **"Trusted Root Certification Authorities"**.
    
2. **Mac:** Double click cert $\rightarrow$ Add to Keychain $\rightarrow$ Double click again $\rightarrow$ Expand "Trust" $\rightarrow$ Set to **"Always Trust"**.
    
3. **Firefox:** Settings $\rightarrow$ Certificates $\rightarrow$ View Certificates $\rightarrow$ Import.
    

### **Task C: The Acceptance**

1. Close and Re-open the certificate file.
    
2. It should now say: _"This certificate is valid"_ (or have a Green Checkmark).
    

---

## **Part 3: Deliverables**

### **Screenshot 1: Before & After**

- **Requirement:** Two screenshots side-by-side (or one above the other).
    
    1. The Certificate showing the **Error/Not Trusted** status.
        
    2. The Certificate showing the **Valid/Trusted** status.
        
- **Markup:**
    
    - **Red Box** around the **Organization Name** in both images.
        

### **Analysis Questions**

1. **Scenario:** If a hacker installed a malicious Root CA on your laptop (like you just did), and you visited `bankofamerica.com`, could the hacker show you a "Fake" certificate for the bank without your browser warning you?
    
2. **Mechanism:** This is often called a **MITM (Man-in-the-Middle)** attack. How does the "Trusted Root Store" normally prevent this?
    

---

---

# **LAB 5 (VARIANT C): THE HASH RACE**

Topic: Slow Hashes (Bcrypt) vs. Fast Hashes (MD5).

Note: Demonstrates why "speed" is bad for password hashing.

---

## **Part 1: Student Identity Parameters (SIP)**

1. **Input String:** `Race_[YourFirstname]`
    
    - _Example:_ `Race_Jian`
        

---

## **Part 2: The Experiment**

### **Task A: The Script**

Copy and run this Python script to benchmark hashing speeds.

Python

```
import hashlib
import time
import bcrypt # You may need: pip install bcrypt

# --- SIP CONFIGURATION ---
student_input = "Race_Jian"  # CHANGE THIS
# -------------------------

password = student_input.encode()

print(f"Benchmarking for input: {student_input}\n")

# 1. MD5 BENCHMARK (The Hare)
start = time.time()
for i in range(100000): # Run 100,000 times
    hashlib.md5(password).hexdigest()
end = time.time()
md5_time = end - start
print(f"MD5 (100k runs): {md5_time:.4f} seconds")

# 2. BCRYPT BENCHMARK (The Tortoise)
# work_factor 12 is standard
start = time.time()
for i in range(5): # Run only 5 times!
    bcrypt.hashpw(password, bcrypt.gensalt(12))
end = time.time()
bcrypt_time = end - start
print(f"Bcrypt (5 runs):   {bcrypt_time:.4f} seconds")

# Extrapolate
print(f"\nEstimated time for 100k Bcrypts: {(bcrypt_time/5)*100000:.2f} seconds")
```

### **Task B: Execution**

1. Run the script.
    
2. Observe the massive difference in time. MD5 can do 100,000 hashes instantly. Bcrypt takes significant time even for 5 hashes.
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Benchmark**

- **Requirement:** Screenshot of the script output terminal.
    
- **Markup:**
    
    - **Red Box** around the `Estimated time for 100k Bcrypts`.
        
    - **Yellow Highlight** around your specific `student_input` string printed at the top.
        

### **Analysis Questions**

1. **Economics:** If an attacker has a GPU cluster that can calculate 10 billion MD5 hashes per second, why does switching to **Bcrypt** effectively destroy their investment?
    
2. **Design:** Lecture 7 states that "Salt prevents Rainbow Tables" and "Slow Hashes prevent Brute Force." Explain the difference between these two defenses.
    

---

---

# **TA MARKING KEY: LAB 5**

### **Quick Plagiarism Check**

- **Lab 5 (Standard):**
    
    - **Cert:** Is the "Issued To" field the Student's ID?
        
    - **CrackStation:** Is the result "Not Found"?
        
- **Variant B (Root Trust):**
    
    - Does the Org Name contain the Student ID?
        
    - Does the second screenshot show the cert is valid/trusted?
        
- **Variant C (Hash Race):**
    
    - Does the script output match the Python format?
        
    - Is the input string `Race_[Name]`?
        

### **Answer Key (Analysis Questions)**

**Lab 5 (Standard)**

- **Q1:** **Root of Trust.** Browsers come pre-installed with a list of "Trusted Root CAs" (like DigiCert, Let's Encrypt). The student's self-signed cert is not signed by any of these trusted roots, so the chain of trust fails.
    
- **Q2:** **Rainbow Tables** are pre-computed lists of `Hash -> Password`. If you Salt the password (`Hash(Salt + Password)`), the pre-computed hash in the table won't match the user's hash, even if the password is "123456". The attacker would need to build a _new_ table specifically for that one salt, which is computationally infeasible.
    

**Variant B (Root Trust)**

- **Q1:** **Yes.** If the root is trusted, the browser will trust _any_ certificate signed by that root. A MITM attacker can generate a fake cert for `bankofamerica.com`, sign it with their malicious root, and the victim's browser will show the "Green Lock."
    
- **Q2:** Normally, the Trusted Root Store acts as a **Allowlist**. Only authorities vetted by Apple/Microsoft/Mozilla are allowed to vouch for websites.
    

**Variant C (Hash Race)**

- **Q1:** Bcrypt is **CPU-Hard** and **Memory-Hard**. It is designed to be slow. If one hash takes 0.5 seconds, 10 billion hashes would take ~158 years. It forces the attacker to spend massive amounts of electricity and time.
    
- **Q2:**
    
    - **Salt:** Stops pre-computation (Rainbow Tables) where the attacker looks up the answer.
        
    - **Slow Hash:** Stops real-time guessing (Brute Force) where the attacker tries every combination.