# **LAB 2 – THE PERFECT SECRET (OTP)**

**Topics:** Binary, Bitwise XOR, One-Time Pads, & Key Reuse.

---



![Image of XOR logic gate symbol](https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcSQZ1MFGROPciNRU2OwJWvMVlNHcl9kfiAP7xtOwSflb-BDOfrza8fFsiXmxVCOMHecZt9llnEv7Xe3Po1et5iqE-10q2_mVkN8e8wVy_sefdZf1W8)



**Tools Required:** Spreadsheet (Excel/Google Sheets) OR Python OR Pen & Paper.

---

## **Part 1: Student Identity Parameters (SIP)**

**CRITICAL:** You must use the specific parameters below based on your own identity. Submitting a lab using generic values will result in a grade of **0**.

1. **Your Message (M):** The **First 3 Letters** of your First Name.
    
    - _Example:_ `Jian` $\rightarrow$ `JIA`.
        
2. **Your Key (K):** The **Last 3 Digits** of your Student ID.
    
    - _Example:_ ID `801234567` $\rightarrow$ Key is `5`, `6`, `7`.
        

---

## **Part 2: The Experiment**

### **Task A: Binary Construction**

1. Convert the 3 letters of your **Message** into 8-bit Binary (ASCII).
    
    - _Tip:_ You can use an online "Text to Binary" converter.
        
2. Convert the 3 digits of your **Key** into 8-bit Binary (ASCII).
    
    - _Note:_ Treat the digits as characters (e.g., '8' is ASCII 56, which is `00111000`).
        

### **Task B: The XOR Calculation ($M \oplus K$)**

1. Set up a Spreadsheet or write on paper.
    
2. Align the binary of your Message directly above the binary of your Key.
    
3. Perform the **XOR** operation bit-by-bit down the columns.
    
    - **Rules:** $0\oplus0=0$, $1\oplus1=0$, $1\oplus0=1$, $0\oplus1=1$. (Same = 0, Different = 1).
        
4. The result is your **Ciphertext (C)**.
    

### **Task C: The Verification ($C \oplus K$)**

1. Take your **Ciphertext** binary.
    
2. XOR it again with the **Key** binary.
    
3. Convert the resulting binary back to Text.
    
4. **Requirement:** It _must_ spell your name (Message) again.
    

---

## **Part 3: Deliverables & Screenshots**

### **Screenshot 1: The Calculation Table**

- **Requirement:** A **Full Desktop** screenshot of your spreadsheet, script output, or clear photo of handwritten math.
    
- **Markup:**
    
    - Draw a **Red Box** around the binary row representing your **Key (Student ID)**.
        
    - Use **Yellow Highlighter** on the **Ciphertext** binary row.
        
- **Structure Example:**
    
    Plaintext
    
    ```
    Message (R): 01010010
    Key (8):     00111000
    ---------------------
    Cipher (C):  01101010
    ```
    

---

## **Part 4: Analysis Questions**

1. **Observation:** Look at your Ciphertext binary. Does it look anything like your original Name? If an attacker intercepted _only_ the ciphertext, could they guess your name?
    
2. **Concept:** In this lab, we generated the Key using your Student ID (starting with 80...). Why is this **NOT** a secure One-Time Pad system in the real world? (Hint: Review the requirements for a "Perfect" key in Lecture 2 regarding randomness).
    

---

---

# **LAB 2 (VARIANT B): VISUAL XOR (THE PIXEL PAD)**

**Topic:** Visual Cryptography & The physical meaning of XOR.

**Software:** We have provided a custom **Visual XOR Tool** (HTML file) below.

---

## **Part 1: The Tool Setup**

Since doing this manually on graph paper is tedious, use the following code to generate your unique grids.

1. Copy the code block below.
    
2. Save it on your computer as `visual_xor.html`.
    
3. Open that file in Chrome, Firefox, or Safari.
    


