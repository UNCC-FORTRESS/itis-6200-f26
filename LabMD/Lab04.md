# **LAB 4 – HASHING & INTEGRITY**

**Topic**: Cryptographic Hash Functions (SHA-256), Integrity, & The Avalanche Effect.

**Tools Required**: Web Browser (CyberChef).

---
 
## **Part 1: Student Identity Parameters (SIP)**

**CRITICAL:** You must use the specific parameters below based on your own identity.

1. **String A (Base):** `"I, [Your Full Name], am a student in ITIS 6200/8200."`
    
    - _Example:_ `I, Jian Xiang, am a student in ITIS 6200/8200.`
        
2. **String B (Tampered):** Change the **punctuation** at the very end of String A from a period `.` to an exclamation mark `!`.
    
    - _Example:_ `I, Jian Xiang, am a student in ITIS 6200/8200!`
        

---

## **Part 2: The Experiment**

### **Task A: The Baseline Hash**

1. Open [CyberChef](https://gchq.github.io/CyberChef/).
    
2. Drag **"SHA2"** (SHA-256) into the Recipe.
    
3. Paste **String A** into the Input.
    
4. Copy the resulting **Output Hash** (64 hex characters) into a text file.
    

### **Task B: The Avalanche**

1. Paste **String B** into the Input (just changing that one character).
    
2. Observe the new Output Hash.
    
3. Copy this hash into the text file below the first one.
    

### **Task C: Comparison**

1. Place the two hashes one above the other in your text file.
    
2. Visually compare them. Are they similar? (e.g., did only the last few characters change?)
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Comparison**

- **Requirement:** Full Desktop screenshot of your text editor showing both hashes.
    
- **Markup:**
    
    - **Red Box** around the first 4 characters of Hash A.
        
    - **Red Box** around the first 4 characters of Hash B. (They must be totally different).
        
    - **Yellow Highlight** over the rest of the hashes.
        

### **Analysis Questions**

1. **Observation:** You changed less than 1 byte of data (just 1 bit difference in ASCII). Approximately what percentage of the output bits changed? (5%? 50%? 99%?)
    
2. **Concept:** This is called the **Avalanche Effect**. Why is this property critical for file integrity? If the hash only changed slightly, what could an attacker theoretically do?
    
3. **Application:** When you download a Windows Installer `.iso`, the website often lists a SHA-256 hash.1 If a hacker modified the installer to include a virus, why is it mathematically impossible for them to make the virus-laden file have the same hash as the original? (Keyword: Pre-image Resistance).
    

---

---

# **LAB 4 (VARIANT B): THE BIRTHDAY PARADOX (COLLISIONS)**

Topic: Collision Resistance & Hash Length.

Note: Demonstrates that while SHA-256 is secure, "short" hashes are vulnerable to collisions.

---

## **Part 1: Student Identity Parameters (SIP)**

1. **Target Prefix:** The first 4 characters of the SHA-256 hash of your **First Name**.
    
    - _Example:_ SHA256("Jian") = `ac23...`. Target = `ac23`.
        

---

## **Part 2: The Experiment**

### **Task A: The Script**

We will simulate a "weak" hash function by only looking at the first 2 bytes (4 hex chars) of SHA-256. We want to find a _different_ string that hashes to the same start as your name.

Copy this script (Python):

Python

```
import hashlib

# ENTER YOUR FIRST NAME HERE
target_name = "Jian"

def get_prefix(s):
    return hashlib.sha256(s.encode()).hexdigest()[:4]

target_hash = get_prefix(target_name)
print(f"Target: Hash({target_name}) starts with [{target_hash}]")
print("Searching for a collision...")

count = 0
while True:
    candidate = f"Attempt{count}"
    if get_prefix(candidate) == target_hash:
        print(f"\n>> COLLISION FOUND! <<")
        print(f"String 1: {target_name}  -> {target_hash}...")
        print(f"String 2: {candidate} -> {get_prefix(candidate)}...")
        break
    count += 1
```

### **Task B: Execution**

1. Run the script.
    
2. It will brute-force generic strings (`Attempt1`, `Attempt2`...) until it finds one that matches your name's hash prefix.
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Collision**

- **Requirement:** Screenshot of the script output showing the two different strings producing the same 4-character hash prefix.
    
- **Markup:**
    
    - **Yellow Highlight** over the matching hash prefix (e.g., `ac23` on both lines).
        

### **Analysis Questions**

1. **Math:** A 4-character hex string represents 16 bits ($2^{16} = 65,536$ possibilities). According to the **Birthday Paradox**, we only need $\sqrt{N}$ tries to find a collision (approx 256 tries). Did your script find a match in roughly that many tries, or did it take thousands? Why do you think this happens?
    
2. **Security:** Modern Hashes use 256 bits (SHA-256) or 512 bits. Why does increasing the length make this attack impossible for modern classical computers?
    

---

---

# **LAB 4 (VARIANT C): COMMITMENT SCHEMES**

Topic: Using Hashes to "Hide" information (Bit Commitment).

Note: Teaches how hashes are used in voting, auctions, and proofs.

---

## **Part 1: Student Identity Parameters (SIP)**

1. **The Secret:** Pick a random number between 1 and 100. (e.g., `42`).
    
2. **The Salt:** Your **Student ID**.
    

---

## **Part 2: The Experiment**

### **Task A: The Commitment (The Envelope)**

Imagine you are betting on a number, but you don't want to reveal it yet.

1. Open [CyberChef](https://gchq.github.io/CyberChef/).
    
2. Input: `[YourSecret]-[YourStudentID]`. (e.g., `42-801234567`).
    
3. **Hash it** (SHA-256).
    
4. This Hash is your "Sealed Envelope."
    

### **Task B: The Verify (Opening the Envelope)**

1. Pretend the betting phase is over. You must now reveal your number.
    
2. Provide the "Plaintext" (`42-801234567`) to the verifier.
    
3. The verifier hashes it. It matches the Sealed Envelope.
    

### **Task C: The Cheater (Tampering)**

1. Try to claim you actually bet on number `99`.
    
2. Input: `99-801234567`.
    
3. Hash it. Compare it to your original Sealed Envelope.
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Evidence**

- **Requirement:** Text file or Screenshot showing:
    
    1. The Commitment Hash (Original).
        
    2. The "Cheating" Hash (Result of changing the number).
        
- **Markup:**
    
    - **Red Box** highlighting that the hashes are completely different.
        
    - **Yellow Highlight** over your Student ID in the Input box.
        

### **Analysis Questions**

1. **Purpose:** Why did we add the **Student ID** (Salt) to the input? If we just hashed the number `42` directly, could an attacker reverse the hash using a Rainbow Table?
    
2. **Utility:** How is this concept used in a **Digital Auction** (sealed bid)? (Explain how bidders can submit a price without others seeing it, but also without being able to change their bid later).
    

---

---

# **TA MARKING KEY: LAB 4**

### **Quick Plagiarism Check**

- **Lab 4 (Standard):**
    
    - Does the input string contain the student's name?
        
    - Are the two hashes visually distinct (Avalanche effect)?
        
- **Variant B (Collision):**
    
    - Does the output show `String 1: [StudentName]`?
        
    - Do the printed hash prefixes match?
        
- **Variant C (Commitment):**
    
    - Does the input format match `Number-ID`?
        

### **Answer Key (Analysis Questions)**

**Lab 4 (Standard)**

- **Q1:** **~50%.** Good hash functions behave chaotically; flipping 1 input bit should flip each output bit with 50% probability.
    
- **Q2:** **Integrity Check.** If the hash didn't change much, an attacker could manipulate the file (e.g., insert malicious code) and tweak the remaining bytes to make the hash match the original. The Avalanche Effect prevents this predictability.
    
- **Q3:** **Pre-image Resistance.** Hashing is a one-way function. You cannot "engineer" an input file to match a specific output hash without brute-forcing the entire universe.
    

**Variant B (Collision)**

- **Q1:** It should take very few tries (hundreds or low thousands), verifying the Birthday Paradox (collision probability rises much faster than people expect).
    
- **Q2:** $2^{256}$ is an astronomically large number. Even with the birthday paradox reducing work to $2^{128}$, that is still computationally impossible for all computers on Earth combined.
    

**Variant C (Commitment)**

- **Q1:** **Prevent Brute Force.** The range of numbers (1-100) is tiny. An attacker could just hash every number from 1-100 and see which one matches your commitment. Adding the unique ID (Salt) expands the input space, making guessing impossible.
    
- **Q2:** Bidders submit `Hash(Price + Salt)`. The auctioneer sees the hashes but not the prices. At the deadline, everyone reveals their `Price` and `Salt`. The auctioneer verifies the hashes match. This prevents bid leaking and bid changing.