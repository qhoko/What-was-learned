# Walkthrough 🚀

This walkthrough describes the steps to exploit a vulnerable machine. Each step is documented with detailed instructions, visuals, and explanations. 🌟

---

## 🛠️ Steps

### 1. 🔍 Port Scanning
Perform a port scan to identify open ports and services:

![image](https://github.com/user-attachments/assets/c2ed84f9-36ac-4cc3-8925-b94615f2da33)

📌 **Why it matters:** This provides an overview of the services running on the target machine and potential vulnerabilities.

---

### 2. 📂 Directory Scanning
Scan directories to identify hidden resources:

![image](https://github.com/user-attachments/assets/7b30e84c-65e7-478e-9395-90fa9f1d3afd)

📌 **Why it matters:** Hidden directories might contain critical files or administrative interfaces.

---

### 3. 🛠️ Extracting Information from Source Code
In the source code of the page `192.168.31.123/fristi`, find the following entry:

![image](https://github.com/user-attachments/assets/aa7c847f-3f4e-4cb7-8166-c7287eab03c2)

At the end of the code, locate a Base64-encoded image:

![image](https://github.com/user-attachments/assets/dedb3408-5f64-4a46-b2a2-5518ebc4b891)

📌 **Why it matters:** Source code often contains comments or hidden data that can be exploited.

---

### 4. 🖼️ Decoding the Image
Decode the Base64 string and convert it into an image:

![image](https://github.com/user-attachments/assets/538305f1-b56d-4166-840f-d7adb689b5c8)

From the image, retrieve the password for the `eezeepz` user:

![image](https://github.com/user-attachments/assets/0c047277-478c-4298-838b-f63767804922)

📌 **Why it matters:** Such images often contain encoded or hidden data used for authentication.

---

### 5. 🔑 Logging In
Log in using the credentials:

![image](https://github.com/user-attachments/assets/90c6cec5-d68a-4ab0-8dd2-6642dd0cd899)

📌 **Outcome:** Successful login provides access to new exploitation opportunities.

---

### 6. 💻 Creating and Uploading a Shell
Create a `shell.php.png` file:

![image](https://github.com/user-attachments/assets/d16233a4-821e-4fe3-9a1e-b0e6824c283b)

Upload the shell:

![image](https://github.com/user-attachments/assets/0128fe1b-54a4-4719-a22f-3082b202135a)

📌 **Why it matters:** A shell allows remote access to the machine with minimal restrictions.

---

### 7. 🎧 Establishing a Listener
Enable a listener on your machine:

![image](https://github.com/user-attachments/assets/589a2607-6578-4879-a949-fb998453e671)

Navigate to:

`http://192.168.31.123/fristi/upload.php`

Obtain a reverse shell:

![image](https://github.com/user-attachments/assets/8aedf0df-fb00-4c16-8309-4dbd41866780)

📌 **Why it matters:** A reverse shell provides complete control over the target system.

---

### 8. 📜 Writing and Executing Files
Write a file named `runthis` into the `/tmp` directory:

![image](https://github.com/user-attachments/assets/d150dd0f-a0d2-4568-9781-f3750a93378a)

Move to the `/home/admin` directory:

![image](https://github.com/user-attachments/assets/2732bc53-e2db-4da0-a9bb-91a5771a4829)

Read the `whoisyourgodnow.txt` file to obtain an encoded password for the `fristigod` user:

`=RFn0AKnlMHMPIzpyuTI0ITG`

📌 **Why it matters:** Such files often contain keys or passwords for further progression.

---

### 9. 🔓 Decrypting the Password
Using the `cryptpass.py` script, write your own decryption code:

![image](https://github.com/user-attachments/assets/53be258a-356c-448e-bbe0-b28a30342aff)

Run the code to retrieve the password:

![image](https://github.com/user-attachments/assets/8f08bb8d-277f-42ed-ac98-ad0065e22ed8)

📌 **Outcome:** The decrypted password allows you to log in as a new user.

---

### 10. 🚀 Logging in and Exploiting Misconfiguration
Log in as `fristigod`:

![image](https://github.com/user-attachments/assets/92c35c79-166b-4884-bf66-687be22be984)

Run `sudo -l` to find a misconfiguration:

![image](https://github.com/user-attachments/assets/0fcb147c-fc05-424a-9ce6-5b2231a87f84)

Execute the exploit:

![image](https://github.com/user-attachments/assets/94c3015b-3d25-4606-b35f-c197f828b981)

Gain root access:

![image](https://github.com/user-attachments/assets/af15be46-1fbc-44cd-96ce-6148454ecfcb)

📌 **Why it matters:** Root access provides complete control over the system.

---

### 🎉 Conclusion
Congratulations! You have successfully exploited the machine. This demonstrates your skills in scanning, decoding, and exploiting vulnerabilities. 🥳
