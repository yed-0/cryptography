# 🔐 Network Protocol Vulnerability Lab Walkthrough

**Name:** Syed Muhammad Sajjad 

**Target Machine:** Metasploitable 2 (`192.168.100.131`)  


---

## 🎯 Objective
Explore vulnerabilities in common protocols (FTP, TELNET, SSH, HTTP) via brute-force attacks and network sniffing. Analyze security flaws and propose mitigations.

---
First of all get the ip for our victim which is metasploitable 2.

![image](https://github.com/user-attachments/assets/d5a91d2f-1ef7-49ac-b70b-bca53901c591)

use ip a command.

so here our target ip is 192.168.100.131
## 🔮 1. Enumeration

### Tool Used:
- Nmap

### Command:
```bash
nmap -sS -sV 192.168.100.131
```
use this command on kali :)

![image](https://github.com/user-attachments/assets/a5b52866-3776-4fa3-b9d2-ecf55a82b130)


### Discovered Open Ports & Services:
- FTP (21)
- SSH (22)
- TELNET (23)
- HTTP (80)
- Others (Samba, MySQL, etc.)

we have to get the username in order to launch attack.
So here we can use samba port to get it ><


WHAT IS SAMBA PORT?
-----------------------------------------------------------
Samba is an open-source implementation of theSMB (Server Message Block) protocol
used for file and printer sharing between systems, especially between Linux and Windows.

HOW TO USE IT?
----------------------
enum4linux -U 192.168.100.131 

![image](https://github.com/user-attachments/assets/fa5186a8-b64e-4795-bd1b-4e2c80ba1b48)


### Usernames Found:
From enum4linux :
- msfadmin
- postgres
- root
- ftp
- telnetd
- user

---

## 🔧 2. Brute Force Attacks

### ✅ FTP
**Tool:** Medusa  
**Command:**
```bash
medusa -h 192.168.100.131 -u msfadmin -P passwords.txt -M ftp
```
we need to create password.txt which will be list of password we use to bruteforce.

![image](https://github.com/user-attachments/assets/232bdb41-7f2b-4019-96e6-6f546d35f3eb)

![image](https://github.com/user-attachments/assets/d7ae1f3a-2b78-445f-896f-8ec37ee58103)

now we can use the command to bruteforce ftp port :)

![image](https://github.com/user-attachments/assets/259ff710-ce32-44a7-bd5d-62311701752f)

YEAYYY!!!
### ✅ TELNET
**Tool:** Hydra

**Command:**
```bash
hydra -l msfadmin -P passwords.txt 192.168.100.131 telnet
```

as Hydra is often more reliable with Telnet we will use it.

![image](https://github.com/user-attachments/assets/913775ce-e25f-4035-9301-504aa03408ed)

we can see from the result that the valid password is "msfadmin" ><

### ✅ SSH
**Tool:** NetExec  
**Command:**
```bash
nxc ssh 192.168.100.131 -u username.txt -p passwords.txt
```
SSH Brute Force using NetExec
✅ Step 1: Create a new list for username and save it as username.txt

![image](https://github.com/user-attachments/assets/02d161e2-cb7d-4c88-b18e-19cd328daea4)


now we can use the command to bruteforce SSH  >< :)


![image](https://github.com/user-attachments/assets/7ef51a40-a716-428a-923a-f4c446d8e599)


![image](https://github.com/user-attachments/assets/89f83e58-970f-4b18-b4cd-ad16819a6f7f)





### ✅ HTTP Login (DVWA)
**Tool:** Burp Suite Intruder
- Target: `http://192.168.x.x/dvwa`
- Payload: Use common user/password list
- Result: [Successful login screenshot]



