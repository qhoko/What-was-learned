# PwnLab Walkthrough 🚀

This walkthrough describes the steps to exploit the PwnLab machine on Vulnhub. Each step is documented with screenshots to make the process clear and replicable. 📘

## 🛠️ Steps

### 1. Port Scanning
Perform a port scan to identify open ports and services:

![image](https://github.com/user-attachments/assets/9931d39b-9214-4fcf-8bea-084b6d681db5)

---

### 2. Accessing the Target
Access the target URL with a specific payload:

`http://<ip>/?page=php://filter/convert.base64-encode/resource=config`

![image](https://github.com/user-attachments/assets/60118c58-6fc7-44b9-9334-16fa11f526da)

---

### 3. Decoding Base64 Data
Decode the obtained data to reveal sensitive information:

![image](https://github.com/user-attachments/assets/443a9bf9-504c-448d-928a-8b747158b0ac)

---

### 4. Extracting Database Information
Retrieve the content of the `Users` table from the database:

![image](https://github.com/user-attachments/assets/3782dcfe-a1d3-4b17-8d94-a93fe2786068)

---

### 5. Crafting Payload
Generate a payload using `msfconsole` for exploitation:

![image](https://github.com/user-attachments/assets/e378db62-7abb-4c66-a1fe-21825ef3a6b7)

Convert the payload into a `.gif` file for further use:

![image](https://github.com/user-attachments/assets/8e5eea5a-8bbc-464f-8ea4-2335e65eed39)

---

### 6. Executing Payload via Cookie Modification
Execute the payload by altering the `cookie` header:

![image](https://github.com/user-attachments/assets/38b2ee46-983e-4883-92e4-31799aa200be)

---

### 7. Privilege Escalation
Switch to user `kane` using the identified password:

![image](https://github.com/user-attachments/assets/5fb07111-ef81-4b5a-aa57-29a9d014d821)

Modify the requested path to utilize built-in binaries for privilege escalation:

![image](https://github.com/user-attachments/assets/0e7dcddc-c757-47a3-b572-a0be9c10367a)

Create a custom `cat` file for further steps:

![image](https://github.com/user-attachments/assets/ee47764a-a79e-4b8d-81b3-c2a65c5e834a)

---

### 8. Root Access
Run `msgmike` to proceed with exploitation:

![image](https://github.com/user-attachments/assets/9df8a414-1e76-48c6-bc6c-53cbd607e57e)

Execute `msg2root` to gain root privileges:

![image](https://github.com/user-attachments/assets/2dc3e832-5893-4800-826b-d64b920b5f41)

---

### Conclusion
Congratulations! You've successfully exploited the PwnLab machine. 🎉
