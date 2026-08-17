# Lab 08 — Grading Rubric

**Total: 100 points** (Part 1: 35, Part 2: 65). See `../SCREENSHOT_PENALTY_POLICY.md` for the missing-screenshot cap policy, applied on top of this breakdown — Part 2 has no screenshots (it's a written exercise), so the cap only applies against Part 1's 4 required screenshots.

## Part 1: Stack Overflow & Shellcode Placement — 35 pts
- **Deliverables (20 pts, 5 each):** Step 0 (Exploit Payload mode) screenshot; Step 2 ("At buf start") screenshot; Step 5 (EXPLOITED indicator) screenshot; "Above RIP" placement screenshot. Every screenshot must show the student's own personalized parameters (`BUF_SIZE`, `SHELL_SIZE`, `PAD_CHAR`), not the example values.
- **Q1 (5 pts):** hand-calculated padding-byte count using the student's own `BUF_SIZE`/`SHELL_SIZE`, with arithmetic shown.
- **Q2 (5 pts):** why the RIP address differs between "In buffer" and "Above RIP" placement, and what determines the address written.
- **Q3 (5 pts):** correct explanation of the `movl %ebp,%esp` → `popl %ebp` → `ret` epilogue sequence and why it lets an overwritten RIP redirect execution.

## Part 2: Exploit Construction & Defenses — 65 pts (written, no screenshots)
- **Task A (15 pts):** correct SFP and RIP address calculation from `buf`'s start address and (4-byte-aligned) size, with hex arithmetic shown.
- **Task B (15 pts):** correct padding-byte count using the student's own `TARGET_MSG` and `PAD_CHAR`, with the formula applied correctly.
- **Task C (20 pts):** a correctly-assembled exploit string in Python notation, including the little-endian 4-byte address encoding.
- **Task D (15 pts):** three correctly-named and correctly-explained system/compiler-level defenses (e.g. stack canaries, ASLR, NX/DEP), each tied to the specific attack step it disrupts.

Partial credit throughout Part 2 for correct method/formula with an arithmetic error, versus no credit for an unsupported final answer with no shown work.
