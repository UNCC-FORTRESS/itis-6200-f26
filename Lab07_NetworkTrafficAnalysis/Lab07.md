# Lab 07: Network Traffic Analysis (ARP, NetLab, TELNET/SSH, TCP, Python Sockets)

**Ported from the real Spring 2026 Canvas assignment** (`original_docs/Lab07_Original.docx`). This replaced the never-deployed `archive/Lab07_AccessControl/` design (RBAC/ABAC content that actually shipped as part of **Lab 05**, via the BLP model) — see `../archive/README.md`. This is the lab the "NetLab" mid-semester "Packet Visibility Issue" announcements referred to; **NetLab is now confirmed to be the external third-party site https://netlab.thecybersecguru.com** (recovered directly from the assignment's embedded hyperlinks), not anything self-hosted in this repo — treat it as an outside dependency the course doesn't control, and re-verify it's stable before Fall 2026 (see the private `3200-6200-8200-grading-keys` repo's `ta-prep-fall2026/LAB_TOOL_AUDIT.md` item 4). A self-contained fallback with no third-party dependency exists at [NetworkSim.html](https://uncc-fortress.github.io/itis-6200-f26/Lab10_NetworkSecurity/tools/NetworkSim.html) if NetLab breaks again.

**Tools required:** Wireshark (https://www.wireshark.org/download.html), a terminal, Python 3.

## Part 1: ARP Analysis on Your Own Network (18 points)

1. Start Wireshark on your active interface.
2. Find your IP (`ifconfig` on macOS/Linux, `ipconfig` on Windows) and your default gateway.
3. Generate ARP traffic by pinging your gateway (or another local-subnet IP): `ping x.x.x.x`. *(If the ping itself times out, that's fine and expected — many routers/hosts block ICMP; the ARP request still goes out before the ping attempt, so proceed regardless.)*
4. Filter Wireshark on `arp`.

**Deliverables:** terminal screenshot, red box on your active adapter, yellow highlight on your IPv4 address (3 pts). Ping-command screenshot, red box on output, yellow highlight on the replying/timing-out IP (3 pts). ARP Request packet screenshot, red box on the packet-details pane, yellow highlight on the broadcast destination MAC (3 pts). ARP Reply screenshot, red box on Info column, yellow highlight on the returned MAC (3 pts, note: only reachable if the gateway actually replies to ARP — most will, even if ICMP is blocked).

**Analysis:** Q1 (2 pts) what is ARP for, and why only within a local network? Q2 (2 pts) how can an on-path attacker abuse ARP to redirect a victim's traffic? Q3 (2 pts) what defenses mitigate ARP attacks, and how?

## Part 2: Visualizing Packets in NetLab (12 points)

1. Open https://netlab.thecybersecguru.com, wait for it to load.
2. Top-right "Templates & Snapshots" icon → Starter Templates → Load Template under "Starter LAN" (a Router 2911, Switch 2960-24TT, PC-PT, Server-PT).
3. Click the Server-PT, find its IPv4 address.
4. Click the PC-PT, open its terminal, `ping [Server IP]`.
5. Watch the animation: an ARP broadcast first, then ICMP packets flowing directly.

**Deliverables:** topology screenshot, red box on the switch, yellow highlight on its link indicators to PC and Server (3 pts). Event-log screenshot, red box on ping output, yellow highlight on the ARP packet type that had to precede the ping (3 pts).

**Analysis:** Q4 (2 pts) why did an ARP packet have to be sent before the ICMP ping? Q5 (2 pts) what did the switch do with the first ARP request — forward to just the destination, or flood to all connected devices? Q6 (2 pts) how does the visual simulator help you interpret raw Wireshark captures?

## Part 3: TELNET Traffic Analysis (12 points)

1. Start a Wireshark capture on your active interface.
2. `telnet telehack.com 23` (macOS: `brew install telnet` first if needed; Windows: enable the Telnet Client via "Turn Windows features on or off," or use PuTTY).
3. Type your first name + Student ID as one continuous string (e.g. `JaneDoe-987654321`) and press Enter — a "command not found" response is expected and fine, the point is the traffic itself.
4. Exit with `Ctrl+]` then `quit`.
5. Filter `tcp.port == 23`, right-click a packet → Follow → TCP Stream.

**Deliverables:** packet-list screenshot, red box on the row, yellow highlight on telehack's destination IP (3 pts). TCP Stream screenshot showing the reassembled plaintext, red box on the text block, yellow highlight on your name+ID as captured in the clear (3 pts — this is your proof of completion).

**Analysis:** Q7 (2 pts) what's telehack's server IP? Q8 (2 pts) why can individual keystrokes appear as separate TELNET packets? Q9 (2 pts) why is TELNET insecure, and what does it leak?

## Part 4: SSH Traffic Analysis (15 points)

1. Fresh Wireshark capture.
2. `ssh new@sdf.org`.
3. Filter `tcp.port == 22`. Look for `SSHv2` in the Protocol column and "Key Exchange Init" / "Diffie-Hellman" in the Info column for the negotiation phase.

**Deliverables:** terminal screenshot, red box on terminal, yellow highlight on the SSH command (3 pts). Packet-details screenshot, red box on the payload section, yellow highlight on the encrypted gibberish (3 pts). Negotiation-phase screenshot, red box on the packet, yellow highlight on the SSHv2 version string (3 pts).

**Analysis:** Q10 (2 pts) compare SSH vs TELNET packet contents — what's different? Q11 (2 pts) how does SSH defend against eavesdropping that TELNET can't? Q12 (2 pts) walk through the SSH negotiation/key-exchange steps and why they matter.

## Part 5: TCP Connection Analysis (12 points)

1. Filter `tcp`.
2. Pick one of your earlier TELNET or SSH sessions, find its opening `[SYN]`, `[SYN, ACK]`, `[ACK]` sequence.
3. Follow → TCP Stream on one of these packets.

**Deliverables:** three-way-handshake screenshot, red box on all three packets, yellow highlight on the server's `SYN/ACK` response (3 pts). TCP header detail for the final `[ACK]`, red box on the TCP section, yellow highlight on the Acknowledgment Number (3 pts).

**Analysis:** Q13 (2 pts) purpose of SYN/ACK flags in connection setup. Q14 (2 pts) why sequence numbers instead of just sending raw data? Q15 (2 pts) what flag gracefully closes a TCP connection, and what did you observe when you typed `quit`/closed the terminal?

## Part 6: Protocol Statistics and Network Evidence (12 points)

1. Statistics → Protocol Hierarchy.
2. Statistics → Conversations → IPv4 tab.

**Deliverables:** Protocol Hierarchy screenshot, red box on the list, yellow highlight on the highest-percentage Application Layer protocol (3 pts). Conversations screenshot, red box on the list, yellow highlight on the IP you exchanged the most bytes with — the "top talker" (3 pts).

**Analysis:** Q16 (2 pts) which protocol dominated your traffic, and does that match what you actually did? Q17 (2 pts) why is the Conversations view useful when investigating a breach? Q18 (2 pts) what would a large amount of "Unknown"/"Malformed" traffic suggest?

## Part 7: Network Programming with Python Sockets (15 points)

`server.py` and `client.py` in this folder (recovered verbatim from the original assignment; `client.py`'s TODOs are intentionally left unimplemented — that's the student's task, not this port's).

1. Copy/run `server.py`.
2. Complete `client.py`'s TODOs using the official docs (https://docs.python.org/3/library/socket.html): connect to the server, build a message containing your Student ID + the word "robux", send it, receive the reply, print it.
3. Wireshark: capture on the Loopback interface, filter `tcp.port == 65432`.
4. Run `python server.py` in one terminal, `python client.py` in another.
5. Stop the capture, find the exchange, Follow → TCP Stream (client request in red, server response in blue).

**Deliverables:** `client.py` code screenshot, red box on your code, yellow highlight on your `recv()` call (3 pts). Both terminals side-by-side, red box on the client terminal, yellow highlight on the received `TX_ID` hash (3 pts). Wireshark TCP Stream screenshot, red box on the conversation, yellow highlight on the server's approval response (3 pts).

**Analysis:** Q19 (2 pts) what is a socket, conceptually? Q20 (2 pts) why capture on Loopback rather than Wi-Fi/Ethernet for this script specifically? Q21 (2 pts) why do network apps need to define a `recv()` buffer size?

## References & Further Reading

Lab-wise, if you face any difficulties regarding the setup, tool usage, or markup help, please feel free to reach out to the TAs during office hours or via email. All assessments and deductions are done at the discretion of the TAs, but mostly based off of a preset rubric to ensure fairness and consistency — if you have any reservations with how your submission was graded, please raise them within three days of receiving your points.

1.  **Wireshark:** [User's Guide — Getting Started](https://www.wireshark.org/docs/wsug_html_chunked/ChapterIntroduction.html) — general capture/filter mechanics, useful across every part of this lab.
2.  **Ref:** [Address Resolution Protocol (Wikipedia)](https://en.wikipedia.org/wiki/Address_Resolution_Protocol) — background for Part 1's ARP questions.

## AI Appendix & submission format

Follow `../guidelines.txt` — not restated here.