```HTML
<!DOCTYPE html>
<html>
<head>
    <title>ITIS 6200/8200 Lab 2: Multi-Char Visual XOR</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; text-align: center; background: #f4f4f9; color: #333; }
        h1 { margin-bottom: 5px; }
        .controls { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); display: inline-block; margin-bottom: 20px; }
        input { padding: 10px; margin: 5px; width: 250px; font-size: 16px; border: 1px solid #ccc; border-radius: 4px; }
        button { padding: 10px 25px; background: #28a745; color: white; border: none; cursor: pointer; font-size: 16px; border-radius: 4px; }
        button:hover { background: #218838; }
        
        /* Container for the visual rows */
        .visual-stack { display: flex; flex-direction: column; align-items: center; gap: 10px; overflow-x: auto; padding: 10px; }
        
        .row-container { display: flex; align-items: center; gap: 15px; }
        .label-box { width: 120px; text-align: right; font-weight: bold; }
        .canvas-box { background: white; padding: 10px; border-radius: 4px; box-shadow: 0 1px 3px rgba(0,0,0,0.2); }
        
        canvas { border: 1px solid #ddd; image-rendering: pixelated; display: block; }
        
        /* Highlight specific rows for the assignment */
        .key-row canvas { border: 3px solid #dc3545; } /* Red for Key */
        .cipher-row canvas { border: 3px solid #ffc107; } /* Yellow for Cipher */
        .decrypt-row canvas { border: 3px solid #28a745; } /* Green for Success */

        .note { font-size: 0.9em; color: #666; max-width: 600px; margin: 0 auto; }
    </style>
</head>
<body>

    <h1>Lab 2: Visual XOR Tool (Multi-Char)</h1>
    <p class="note">Enter a word (up to 8 chars) and your Student ID to see the One-Time Pad in action.</p>
    <br>

    <div class="controls">
        <label>Message (Text):</label><br>
        <input type="text" id="msgInput" placeholder="e.g. SECRET" maxlength="8" value="LABTWO">
        <br><br>
        <label>Key (Student ID):</label><br>
        <input type="text" id="idInput" placeholder="e.g. 801234567" value="801234567">
        <br><br>
        <button onclick="runEncryption()">Encrypt & Decrypt</button>
    </div>

    <div class="visual-stack" id="displayArea" style="display:none;">
        
        <div class="row-container">
            <div class="label-box">Message (M)</div>
            <div class="canvas-box">
                <canvas id="cvsM" height="60"></canvas>
            </div>
        </div>

        <div class="row-container">
            <div class="label-box">Key (K)<br><span style="font-size:0.8em; color:red;">(Based on ID)</span></div>
            <div class="canvas-box key-row">
                <canvas id="cvsK" height="60"></canvas>
            </div>
        </div>

        <div class="row-container">
            <div class="label-box">Ciphertext (C)<br><span style="font-size:0.8em; color:#d39e00;">(M ⊕ K)</span></div>
            <div class="canvas-box cipher-row">
                <canvas id="cvsC" height="60"></canvas>
            </div>
        </div>

        <hr style="width: 50%; border: 1px dashed #ccc; margin: 20px 0;">

        <div class="row-container">
            <div class="label-box">Decryption<br><span style="font-size:0.8em; color:green;">(C ⊕ K = M)</span></div>
            <div class="canvas-box decrypt-row">
                <canvas id="cvsD" height="60"></canvas>
            </div>
        </div>

    </div>

    <script>
        // 4x4 Bitmap Font for A-Z and 0-9
        // 1 = Black, 0 = White
        // Each array is 16 integers
        const fontMap = {
            'A': [0,1,1,0, 1,0,0,1, 1,1,1,1, 1,0,0,1],
            'B': [1,1,1,0, 1,0,0,1, 1,1,1,0, 1,1,1,0],
            'C': [0,1,1,1, 1,0,0,0, 1,0,0,0, 0,1,1,1],
            'D': [1,1,1,0, 1,0,0,1, 1,0,0,1, 1,1,1,0],
            'E': [1,1,1,1, 1,1,0,0, 1,0,0,0, 1,1,1,1],
            'F': [1,1,1,1, 1,1,0,0, 1,0,0,0, 1,0,0,0],
            'G': [0,1,1,1, 1,0,0,0, 1,0,1,1, 0,1,1,1],
            'H': [1,0,0,1, 1,1,1,1, 1,0,0,1, 1,0,0,1],
            'I': [0,1,1,0, 0,1,1,0, 0,1,1,0, 0,1,1,0],
            'J': [0,0,0,1, 0,0,0,1, 1,0,0,1, 0,1,1,0],
            'K': [1,0,0,1, 1,1,1,0, 1,0,1,0, 1,0,0,1],
            'L': [1,0,0,0, 1,0,0,0, 1,0,0,0, 1,1,1,1],
            'M': [1,1,1,1, 1,0,0,1, 1,0,0,1, 1,0,0,1], // Simplified
            'N': [1,0,0,1, 1,1,0,1, 1,0,1,1, 1,0,0,1],
            'O': [0,1,1,0, 1,0,0,1, 1,0,0,1, 0,1,1,0],
            'P': [1,1,1,0, 1,0,0,1, 1,1,1,0, 1,0,0,0],
            'Q': [0,1,1,0, 1,0,0,1, 0,1,1,0, 0,0,1,1],
            'R': [1,1,1,0, 1,0,0,1, 1,1,0,0, 1,0,0,1],
            'S': [0,1,1,1, 1,1,0,0, 0,0,1,1, 1,1,1,0],
            'T': [1,1,1,1, 0,1,1,0, 0,1,1,0, 0,1,1,0],
            'U': [1,0,0,1, 1,0,0,1, 1,0,0,1, 0,1,1,0],
            'V': [1,0,0,1, 1,0,0,1, 0,1,1,0, 0,1,0,0],
            'W': [1,0,0,1, 1,0,0,1, 1,0,0,1, 0,1,1,0], // Simplified
            'X': [1,0,0,1, 0,1,1,0, 0,1,1,0, 1,0,0,1],
            'Y': [1,0,0,1, 0,1,1,0, 0,1,0,0, 0,1,0,0],
            'Z': [1,1,1,1, 0,0,1,0, 0,1,0,0, 1,1,1,1],
            '0': [0,1,1,0, 1,0,0,1, 1,0,0,1, 0,1,1,0],
            '1': [0,0,1,0, 0,1,1,0, 0,0,1,0, 0,1,1,1],
            '2': [1,1,1,0, 0,0,0,1, 0,1,1,0, 1,1,1,1],
            '3': [1,1,1,0, 0,0,1,1, 0,0,0,1, 1,1,1,0],
            '4': [1,0,0,1, 1,1,1,1, 0,0,0,1, 0,0,0,1],
            '5': [1,1,1,1, 1,1,0,0, 0,0,1,1, 1,1,1,0],
            '6': [0,1,1,0, 1,0,0,0, 1,1,1,0, 0,1,1,0],
            '7': [1,1,1,1, 0,0,0,1, 0,0,1,0, 0,1,0,0],
            '8': [0,1,1,0, 1,0,0,1, 1,0,0,1, 0,1,1,0],
            '9': [0,1,1,0, 1,0,0,1, 0,0,1,0, 0,1,0,0]
        };
        const defaultChar = [1,1,1,1, 1,0,0,1, 1,0,0,1, 1,1,1,1]; // Square box for unknown

        function runEncryption() {
            // 1. Get Inputs
            let msg = document.getElementById('msgInput').value.toUpperCase().replace(/[^A-Z0-9]/g, '');
            let id = document.getElementById('idInput').value.replace(/[^0-9]/g, '');

            if (msg.length === 0) { alert("Please enter a message."); return; }
            if (id.length === 0) { alert("Please enter a Student ID."); return; }

            // 2. Setup Dimensions
            const cellSize = 15; // px size per bit
            const charWidth = 4; // 4 bits wide
            const charHeight = 4; // 4 bits high
            const gap = 1; // gap between letters
            
            // Calculate total width based on message length
            const totalCols = (msg.length * charWidth) + (msg.length * gap);
            const cvsWidth = totalCols * cellSize;
            const cvsHeight = charHeight * cellSize;

            // 3. Prepare Canvases
            const cM = document.getElementById('cvsM');
            const cK = document.getElementById('cvsK');
            const cC = document.getElementById('cvsC');
            const cD = document.getElementById('cvsD');

            [cM, cK, cC, cD].forEach(c => {
                c.width = cvsWidth;
                c.height = cvsHeight;
            });

            // 4. Generate Data Streams
            let bitsM = []; // Message Bits
            let bitsK = []; // Key Bits
            let bitsC = []; // Cipher Bits
            let bitsD = []; // Decrypted Bits

            // Build Message Stream
            for (let i = 0; i < msg.length; i++) {
                let char = msg[i];
                let bitmap = fontMap[char] || defaultChar;
                bitsM = bitsM.concat(bitmap);
            }

            // Build Key Stream (Repeat ID digits to match length)
            // Length needed = msg.length * 16 bits
            const totalBits = bitsM.length;
            for (let i = 0; i < totalBits; i++) {
                let digitChar = id[i % id.length];
                let digit = parseInt(digitChar);
                // Logic: Odd digit = 1 (Black), Even digit = 0 (White)
                bitsK.push(digit % 2 !== 0 ? 1 : 0);
            }

            // XOR Calculation (Encryption)
            for (let i = 0; i < totalBits; i++) {
                bitsC.push(bitsM[i] ^ bitsK[i]);
            }

            // XOR Calculation (Decryption: Cipher ^ Key)
            for (let i = 0; i < totalBits; i++) {
                bitsD.push(bitsC[i] ^ bitsK[i]);
            }

            // 5. Drawing Function
            function drawStream(ctx, bits, length) {
                ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
                
                // Loop through every CHAR in the word
                for(let c = 0; c < length; c++) {
                    // Loop through 16 bits of that char
                    for(let b = 0; b < 16; b++) {
                        let bitIndex = (c * 16) + b;
                        let val = bits[bitIndex];

                        // Calculate X/Y local to the 4x4 grid
                        let localX = b % 4;
                        let localY = Math.floor(b / 4);

                        // Calculate global X including gaps
                        let globalX = (c * (charWidth + gap)) + localX;

                        // Draw
                        ctx.fillStyle = val === 1 ? 'black' : 'white';
                        ctx.fillRect(globalX * cellSize, localY * cellSize, cellSize, cellSize);
                        
                        // Grid lines
                        ctx.strokeStyle = '#ddd';
                        ctx.lineWidth = 1;
                        ctx.strokeRect(globalX * cellSize, localY * cellSize, cellSize, cellSize);
                    }
                }
            }

            const ctxM = cM.getContext('2d');
            const ctxK = cK.getContext('2d');
            const ctxC = cC.getContext('2d');
            const ctxD = cD.getContext('2d');

            drawStream(ctxM, bitsM, msg.length);
            drawStream(ctxK, bitsK, msg.length);
            drawStream(ctxC, bitsC, msg.length);
            drawStream(ctxD, bitsD, msg.length);

            // Show the area
            document.getElementById('displayArea').style.display = 'flex';
        }
    </script>
</body>
</html>```

