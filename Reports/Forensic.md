# 🌐 Forensic Report of Virtual Machine **3Day** 🔎

## 🎯 Object of Investigation
- **Virtual Machine**: 3Day

## 🏗️ Infrastructure
- **Tasks**:
  1. 🕵️ Find the virus
  2. 🧹 Remove it
  3. 📝 Prepare a detailed report on the task execution for the client

---

## 🔎 Information Discovered During the Investigation

### ⚒️ Process Information
1. **Traffic Dump Analysis**
2. **Log Analysis**
3. **Virus Execution and Subsequent Removal**

### 💻 Traffic Dump
- ✅ Successful attempt ![successpayload](https://github.com/user-attachments/assets/2a116e78-49e2-44bb-a07e-70938f3ba05c) to read the accounts document by accessing files ![etcpasswdinresponse](https://github.com/user-attachments/assets/fd42b864-6ef2-4f1a-b1b1-44ff060f05dc).
- 📥 Uploading the `drop.sh` script to the server ![drop sh](https://github.com/user-attachments/assets/5c0ba2c1-9bc7-405c-b2df-57e7094da702).

### 🦠 Information on Malicious Files
- **drop.sh** launches a script that navigates to the `/tmp` directory and downloads two files: `diamorphine.ko` and `bot` from an external resource.
- **diamorphine.ko** — a rootkit for Linux Kernels 2.6.x/3.x/4.x/5.x/6.x (x86/x86_64 and ARM64), removed using commands ![deactivatediamorphine](https://github.com/user-attachments/assets/a1e3a60d-062f-449a-92fd-deaec38ad8f7).

---

## 🌐 Network Connections Information

### 🌍 Attack Source Addresses
- `37.46.128.71` (whois) ![37 46 128 71](https://github.com/user-attachments/assets/1e48aaef-ebf6-4e90-a93c-5bae613c6f8b)
- `85.140.1.102` (whois) ![85 140 1 102](https://github.com/user-attachments/assets/8ef1c643-5145-4365-9778-413ee0f8e3bb)
- `89.232.113.1` (whois) ![89 232 113 1](https://github.com/user-attachments/assets/8c4e1762-4013-45b3-a555-dcb8eb97537d)

### 🎯 Attack Destination Address
- `10.90.90.116` (whois) ![10 90 90 116](https://github.com/user-attachments/assets/c85a6c3f-725d-43ac-a8e0-d8235cd617a3)

---

## 🔗 Dependencies
- The execution of `drop.sh` (renamed to **ahasi** and hidden in the `/bin` directory) depended on **crontab**, which launched this script upon reboot.
- The script's operation depended on an internet connection. Disconnecting the server from the internet prevented the rootkit from re-entering the system.

---

## 📂 Log Files
- Attempts to perform injections were found in `nginx/access.log`.
- A password brute-force method was detected in `auth.log`.

---

## 📑 Other Data
- An unusual action `reboot /bin/avahi` was noticed in the task scheduler (crontab), causing this file to execute after every reboot ![crontab](https://github.com/user-attachments/assets/423368a1-5992-40e6-8de3-701c8b10c738).
- The contents of the `avahi` file ![avahi](https://github.com/user-attachments/assets/62850f55-ea42-491b-85be-3610fbd1443f) — this is the renamed `drop.sh`.
- Successful access to `/etc/passwd` by the attacker facilitated a brute-force attack.

---

## 🏆 Conclusions
- 🧹 The virus was successfully removed.
- 🕵️ The attack source was identified.
- 📝 A detailed description of the actions taken was prepared for the client.

---

## 🛡️ Security Measures and Protection Recommendations
1. 🛠️ Install Information Security Systems (ISS).
2. 📌 Conduct regular penetration tests.
3. 🔒 Properly configure internal and external infrastructure.
4. 🚀 Perform regular system updates and audits.
