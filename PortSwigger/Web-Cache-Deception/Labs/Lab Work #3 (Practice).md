# 🌐 Lab Work: Using Server Normalization for Web Cache Deception 😎

---

## 📜 Description
In this lab work, we will learn to exploit normalization discrepancies between the web cache and the origin server to obtain the API key for user **carlos**.

---

## 🎯 Objectives
- 👉 Familiarize yourself with path matching and its use for resource access.
- 👉 Explore the technical aspects of web caching and possibilities for bypassing it.

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
2. ✉️ **Send the request to Repeater** and change the path to an arbitrary one, for example `/my-account/eyes`:
   ![Change Path](https://github.com/user-attachments/assets/4cb281a1-eb8d-4967-ae98-adb9e89ea9b8). Note the 404 error.
3. Remove the arbitrary segment and add an arbitrary string to the original path. For example, change the path to `/my-accounteyes`:
   ![image](https://github.com/user-attachments/assets/fcf4a0a5-0b38-4d11-8b19-20062a4b4ee3). This will serve as reference material.

---

### 🔄 3. Using Delimiters in Intruder
Let's investigate how delimiters affect website behavior.
1. Right-click on the request and select "Send To Intruder" (or use Ctrl + I).
2. 📄 **Specify the payload location**:
   ![image](https://github.com/user-attachments/assets/f6128369-516e-44b4-a804-e145f593ff38).
3. Uncheck "URL-encode this characters."
4. 📊 **Add a list of characters**: In the "Payload Configuration" section, add a list of characters that can be used as delimiters. 
   Copy the list from here: [Character List](https://portswigger.net/web-security/web-cache-deception/wcd-lab-delimiter-list).
   ![image](https://github.com/user-attachments/assets/f109678c-1d5f-4604-8970-97562d595b7c)
5. Click "Start attack." 
   In the output, you will see a code 200, indicating that only 1 delimiter "?" is processed: 
   ![image](https://github.com/user-attachments/assets/ddf4aa9e-ed88-43d0-812b-a5af1c781d22).

---

### 🛡️ 4. Investigating Path Normalization Discrepancies
1. 🔄 **Return to Repeater**: Remove the arbitrary string and add an arbitrary directory, followed by an encoded dot-segment at the beginning of the original path. For example, `/eyes/..%2fmy-account`.
2. Send the request: ![image](https://github.com/user-attachments/assets/28f7ee83-1d0c-49ba-acb1-adae7ab89da9). Note that it receives a 200 response with your API key. This means the origin server decodes and resolves the dot-segment, interpreting the URL path as `/my-account`.
3. In Proxy > HTTP history, notice that all paths for static resources start with the directory prefix `/resources`. Note that responses to requests with the `/resources` prefix show signs of caching.
4. Right-click the request with the `/resources` prefix and select "Send to Repeater."
5. In Repeater, add the encoded dot-segment after the `/resources` prefix in the path, for example `/resources/..%2feyes`.
6. Send the request. Note that the 404 response contains the X-Cache: miss header: ![image](https://github.com/user-attachments/assets/af167914-d73b-4588-92bd-6211f71ddc87).
7. Repeat the request. Note that the value of the X-Cache header changes to hit. This confirms that there exists a static caching rule for directories based on the `/resources` prefix: ![image](https://github.com/user-attachments/assets/ac4b10be-402b-4eb6-a591-d169a82f8044).

---

### 🗝️ 5. Obtaining the API Key
Determine the API key for user **carlos**.
1. 🛠️ In the Body section, create an exploit that redirects the victim user carlos to a malicious URL. Be sure to add an arbitrary parameter as a cache-buster to ensure the victim does not receive your previously cached response: <script>document.location="https://YOUR.LAB.LINK/resources/%2e%2e%2fmy-account?eyes"</script>
2. 🚀 Click "Deliver Exploit to Victim". When the victim views the exploit, the response is cached.
3. 🌐 Go to the URL specified in your exploit: 
   `https://YOUR.LAB.LINK/PREFIX/resources/%2e%2e%2fmy-account?eyes`.
4. 🔑 The response includes the API key for **carlos**:
   ![API Key for carlos](https://github.com/user-attachments/assets/6e9c4f5b-5c3c-4ba3-a2e0-39685da71337).

---

## 💡 Conclusion
As a result of completing this lab work, you will be able to access protected resources using skills in path matching and cache bypassing. This knowledge will be useful in your future projects and career. 🚀