## **Part 2: The Experiment**

1. Run the tool.
    
2. Enter the **First Letter** of your name.
    
3. Enter your **Student ID** (Must start with `80...`).
    
4. Click **Encrypt via XOR**.
    

## **Part 3: Deliverables**

### **Screenshot 1: The Visual XOR**

- **Requirement:** Take a **Full Desktop** screenshot of the web page showing all three grids.
    
- **Markup:**
    
    - The tool automatically puts a Red Border around the Key and a Yellow Border around the Ciphertext. Just ensure these are visible.
        

### **Analysis Questions**

1. **Observation:** Look at the **Ciphertext (C)** grid. Can you clearly see your letter, or does it look like random noise? What desirable properties of ciphertexts can we infer from this?
    
2. **Critical Thinking:** In this tool, `Black + Black = White`. If you were using physical transparency sheets (overhead projector sheets), `Black + Black` would equal **Black**. Why does this make physical transparencies unsuitable for a true XOR One-Time Pad?
    

---

---

# **LAB 2 (VARIANT C): THE BIT-FLIP ATTACK**

**Topic:** Stream Ciphers, Malleability, & Lack of Integrity.

**Note:** Demonstrates how an attacker can forge a check without knowing the key.

---

## **Part 1: Student Identity Parameters (SIP)**

