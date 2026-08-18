# Lab 08: Memory Safety Vulnerabilities

**Tool:** [Memory Safety Lab](https://uncc-fortress.github.io/itis-6200-f26/memsafety.html) — you can also open `memsafety.html` directly in a browser without needing the live link.

## Your Personalized Parameters

Derive these from your Student ID (digits `D1`-`D9`) and first name — your grader checks that your screenshots match your own values, so using the example ID below will score zero:

| Parameter | Formula | Example (ID 801234567) |
|---|---|---|
| `BUF_SIZE` | 12 + (D8 × 4) | 12 + (6×4) = 36 |
| `SHELL_SIZE` | 8 + (D5 + D6) | 8 + (3+4) = 15 |
| `PAD_CHAR` | first letter of your first name, uppercase | Jane Doe → `J` |
| `FMT_OFFSET` | D3 + D4 | 1 + 2 = 3 |
| `INT_VAL` | (D1+D9) even → `-1`; odd → `0xFFFFFFFE` | 8+7=15 (odd) → `0xFFFFFFFE` |
| `TARGET_MSG` | your full name + last 4 digits of your Student ID | `JaneDoe4567` |

## Part 1: Stack Overflow & Shellcode Placement

**Objective:** configure the Stack Overflow simulator with your parameters, trace how an overflow overwrites the RIP, and see what happens when shellcode must sit outside the buffer.

1. Open the [Memory Safety Lab tool](https://uncc-fortress.github.io/itis-6200-f26/memsafety.html), go to the "Stack Exploit" tab.
2. Click "Exploit Payload" mode. Set the Buffer slider to `BUF_SIZE`, Shellcode slider to `SHELL_SIZE`, Pad char field to `PAD_CHAR`.
3. Select "At buf start" placement. Step through Steps 0-5, watching which memory cells change and where ESP/EBP/EIP point at each stage.
4. Reset, switch to "Above RIP" placement, set shellcode size to `BUF_SIZE + 8` (won't fit in the buffer). Step through again.
5. Reset, click "Normal Input" mode, step through to see a non-malicious input stay in bounds with RIP untouched.

**Deliverables:** Step 0 screenshot (Exploit Payload mode), red box on Payload Builder, yellow highlight on Buffer/Shellcode slider values (5 pts). Step 2 screenshot ("At buf start"), red box on the RIP memory cell, yellow highlight on the 4-byte shellcode address written there (5 pts). Step 5 screenshot showing the EXPLOITED indicator, red box on registers, yellow highlight on EIP pointing at the shellcode address (5 pts). "Above RIP" Step 2 screenshot, red box on the stack diagram, yellow highlight on the SHELLCODE rows above RIP and the new RIP value (5 pts).

**Analysis:**
- Q1 (5 pts): using your specific `BUF_SIZE`/`SHELL_SIZE`, calculate by hand the garbage padding bytes needed (including the 4 bytes overwriting the SFP) when shellcode sits at the buffer start. Show your arithmetic.
- Q2 (5 pts): the RIP address changes between "In buffer" and "Above RIP" placement — why, and what determines the address the attacker actually writes?
- Q3 (5 pts): the function epilogue runs `movl %ebp, %esp` → `popl %ebp` → `ret`. Explain what each instruction does to ESP/EBP/EIP, and why this sequence lets an overwritten RIP become the new instruction pointer.

## Part 2: Exploit Construction & Defenses (written, no screenshots)

Consider:
```c
void vulnerable(void) {
    char buf[N];   // N = your BUF_SIZE
    gets(buf);
}
int main(void) {
    vulnerable();
    return 0;
}
```
Assume `buf` starts at stack address `0xbfffcd40` and has size `BUF_SIZE`.

- **Task A:** calculate the memory addresses of the SFP and the RIP, given `buf`'s start address and size. Remember the compiler rounds `BUF_SIZE` up to the nearest multiple of 4 for alignment. Show hex arithmetic.
- **Task B:** your `TARGET_MSG` is the "shellcode," placed at the buffer start. Calculate the padding bytes needed after `TARGET_MSG` to fill `buf[]` and reach the SFP, using `PAD_CHAR` as filler: `padding = (aligned BUF_SIZE) + 4 - len(TARGET_MSG)`.
- **Task C:** write the full exploit input string in Python notation: `TARGET_MSG + PAD_CHAR * padding + address`, with the address (pointing at `buf`'s start, `0xbfffcd40`) written little-endian as four `\xNN` bytes.
- **Task D:** name and explain three system/compiler-level defenses against buffer overflow exploits (e.g. stack canaries, ASLR, NX/DEP). For each, say what it does and which specific step of the attack above it disrupts.

Show all arithmetic and conversions — partial credit is given for correct intermediate steps.

## References & Further Reading

Lab-wise, if you face any difficulties regarding the setup, tool usage, or markup help, please feel free to reach out to the TAs during office hours or via email. All assessments and deductions are done at the discretion of the TAs, but mostly based off of a preset rubric to ensure fairness and consistency — if you have any reservations with how your submission was graded, please raise them within three days of receiving your points.

1.  **Paper:** [Smashing the Stack for Fun and Profit (Aleph One, Phrack #49)](https://phrack.org/issues/49/14.html) — the classic explanation of how stack layout and the RIP overwrite mechanism this lab explores actually works.
2.  **Ref:** [Calling Convention (Wikipedia)](https://en.wikipedia.org/wiki/Calling_convention) — background for Q3's function-epilogue question.

## AI Appendix & submission format

Follow `../guidelines.txt` — not restated here.
