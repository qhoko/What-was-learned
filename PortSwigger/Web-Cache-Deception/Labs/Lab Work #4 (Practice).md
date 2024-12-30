# 🌐 Lab Work: Using Server Normalization for Web Cache Deception 😎

---

## 📜 Description
In this lab work, we will learn how to exploit normalization discrepancies between the web cache and the origin server to obtain the API key for the user **carlos**.

---

## 🎯 Objectives
- 👉 Familiarize yourself with path matching and its use for accessing resources.
- 👉 Explore the technical aspects of web caching and the possibilities for bypassing it.

---

## 🔑 Credentials
To log into the account, use the following credentials:
- **Username:** `wiener`
- **Password:** `peter`

---

## 📚 Theoretical Basis

### 🛤️ URL Path Matching
- 🔍 **API endpoints** may use regular expressions to match URL paths to resources.
- 🔍 It is important to understand how different paths can be processed differently depending on cache and server configurations.

### 🔄 Caching and Its Discrepancies
- 🔄 The web cache and server may have various mechanisms for handling the same requests.
- 🔄 Bypassing the cache is done by modifying the request path to access uncached data on the server.

---

## 📋 Steps to Execute

### 🔐 1. Logging In
Log in using the provided credentials to view your **API key**:
![API Key](https://github.com/user-attachments/assets/a1cecc2a-c2b2-47dc-8f05-37fb9b5611a3)

---

### 🔍 2. Analyzing Endpoints
Explore the available API endpoints to determine how they are handled by the server.
1. 🔍 **Pay attention to paths** that may be vulnerable to bypassing. Launch **Burp Suite** and capture the request by refreshing the page at `/my-account`.
2. ✉️ **Send the request to Repeater** and change the path to an arbitrary one, for example, `/my-account/eyes`:
   ![Change Path](https://github.com/user-attachments/assets/4cb281a1-eb8d-4967-ae98-adb9e89ea9b8). Note the 404 error.
3. Remove the arbitrary segment and add an arbitrary string to the original path. For instance, change the path to `/my-accounteyes`:
   ![Image](https://github.com/user-attachments/assets/fcf4a0a5-0b38-4d11-8b19-20062a4b4ee3). This will serve as reference material.

---

### 🔄 3. Using Delimiters in Intruder
Let’s explore how delimiters affect the website's behavior.
1. Right-click on the request and select "Send To Intruder" (or use Ctrl + I).
2. 📄 **Specify the payload location**:
   ![Image](https://github.com/user-attachments/assets/f6128369-516e-44b4-a804-e145f593ff38).
3. Uncheck "URL-encode this characters."
4. 📊 **Add a list of characters**: In the "Payload Configuration" section, add a list of characters that can be used as delimiters. 
   Copy the list from here: [Character List](https://portswigger.net/web-security/web-cache-deception/wcd-lab-delimiter-list).
   ![Image](https://github.com/user-attachments/assets/f109678c-1d5f-4604-8970-97562d595b7c)
5. Click "Start attack."
   In the output, you will see a code 200, indicating that only 4 delimiters are processed: `"#"`, `"?"`, `"%23"`, `"%3f"`:
   ![Image](https://github.com/user-attachments/assets/06664df0-0847-4509-8ca8-a4aca2c1e78f).
6. This means they are used by the origin server as path delimiters. Ignore the `#` symbol; it cannot be used in the exploit as the victim's browser will treat it as a delimiter before forwarding the request to the cache.

---

### 🛡️ 4. Investigating Path Delimiter Discrepancies
1. Go to the Repeater tab containing the `/my-accountabc` request. Add a `?` symbol after `/my-account` and append a static extension to the path. For example, change the path to `/my-account?eyes.js`.
2. Send the request. Note that the response does not contain indications of caching. This either suggests that the cache also uses `"?"` as a path delimiter or that the cache does not have a rule based on the `.js` extension.
3. Repeat this test using the symbols `"%23"` and `"%3f"` instead of `"?"`. Observe that the responses do not show signs of caching.

---

### 🗝️ 6. Obtaining the API Key
Determine the API key for the user **carlos**.
1. Navigate to the Repeater tab that contains the `/eyes/..%2fmy-account` request. Use the `"?"` delimiter to attempt to create an exploit like this: `/my-account?%2f%2e%2e%2fresources`.
2. Send the request. Note that you receive a 200 response with your API key, but it does not contain indications of caching.
3. Repeat this test using the symbols `%23` and `%3f` instead of `"?"`. Note that when using the `%23` symbol, it receives a 200 response with your API key and the `X-Cache: miss` header. Repeating the send shows it updates to `X-Cache: hit`. You can use this delimiter for the exploit.
4. In Burp's browser, click on "Go to the exploit server."
5. In the Body section, create an exploit that redirects the victim user carlos to a malicious URL. Be sure to add an arbitrary parameter as a cache-buster: <script>document.location="https://YOUR.LAB.LINK/my-account%23%2f%2e%2e%2fresources?eyes"</script>
6. Click "Deliver Exploit to Victim."
7. Visit the URL you specified for carlos in your exploit: `https://YOUR.LAB.LINK/my-account%23%2f%2e%2e%2fresources?eyes`.
8. Note that the response includes the API key for the user carlos. Copy this.
   ![API Key for carlos](https://github.com/user-attachments/assets/75ecde4d-d3e8-4dfe-a42d-8083b51c230d).

---

## 💡 Conclusion
As a result of completing this lab work, you will be able to access protected resources using skills in path matching and cache bypassing. This knowledge will be useful in your future projects and career. 🚀