1. **Message:** The string `"Pay $100"`.
    
2. **Key:** Your **Student ID** (e.g., `80123456...`) repeated until it matches the length of the message.
    

---

## **Part 2: The Experiment**

### **Task A: Encrypt**

1. Convert `"Pay $100"` to binary (ASCII).
    
2. XOR it with your Key (using the first 8 digits of your ID) to get the **Ciphertext**.
    
    - _Example:_ If ID is `801234567`, use `8` against `P`, `0` against `a`, `1` against `y`, etc.
        

### **Task B: The Attack (Bit Flipping)**

1. Locate the binary byte in the Ciphertext that corresponds to the character `'1'` in the message. (This is the 6th character).
    
2. We want to change `'1'` into `'9'` without knowing the Key.
    
3. **Calculate the XOR difference:**
    
    - Binary for `'1'` (ASCII 49): `00110001`
        
    - Binary for `'9'` (ASCII 57): `00111001`
        
    - Difference ($1 \oplus 9$): `00001000` (This is the "Flip Mask").
        
4. **Apply the Mask:** XOR the 6th byte of your **Ciphertext** with `00001000`.
    

### **Task C: Decrypt**

1. Take your **Modified Ciphertext** and XOR it with the original Key (Your ID).
    
2. Convert to Text. It should now read `"Pay $900"`.
    

