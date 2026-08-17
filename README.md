# ITIS 6200/8200 — Lab Starter Repo

Starter code and lab handouts for "Principle of Information Security and Privacy" (ITIS 6200 graduate / ITIS 8200 PhD combined section, UNC Charlotte, Dr. Jian Xiang). This is the graduate repo — the undergraduate ITIS 3200 section has its own repo at [UNCC-FORTRESS/itis-3200-f26](https://github.com/UNCC-FORTRESS/itis-3200-f26), forked from the same Fall 2026 starter material. This README reflects the **Fall 2026 reorganization**: every `LabNN_*` folder at the top level now matches, in both number and content, what actually ran as that lab in Spring 2026 (verified directly against the real Canvas assignment files, not just the earlier tool designs built for each slot).

**This repo is student-facing only** — labs, tools, and an open point-value rubric per lab. TA/grading material (answer keys, course audit, project catalog) lives in a separate private repo.

## Canonical labs (Lab00-08 — what actually shipped)

| Lab | Topic | Tooling |
|---|---|---|
| `Lab00_Dummy/` | SIP (Student Identity Parameter) mechanic, submission-format drill | CyberChef |
| `Lab01_Cryptography/` | Classical ciphers, AES avalanche effect | Custom HTML tools |
| `Lab02_BlockCiphers/` | ECB/CBC/CTR modes, two-time-pad attack | `BlockCipherModes.html` + `aesVisual` |
| `Lab03_AuthEncryption_DH_MITM/` | Authenticated-encryption scheme analysis, Diffie-Hellman + PRNG rollback, Mallory MITM programming assignment | CyberChef + Python (`Lab03DHProgram.py`) |
| `Lab04_RSA_Certs_PasswordSalting/` | RSA encrypt/sign/verify, hybrid encryption, certificate chain-of-trust theory, password-salting timing simulation | CyberChef + a Colab notebook |
| `Lab05_AccessControl_BLP/` | Bell-LaPadula access control, web origins/same-origin policy, cookies/CSRF theory | Python (`BLP.py`/`Cases.py`) |
| `Lab06_DVWA_PenTest/` | Hands-on CSRF, SQL injection, reflected + stored XSS exploitation | DVWA in a VM |
| `Lab07_NetworkTrafficAnalysis/` | ARP, TELNET vs. SSH, TCP handshake, protocol statistics, Python sockets | Wireshark + NetLab (external, see below) + `server.py`/`client.py` |
| `Lab08_MemorySafety/` | Buffer overflow, shellcode placement, exploit construction, mitigations | `memsafety.html` (repo root) |

Each lab folder now includes a clean `LabNN.md` (this port), an `original_docs/` copy of the real source `.docx` (equation diagrams and reference screenshots don't survive markdown conversion — treat the original as authoritative for anything ambiguous in the port), and a `LabNN_Rubric.md` — the full point breakdown for every question/section/subpart, published openly so you know exactly what you're being graded on. `LabNN_Rubric.md` intentionally does not include answers; that's kept separately by the teaching team.

## `archive/` — designs that don't match what shipped

Nine lab folders were built for a lab-number slot that ultimately ran different content (or, in two cases, lost out to a richer sibling design that also never shipped). Nothing is deleted — several are cited as optional starter material for Fall 2026 course projects. See `archive/README.md` for exactly why each one is there.

## Built but never deployed as a numbered lab (still at top level, no conflict)

`Lab09_XSS_SQLi/`, `Lab10_NetworkSecurity/`, `Lab11_IDS/`, `Lab12_MemoryVulns/`, `Lab13_SystemExecution/` — Canvas never assigned a Lab 09 or beyond in Spring 2026, so these don't collide with anything real. They're fully built, well-designed, and available as optional starter material for course projects.

## Grading & policy (public — no answers)

- `guidelines.txt` / `LabGuidelines.docx` — submission format, screenshot/markup rules, AI-use policy, academic-integrity policy. Applies to every lab above; not restated per-lab.
- `basicGradingRubric.md` — the generic Format/Completion/Analysis point-category breakdown + late-penalty scale that applies on top of each lab's own `LabNN_Rubric.md`.
- `SCREENSHOT_PENALTY_POLICY.md` — how missing screenshots cap your maximum achievable score on an assignment. Read this before you submit.

## Other top-level material

- `memsafety.html` — the standalone tool Lab 08 uses.
- `LectureNotes/`, `LectureSchedule.pdf`, `Assignments/` (Homeworks 1-4) — lecture and homework material, unaffected by this reorganization.
- `LABDOCS/`, `LabMD/` — two earlier generations of the lab sequence (CyberChef-only, pre-dating the custom-tool build-out). Superseded, kept for history.

## GitHub Pages

Live at `https://uncc-fortress.github.io/itis-6200-f26/` (e.g. `memsafety.html` is reachable at `https://uncc-fortress.github.io/itis-6200-f26/memsafety.html`). The Spring 2026 predecessor repo's assignment linked directly to `https://oatkrs.github.io/ITIS-6200/memsafety.html`, which is how we know this pattern is what Lab 08 actually depends on.
