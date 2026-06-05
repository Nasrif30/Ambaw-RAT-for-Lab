
# AMBAW RAT - Educational Lab Demonstration

**Ambaw** means **Rat** in the Tausug language.

"Like a rat hiding in your device. From a simple PDF open to complete remote control of another computer."

---

## DISCLAIMER

This project is for **EDUCATIONAL PURPOSES ONLY**. Use only in your own lab environment with your own virtual machines. Do not use this on any system without explicit written permission. The author is not responsible for any misuse of this code.

---

## DESCRIPTION

This demonstration shows how a Remote Access Trojan (RAT) can be delivered through a seemingly innocent PDF file. Once the victim opens the PDF, the attacker gains full remote control over the victim's machine without any visible indication.

This is designed for cybersecurity classrooms to help students understand:
- How RATs operate
- The danger of opening untrusted files
- The importance of security awareness

---

## LAB ENVIRONMENT

| Component | Specification |
|-----------|---------------|
| Attacker Machine | Windows WSL (Kali Linux) |
| Victim Machine | Kali Linux Virtual Machine |
| Virtualization | Oracle VirtualBox |
| Network Mode | NAT with Port Forwarding |

---

## HOW TO DOWNLOAD

**Method 1: Clone with Git**

```bash
git clone https://github.com/yourusername/Ambaw-RAT-Lab-Demo.git
cd Ambaw-RAT-Lab-Demo
```

**Method 2: Direct Download**

1. Visit https://github.com/yourusername/Ambaw-RAT-Lab-Demo
2. Click the green "Code" button
3. Select "Download ZIP"
4. Extract the ZIP file to your desired location

**Method 3: Manual Copy**

Create two files on your system:
- ambaw.sh
- ambaw_victim.sh

Copy the contents from this repository into each file.

---

## HOW TO RUN

**Step 1: Make Scripts Executable**

On WSL (Attacker):
```bash
chmod +x ambaw.sh
```

On Kali VM (Victim):
```bash
chmod +x ambaw_victim.sh
```

**Step 2: Configure Port Forwarding in VirtualBox**

1. Open VirtualBox
2. Select your Kali VM
3. Click Settings -> Network
4. Ensure Adapter 1 is set to NAT
5. Click Advanced -> Port Forwarding
6. Add a new rule:
   - Name: AmbawRAT
   - Protocol: TCP
   - Host IP: 127.0.0.1
   - Host Port: 4444
   - Guest IP: 10.0.2.15
   - Guest Port: 4444
7. Click OK

**Step 3: Start the Attacker Listener**

On WSL (Attacker):
```bash
./ambaw.sh
```

Expected output:
```
[+] Your C2 Server IP (Attacker): 10.255.255.254
[+] Listening on port 4444...
[+] Waiting for Ambaw (RAT) to connect...
```

**Step 4: Run the Victim Script**

On Kali VM (Victim):
```bash
./ambaw_victim.sh
```

Expected output:
```
[*] Opening: Confidential_Report_2026.pdf
[!] SECURITY WARNING: Document contains embedded script
[hahahahhahah] AMBAW (RAT) ACTIVATED
[+] Connected to remote C2 server
```

**Step 5: Verify Connection**

On WSL, you should see:
```
connect to [127.0.0.1] from (UNKNOWN) [127.0.0.1] 12345
```

You now have remote control of the Kali VM.

---

## DEMONSTRATION COMMANDS

Once connected, type these commands on the attacker (WSL) terminal:

| Command | Function |
|---------|----------|
| whoami | Display current user on victim |
| pwd | Show current directory on victim |
| ls -la | List files on victim |
| echo "text" > file.txt | Create a file remotely |
| cat file.txt | Read file contents from victim |
| mkdir folder | Create a directory on victim |
| ip a | Show victim network configuration |
| date | Display victim system time |
| exit | Close the RAT connection |

---

## EXAMPLE DEMO FLOW

```bash
whoami
pwd
ls -la
echo "Ambaw RAT was here" > hacked.txt
cat hacked.txt
mkdir RAT_DEMO
ip a
exit
```

---

## HOW IT WORKS

1. The attacker starts a listener on port 4444
2. The victim opens a malicious PDF file
3. The PDF executes a reverse shell script
4. The victim's machine connects back to the attacker using IP 10.0.2.2
5. The attacker gains full remote control

The victim only sees a fake PDF loading screen while the attacker runs commands in the background.

---

## FILES

| File | Purpose |
|------|---------|
| ambaw.sh | Attacker controller script. Displays banner, gets IP address, and starts listener. |
| ambaw_victim.sh | Victim simulation script. Displays fake PDF loading screen and establishes reverse shell. |

---

## TROUBLESHOOTING

| Issue | Solution |
|-------|----------|
| Permission denied | Run chmod +x on both scripts |
| Connection refused | Check Windows Firewall rules for port 4444 |
| No connection established | Verify port forwarding in VirtualBox |
| Ping fails | Ensure VM network is set to NAT mode |
| Address already in use | Change to a different port number |

**Windows Firewall Rule**

Run PowerShell as Administrator:
```powershell
New-NetFirewallRule -DisplayName "AmbawRAT" -Direction Inbound -Protocol TCP -LocalPort 4444 -Action Allow
```

**Test Connectivity**

From Kali VM, ping the Windows host:
```bash
ping 10.0.2.2
```

---

## EDUCATIONAL OBJECTIVES

After this demonstration, students should understand:

1. How easily a simple file can compromise a system
2. The concept of reverse shells and how they bypass firewalls
3. Why they should never open files from untrusted sources
4. The importance of keeping software and antivirus updated
5. How attackers maintain stealth during an attack

---

## SECURITY RECOMMENDATIONS

To protect against RAT attacks:

1. Do not open PDFs or images from unknown senders
2. Disable JavaScript in PDF readers when possible
3. Keep your operating system and applications updated
4. Use endpoint detection and response (EDR) solutions
5. Verify file sources before downloading
6. Use sandbox environments for suspicious files

---

## SYSTEM REQUIREMENTS

| Requirement | Minimum Specification |
|-------------|----------------------|
| Windows | Windows 10 or 11 with WSL installed |
| WSL Distribution | Kali Linux |
| VirtualBox | Version 6.0 or higher |
| VM RAM | 2GB minimum |
| VM Storage | 20GB free space |

---

## AUTHOR

Alnasrif Haliddin

---

## LICENSE

Educational Use Only. Redistribution and classroom use permitted with proper attribution. Commercial use prohibited.