---

## **Part 3: Deliverables**

### **Screenshot 1: The Attack Logic**

- **Requirement:** Screenshot of a text file or script output showing the math.
    
- **Markup:**
    
    - **Red Box** around the "Flip Mask" (`00001000`).
        
    - **Yellow Highlight** over the final decrypted text `"Pay $900"`.
        

### **Analysis Questions**

1. **Concept:** You successfully forged a message ("Pay $900") without ever knowing the Key. What security property does the One-Time Pad **fail** to provide? (Confidentiality, Integrity, or Availability?)
    
2. **Defense:** How could we prevent this attack? (Hint: Lecture 4 discusses "MACs").
    

---

---

# **MARKING KEY: LAB 2**

### **Quick Plagiarism Check**

- **SIP Check:**
    
    - **Lab 2:** Does the "Key" binary row match their Student ID digits? (e.g., ID `8...` $\rightarrow$ ASCII `00111000`).
        
    - **Variant B (Visual):** Does the Key Grid match their ID? (The tool generates this automatically; check if the ID input box in the screenshot matches the filename format `LastName_FirstName_Lab2`).
        
    - **Variant C:** Is the Key derived from their ID?
        
- **Math Check:** Spot check one byte.
    
    - $1 \oplus 1 = 0$
        
    - $1 \oplus 0 = 1$
        

### **Answer Key (Analysis Questions)**

**Lab 2 (Standard)**

- **Q1:** No, the ciphertext should look random.
    
- **Q2:** A Student ID is **Fixed** (not random) and **Reused**. This violates the core rules of OTP (Key must be truly random, Key must never be reused).
    

**Variant B (Visual)**

- **Q1:** No, Grid C should look like random noise.
    
- **Q2:** Physical transparencies act like `OR` logic (Black+Black=Black). Crypto uses `XOR` (Black+Black=White). You cannot do a true OTP with standard transparency sheets because you cannot "unshade" a region by adding more shade.
    

**Variant C (Bit-Flip)**

- **Q1:** It fails to provide **Integrity** (or Authenticity).
    
- **Q2:** We need a **Message Authentication Code (MAC)** or a Digital Signature.