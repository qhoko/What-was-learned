# 🌐 Lab Work: Using Path Matching for Web Cache Deception 😎

---

## 📜 Description
In this laboratory work, we will learn to exploit discrepancies between the web cache and the origin server to obtain the API key for user **carlos**.

---

## 🎯 Objectives
- 👉 Get acquainted with path matching and its use for resource access.
- 👉 Explore the technical aspects of web caching and its bypassing possibilities.

---

## 🔑 Credentials
To log into the account, use the following credentials:
- **Username:** `wiener`
- **Password:** `peter`

---

## 📚 Theoretical Foundation

### 🛤️ URL Path Matching
- 🔍 **API endpoints** may use regular expressions to match URL paths to resources.
- 🔍 It is important to understand how different paths can be handled differently depending on cache and server configurations.

### 🔄 Caching and Its Discrepancies
- 🔄 Web cache and server may have different mechanisms for processing the same requests.
- 🔄 Bypassing cache is done by altering the request path to access uncached data on the server.

---

## 📋 Steps to Perform

### 🔐 1. Logging In
Log in using the provided credentials to see your **API key**:
![API Key](https://github.com/user-attachments/assets/a1cecc2a-c2b2-47dc-8f05-37fb9b5611a3)

---

### 🔍 2. Analyzing Endpoints
Explore the available API endpoints to determine how they are handled by the server.
1. 🔍 **Pay attention to paths** that may be vulnerable to bypassing. Launch **Burp Suite** and capture the request by refreshing the page at `/my-account`.
2. ✉️ **Send the request to Repeater** and change the path to an arbitrary one, for example, to `/my-account/eyes`:
   ![Change Path](https://github.com/user-attachments/assets/4cb281a1-eb8d-4967-ae98-adb9e89ea9b8). Note the 404 error.
3. Remove the arbitrary segment and add an arbitrary string to the original path. For example, change the path to `/my-accounteyes`:
   ![image](https://github.com/user-attachments/assets/fcf4a0a5-0b38-4d11-8b19-20062a4b4ee3). This will serve as a reference material.

---

### 🔄 3. Using Delimiters in Intruder
Let's investigate how delimiters affect website behavior.
1. Right-click the request and select "Send To Intruder" (or use Ctrl + I).
2. 📄 **Specify the payload location**:
   ![image](https://github.com/user-attachments/assets/f6128369-516e-44b4-a804-e145f593ff38).
3. Uncheck "URL-encode this characters."
4. 📊 **Add a list of characters**: In the "Payload Configuration" section, add a list of characters that can be used as delimiters. 
   Copy the list from here: [Character List](https://portswigger.net/web-security/web-cache-deception/wcd-lab-delimiter-list).
   ![image](https://github.com/user-attachments/assets/f109678c-1d5f-4604-8970-97562d595b7c)
5. Click "Start attack."
   In the output, you will see a code 200, indicating that only 2 delimiters are processed: "?" and ";":
   ![image](https://github.com/user-attachments/assets/702f4890-8100-41fb-904c-e6626e919aeb).

---

### 🛡️ 4. Exploring Path Delimiter Mismatches
1. 🔄 **Send a request with ";" in Repeater**:
   ![image](https://github.com/user-attachments/assets/49a5beb0-c8d6-4f9b-84f2-b08446cc9e81). The response shows an X-Cache header: miss.
2. Resend the request and check that the value of the X-Cache header changes to hit. This indicates that the cache does not use ";" as a path delimiter.

---

### 🗝️ 5. Obtaining the API Key
Determine the API key for user **carlos**.
1. 🛠️ On the exploit server, create an exploit that redirects victim `carlos` to a malicious URL:
2. 🚀 Click "Deliver the exploit to the victim." When the victim views the exploit, the response will be stored in the cache.
3. 🌐 Navigate to the URL specified in your exploit:  
   `https://YOUR.LAB.LINK.PART/my-account;eyes.js`.
4. 🔑 The response includes the API key for **carlos**:  
   ![API Key for carlos](https://github.com/user-attachments/assets/6e9c4f5b-5c3c-4ba3-a2e0-39685da71337).

---

## 💡 Conclusion
As a result of completing this laboratory work, you will be able to access protected resources using path matching and cache bypassing skills. This knowledge will prove valuable in your future projects and career. 🚀
