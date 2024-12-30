# 🪟 Windows PrivEsc - Lab Description

## 📜 Description
In this lab, you will learn privilege escalation techniques in Windows by exploiting vulnerabilities and misconfigurations in a specially designed virtual machine. The lab provides multiple ways to gain administrator or SYSTEM privileges using different exploitation methods.

## 🎯 Objectives

### 1. Deploying a Vulnerable Windows VM
Set up a virtual machine with a vulnerable version of Windows to complete further privilege escalation tasks.

### 2. Generating a Reverse Shell Executable
Create an executable file that will establish a reverse connection to your terminal, allowing remote control of the system.

### 3. Service Exploitation - Insecure Service Permissions
Investigate and exploit vulnerabilities related to improperly configured service permissions, which could allow you to execute arbitrary code as a system user.

### 4. Service Exploitation - Incorrect Service Path (Unquoted Service Path)
Discover and exploit vulnerabilities in incorrectly specified service paths that can be used to execute malicious code.

### 5. Service Exploitation - Weak Registry Permissions
Investigate vulnerabilities in registry permissions that could allow you to make changes to the system and elevate your privileges.

### 6. Service Exploitation - Insecure Service Executables
Examine service executables that may be susceptible to replacement, enabling malicious code to run with elevated privileges.

### 7. Registry Vulnerabilities - AutoRuns
Analyze AutoRuns via the registry to identify programs and scripts that run automatically during system boot.

### 8. Registry Vulnerabilities - AlwaysInstallElevated
Study the "AlwaysInstallElevated" configuration, which allows installing programs with administrator rights and may be used for privilege escalation.

### 9. Passwords - Registry
Learn to find passwords and credentials stored in the registry, which can grant access to critical system resources.

### 10. Passwords - Saved Credentials
Work with saved credentials to analyze potential vulnerabilities and gain access to user accounts.

### 11. Passwords - Security Account Manager (SAM)
Study the SAM database to extract password hashes and use them to gain access to the system.

### 12. Passwords - Pass-the-Hash
Learn pass-the-hash techniques that allow authentication on systems without needing to enter plaintext passwords.

### 13. Task Schedulers
Explore vulnerabilities in task schedulers that could be used to execute malicious applications with administrator privileges.

### 14. Vulnerable GUI Applications
Identify vulnerable graphical user interfaces (GUIs) that can be exploited for privilege escalation on the target system.

### 15. Autoload Applications
Analyze applications that run automatically during system boot and search for vulnerabilities to exploit.

### 16. Token Impersonation - Rogue Potato
Use the Rogue Potato tool to impersonate tokens and elevate privileges within the system.

### 17. Token Impersonation - PrintSpoofer
Apply PrintSpoofer to obtain elevated tokens and execute malicious operations.

### 18. Privilege Escalation Scripts
Study and use various scripts that assist in the privilege escalation process, simplifying the exploitation of vulnerabilities.

---

## 🔍 Additional Information
- **Room Type:** Free, anyone can deploy the virtual machine in the room without a subscription.
- **Available Credentials:** 
  - `user: password321`

---

## 💡 Conclusion
This lab will provide you with the necessary knowledge and practical experience for privilege escalation in Windows. You will learn to identify and exploit vulnerabilities in Windows systems, which is valuable for cybersecurity and penetration testing.
