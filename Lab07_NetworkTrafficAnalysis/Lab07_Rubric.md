# Lab 07 — Grading Rubric

**Total: 96 points** across 7 parts (18+12+12+15+12+12+15). See `../SCREENSHOT_PENALTY_POLICY.md` for the missing-screenshot cap policy, applied on top of this breakdown.

*(Note: earlier drafts of this lab's instructions had mislabeled part-total headers that didn't match their own itemized deliverable+analysis points — fixed to the correct sums below; total is 96, not a round 100.)*

## Part 1: ARP Analysis — 18 pts
Deliverables (12 pts: 4 screenshots × 3 pts — adapter/IP, ping, ARP Request packet, ARP Reply packet). Analysis (6 pts: Q1-Q3, 2 pts each — what ARP is for and why local-only; how an on-path attacker abuses it; what defenses mitigate it).

## Part 2: NetLab Visualization — 12 pts
Deliverables (6 pts: 2 screenshots × 3 pts — topology, event log). Analysis (6 pts: Q4-Q6, 2 pts each — why ARP precedes ICMP; switch broadcast behavior; value of visual simulation).

## Part 3: TELNET Analysis — 12 pts
Deliverables (6 pts: 2 screenshots × 3 pts — packet list, TCP stream). Analysis (6 pts: Q7-Q9, 2 pts each — server IP; per-keystroke packets; why TELNET is insecure).

## Part 4: SSH Analysis — 15 pts
Deliverables (9 pts: 3 screenshots × 3 pts — terminal, payload, negotiation phase). Analysis (6 pts: Q10-Q12, 2 pts each — SSH vs TELNET contents; eavesdropping defense; negotiation steps).

## Part 5: TCP Connection Analysis — 12 pts
Deliverables (6 pts: 2 screenshots × 3 pts — handshake, ACK header). Analysis (6 pts: Q13-Q15, 2 pts each — SYN/ACK purpose; sequence numbers; graceful close).

## Part 6: Protocol Statistics — 12 pts
Deliverables (6 pts: 2 screenshots × 3 pts — protocol hierarchy, conversations). Analysis (6 pts: Q16-Q18, 2 pts each — dominant protocol; value of the Conversations tool; meaning of malformed traffic).

## Part 7: Python Sockets — 15 pts
Deliverables (9 pts: 3 screenshots × 3 pts — code, terminals, Wireshark stream). Analysis (6 pts: Q19-Q21, 2 pts each — what a socket is; why loopback; why a buffer size is needed).
