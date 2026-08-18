# ITIS 6200/8200 Lab Repo

Welcome! This repo has everything you need for the labs in *Principles of Information Security and Privacy* (UNC Charlotte, Dr. Jian Xiang) — the graduate ITIS 6200 / ITIS 8200 section. If you're in the undergraduate ITIS 3200 section, you want [UNCC-FORTRESS/itis-3200-f26](https://github.com/UNCC-FORTRESS/itis-3200-f26) instead.

## The labs

| Lab | Topic | Tooling |
|---|---|---|
| `Lab00_Dummy/` | SIP (Student Identity Parameter) mechanic, submission-format drill | CyberChef |
| `Lab01_Cryptography/` | Classical ciphers, AES avalanche effect | Custom HTML tools |
| `Lab02_BlockCiphers/` | ECB/CBC/CTR modes, two-time-pad attack | [BlockCipherModes.html](https://uncc-fortress.github.io/itis-6200-f26/Lab02_BlockCiphers/tools/BlockCipherModes.html) (required) + `aesVisual.py` (optional, source-only) |
| `Lab03_AuthEncryption_DH_MITM/` | Authenticated-encryption scheme analysis, Diffie-Hellman + PRNG rollback, Mallory MITM programming assignment | CyberChef + Python (`Lab03DHProgram.py`) |
| `Lab04_RSA_Certs_PasswordSalting/` | RSA encrypt/sign/verify, hybrid encryption, certificate chain-of-trust theory, password-salting timing simulation | CyberChef + a Colab notebook |
| `Lab05_AccessControl_BLP/` | Bell-LaPadula access control, web origins/same-origin policy, cookies/CSRF theory | Python (`BLP.py`/`Cases.py`) |
| `Lab06_DVWA_PenTest/` | Hands-on CSRF, SQL injection, reflected + stored XSS exploitation | DVWA in a VM |
| `Lab07_NetworkTrafficAnalysis/` | ARP, TELNET vs. SSH, TCP handshake, protocol statistics, Python sockets | Wireshark + NetLab (external, see below) + `server.py`/`client.py` |
| `Lab08_MemorySafety/` | Buffer overflow, shellcode placement, exploit construction, mitigations | [memsafety.html](https://uncc-fortress.github.io/itis-6200-f26/memsafety.html) |

Each lab folder has:
- **`LabNN.md`** — the actual handout: steps, deliverables, analysis questions.
- **`original_docs/`** — the original source document, in case a diagram, equation, or screenshot doesn't come through cleanly in the Markdown version. Treat it as the tiebreaker if anything here looks ambiguous.
- **`LabNN_Rubric.md`** — the exact point breakdown for every part of the lab (screenshots, each analysis question, everything), published openly so you know exactly what you're being graded on before you submit.

## Grading & policy

- **`guidelines.txt`** (or `LabGuidelines.docx`) — submission format, screenshot/markup rules, AI-use policy, and academic-integrity policy. Applies to every lab; not restated per-lab.
- **`basicGradingRubric.md`** — the general Format/Completion/Analysis point categories and late-penalty scale that sit on top of each lab's own rubric.
- **`SCREENSHOT_PENALTY_POLICY.md`** — how a missing screenshot affects your max possible score. Worth reading before you submit.

## Extra material (optional, not graded)

- **`archive/`** — tools built for earlier versions of some labs that ended up not being used. Kept because they're solid, and several are good starting points for a course project. See `archive/README.md`.
- **`Lab09_XSS_SQLi/`, `Lab10_NetworkSecurity/`, `Lab11_IDS/`, `Lab12_MemoryVulns/`, `Lab13_SystemExecution/`** — fully built bonus labs outside the graded Lab00-08 sequence. Good extra practice, or a starting point for a project.
- **`LabMD/`** — an earlier draft of the lab sequence, kept for reference.
- **`memsafety.html`** — the standalone tool Lab 08 uses.

## Tool pages

Browser-based lab tools are hosted live via GitHub Pages at `https://uncc-fortress.github.io/itis-6200-f26/` — every lab links directly to the tool(s) it needs, so you shouldn't have to go looking for these yourself.
