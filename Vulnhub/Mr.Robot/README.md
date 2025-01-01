# Walkthrough 🚀

This walkthrough describes the steps to exploit a vulnerable machine with detailed instructions and visuals. 📘

---

## 🛠️ Steps

### 1. 📄 robots.txt
In the `robots.txt` file, we find the first flag. 🎉

![image](https://github.com/user-attachments/assets/06ed08c5-8b9a-4565-a3f1-adc43b9d01a2)

---

### 2. 🔍 Port Scanning
Perform a port scan to identify open ports and services:

![image](https://github.com/user-attachments/assets/984a42d5-4c56-41a8-99d5-1d9e28b22e7f)

---

### 3. 📂 Login and Password List
Navigate to `http://192.168.31.114/fsocity.dic` and copy the list of logins and passwords.

📌 **Why it matters?** These credentials can be used for brute-forcing accounts.

---

### 4. 🔑 WordPress Login
Go to `http://192.168.31.114/wp-login.php` and find a vulnerability that allows identifying valid logins.

![image](https://github.com/user-attachments/assets/79b2adf4-c419-4510-b263-bd80eaddc475)

📌 **Idea:** Knowing valid logins helps us proceed with password brute-forcing.

---

### 5. 🛠️ Brute-forcing with Hydra
Run `hydra` to brute-force logins:

![image](https://github.com/user-attachments/assets/74291736-3b28-4f88-978c-a9e69c15a2fb)

📌 **Result:** A valid login is found.

![image](https://github.com/user-attachments/assets/d19ba697-6b33-4320-aab9-91c99f866185)

Next, brute-force the password for this login:

![image](https://github.com/user-attachments/assets/33571c3e-4360-4684-9ac6-606f97bec89f)

---

### 6. 🚪 Logging into WordPress
Log into WordPress using the discovered credentials.

![image](https://github.com/user-attachments/assets/ad68fa0c-f342-4c54-9235-a8d7844f8e46)

📌 **Why it matters?** This opens access to the admin panel where malicious code can be uploaded.

---

### 7. 💻 Uploading a Reverse Shell
Upload a reverse shell in the `404.php` file. This file is triggered when a 404 error occurs on the site (missing page).

![image](https://github.com/user-attachments/assets/26829c07-63c2-425f-9694-ee36f40c4072)

Run `nc -nvlp 7777`, trigger the 404 error on the site, and gain a shell:

![image](https://github.com/user-attachments/assets/b71c193d-47af-4a0e-96b6-77fac4c26606)

---

### 8. 📜 Reading Files
Navigate to the `/home/robot` directory and read the file:

![image](https://github.com/user-attachments/assets/f0488140-fa7b-4c4c-ab1b-66e58e3714f2)

📌 **Result:** Obtain a password hash.

Use `crackstation` to decrypt it and get the second key:

![image](https://github.com/user-attachments/assets/8e9367db-a673-4679-b8eb-c5c139ff8dc2)

![image](https://github.com/user-attachments/assets/312a443e-a99e-49f4-b327-c52b17c02a9c)

---

### 9. 🚀 Privilege Escalation
Find files with SUID bits and use the discovered SUID file to escalate privileges to `root`:

![image](https://github.com/user-attachments/assets/05dc599c-fa63-4854-ba4d-073f7a57ea38)

📌 **Why it matters?** This gives complete control over the system.

Read the third flag and complete the exploitation. 🎉

---

### 🎉 Conclusion
Congratulations! You successfully exploited the machine, demonstrating skills in scanning, brute-forcing credentials, uploading shells, and privilege escalation. 🥳
